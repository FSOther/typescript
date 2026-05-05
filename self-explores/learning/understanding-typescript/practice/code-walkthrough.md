---
date: 2026-05-06
type: code-walkthrough
mode: practice
course_slug: understanding-typescript
generated_by: typescript-g3b
---

# Code Walkthrough — Understanding TypeScript Resources

**7 patterns. Actual file paths. Anti-pattern examples included.**

---

## Pattern 1: Generic Observer with Protected Listeners

**Source:** [`resources/179-prj-16-finished/prj-16-finished/src/app.ts`](../../../resources/179-prj-16-finished/prj-16-finished/src/app.ts) — Lines 30–81

```typescript
// PATTERN: generic observer
type Listener<T> = (items: T[]) => void;

class State<T> {
  protected listeners: Listener<T>[] = [];   // protected: subclass can call
  addListener(fn: Listener<T>) { this.listeners.push(fn); }
}

class ProjectState extends State<Project> {
  private updateListeners() {
    for (const fn of this.listeners) {       // accesses protected field
      fn(this.projects.slice());             // .slice() = defensive copy
    }
  }
}
```

**Why `protected` matters:** `State<T>` can't call `this.projects` — it doesn't know what `T` is. `ProjectState` can call `this.listeners` via `protected`. If it were `private`, `updateListeners()` couldn't access it.

**Anti-pattern:**
```typescript
// BAD: listeners as any[] loses type information
class State {
  protected listeners: any[] = [];
  addListener(fn: any) { this.listeners.push(fn); }
}
// No type check on what listener receives — could pass wrong data silently
```

---

## Pattern 2: Abstract Base Class with Generic DOM Type Parameters

**Source:** [`resources/179-prj-16-finished/prj-16-finished/src/app.ts`](../../../resources/179-prj-16-finished/prj-16-finished/src/app.ts) — Lines 143–180

```typescript
abstract class Component<T extends HTMLElement, U extends HTMLElement> {
  templateElement: HTMLTemplateElement;
  hostElement: T;    // typed — could be HTMLDivElement, HTMLUListElement, etc.
  element: U;        // typed — could be HTMLFormElement, HTMLLIElement, etc.

  constructor(templateId: string, hostElementId: string, ...) {
    this.templateElement = document.getElementById(templateId)! as HTMLTemplateElement;
    this.hostElement = document.getElementById(hostElementId)! as T;
    const node = document.importNode(this.templateElement.content, true);
    this.element = node.firstElementChild as U;
    // ...
  }
  abstract configure(): void;
  abstract renderContent(): void;
}

// Usage: type parameters flow to subclass
class ProjectInput extends Component<HTMLDivElement, HTMLFormElement> {
  // this.element is HTMLFormElement — form-specific methods available
  configure() { this.element.addEventListener('submit', ...); }
}
```

**Anti-pattern:**
```typescript
// BAD: no type params — element is HTMLElement, not specific type
class Component {
  element: HTMLElement;
}
class ProjectInput extends Component {
  configure() {
    (this.element as HTMLFormElement).addEventListener('submit', ...); // cast needed every time
  }
}
```

---

## Pattern 3: @autobind Decorator for Event Handler `this` Binding

**Source:** [`resources/156-decorators-09-example-autobind/`](../../../resources/156-decorators-09-example-autobind/) — `app.ts`

```typescript
// PATTERN: autobind fixes this-loss in event handlers
function autobind(_: any, _2: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  return {
    configurable: true,
    get() {
      return original.bind(this);  // 'this' = instance accessing the property
    }
  };
}

class ProjectInput extends Component<HTMLDivElement, HTMLFormElement> {
  configure() {
    this.element.addEventListener('submit', this.submitHandler); // no .bind() needed
  }

  @autobind
  private submitHandler(event: Event) {
    event.preventDefault();
    const input = this.gatherUserInput(); // 'this' correctly refers to ProjectInput
  }
}
```

**Without `@autobind`:**
```typescript
// BAD: this is undefined (strict mode) or global (sloppy mode)
this.element.addEventListener('submit', this.submitHandler); // loses 'this'
// FIX WITHOUT DECORATOR:
this.element.addEventListener('submit', this.submitHandler.bind(this)); // verbose
```

---

## Pattern 4: Drag and Drop via DragEvent DataTransfer

**Source:** [`resources/179-prj-16-finished/prj-16-finished/src/app.ts`](../../../resources/179-prj-16-finished/prj-16-finished/src/app.ts) — Lines 203–295

```typescript
// PATTERN: drag data transfer with type safety
class ProjectItem implements Draggable {
  @autobind
  dragStartHandler(event: DragEvent) {
    event.dataTransfer!.setData('text/plain', this.project.id);
    event.dataTransfer!.effectAllowed = 'move';
  }
}

class ProjectList implements DragTarget {
  @autobind
  dragOverHandler(event: DragEvent) {
    if (event.dataTransfer && event.dataTransfer.types[0] === 'text/plain') {
      event.preventDefault(); // required to allow drop
      this.element.querySelector('ul')!.classList.add('droppable');
    }
  }

  @autobind
  dropHandler(event: DragEvent) {
    const prjId = event.dataTransfer!.getData('text/plain');
    projectState.moveProject(
      prjId,
      this.type === 'active' ? ProjectStatus.Active : ProjectStatus.Finished
    );
  }
}
```

**Key insight:** `event.dataTransfer` is `DataTransfer | null` — the `!` non-null assertion is valid here because `dragover` and `drop` handlers only fire when there's an active drag with dataTransfer. But `event.preventDefault()` MUST be called in `dragover` — without it, `drop` never fires.

---

## Pattern 5: ES Module Split Architecture

**Source:** [`resources/190-modules-03-finished-modules/`](../../../resources/190-modules-03-finished-modules/) — multiple files

```typescript
// models/drag-drop.ts
export interface Draggable { ... }
export interface DragTarget { ... }

// models/project.ts
export enum ProjectStatus { Active, Finished }
export class Project { ... }

// state/project-state.ts
import { Project, ProjectStatus } from '../models/project.js';
import type { Listener } from './state.js';  // type-only import

// components/project-list.ts
import { Component } from './base-component.js';
import type { DragTarget } from '../models/drag-drop.js';  // interface = type-only
```

**Pattern:** Separate files by concern. `models/` = data shapes, `state/` = business logic, `components/` = DOM rendering.

**Anti-pattern (namespace version):**
```typescript
// BAD: all in one namespace — no clear dependency graph
namespace App {
  export interface Draggable { ... }
  export class ProjectState { ... }
  export class ProjectList { ... }
  // Deleting drag-drop.ts won't produce an error if another file imports it
}
```

---

## Pattern 6: Validatable Interface with typeof Narrowing

**Source:** [`resources/168-prj-06-more-elaborate-validation/`](../../../resources/168-prj-06-more-elaborate-validation/) — `app.ts`

```typescript
// PATTERN: type narrowing in validation logic
function validate(validatableInput: Validatable) {
  let isValid = true;
  
  if (validatableInput.minLength != null &&
      typeof validatableInput.value === 'string') {  // narrows to string
    isValid = isValid && validatableInput.value.length >= validatableInput.minLength;
    //                              ^^^^^^ TypeScript knows this is string here
  }
  
  if (validatableInput.min != null &&
      typeof validatableInput.value === 'number') {  // narrows to number
    isValid = isValid && validatableInput.value >= validatableInput.min;
  }
  
  return isValid;
}
```

**Why `!= null` instead of `!== undefined`:** `!= null` matches BOTH `null` AND `undefined` (loose equality). This handles both cases with one check.

**Anti-pattern:**
```typescript
// BAD: no narrowing — TypeScript error
if (validatableInput.minLength != null) {
  isValid = isValid && validatableInput.value.length >= validatableInput.minLength;
  //                                   ^^^^^^ Error: Property 'length' does not exist on type 'string | number'
}
```

---

## Pattern 7: Getter for Computed Display Properties

**Source:** [`resources/179-prj-16-finished/prj-16-finished/src/app.ts`](../../../resources/179-prj-16-finished/prj-16-finished/src/app.ts) — Lines 187–192

```typescript
// PATTERN: getter for formatted property
class ProjectItem extends Component<HTMLUListElement, HTMLLIElement> {
  get persons() {
    if (this.project.people === 1) {
      return '1 person';
    }
    return `${this.project.people} persons`;
  }

  renderContent() {
    this.element.querySelector('h3')!.textContent = this.persons + ' assigned';
    //                                              ^^^^^^^^^ used like a property, not a method
  }
}
```

**Getter vs method:**
- `get persons()` → access as `this.persons` (no parentheses)
- `persons(): string` → access as `this.persons()` (with parentheses)

Use getters for derived values that semantically "belong to" the object. Use methods for operations with side effects or parameters.
