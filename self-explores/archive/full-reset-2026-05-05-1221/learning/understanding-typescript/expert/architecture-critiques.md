---
type: architecture-critiques
course_slug: understanding-typescript
mode: expert
created: 2026-05-04
---

# Architecture Critiques — TypeScript Course Code

Phân tích kiến trúc của code trong course — không chỉ học pattern, mà phải biết khi nào KHÔNG dùng pattern đó.

---

## Critique 1: DnD Project — Class Hierarchy

### What the course does

```typescript
abstract class Component<T extends HTMLElement, U extends HTMLElement> {
  templateElement: HTMLTemplateElement;
  hostElement: T;
  element: U;
  abstract configure(): void;
  abstract renderContent(): void;
}

class ProjectList extends Component<HTMLDivElement, HTMLElement> {
  assignedProjects: Project[] = [];
  constructor(private type: "active" | "finished") {
    super("project-list", "app", type === "active");
    this.configure();
    this.renderContent();
  }
  // ...
}
```

### Critique: What's good

- Generic Component base class avoids repeated DOM wiring code
- Abstract methods enforce interface contract
- `readonly type` via constructor param shorthand — good use of TS features

### Critique: What's problematic

1. **Constructor calling abstract methods (`configure()`, `renderContent()`)** — JavaScript anti-pattern. If a subclass constructor runs before the base constructor sets up state, `configure()` may access uninitialized data.

2. **Two generics `T, U` on Component** — Both are HTMLElement subtypes. But does the caller ever need to distinguish them in the type system? If not, single generic or no generic might be clearer.

3. **Global `ProjectState` singleton** — Tight coupling. Hard to test (can't inject mock state).

4. **Drag-and-drop as interfaces on Component subclasses** — Logical but verbose. React's approach (event handlers as props) is more composable.

### Redesign suggestion

```typescript
// Separate concerns:
// 1. DOM component (pure render)
// 2. State management (separate class/module)
// 3. Drag-drop (behavior mixin or separate handlers)

// Functional component-like approach:
function createProjectList(
  container: HTMLElement,
  state: ProjectState,
  type: "active" | "finished"
): { destroy: () => void } {
  const el = renderTemplate<HTMLElement>("project-list");
  container.appendChild(el);
  
  const unsubscribe = state.subscribe(type, (projects) => {
    renderProjects(el, projects);
  });
  
  setupDragTarget(el, state, type);
  
  return { destroy: () => { el.remove(); unsubscribe(); } };
}
```

---

## Critique 2: Generic LinkedList Project

### What the course does

```typescript
class LinkedList<T> {
  private head: ListNode<T> | null = null;
  
  append(value: T): void { ... }
  prepend(value: T): void { ... }
  // ...
}
```

### What's good

- Proper use of generic `T` — value type is preserved throughout
- `ListNode<T>` correctly typed
- Demonstrates generic classes

### What's missing / could be better

1. **No iterator protocol** — Can't use `for...of` on the list. Should implement `[Symbol.iterator]()`.

2. **No immutable variant** — Production code often needs `ReadonlyLinkedList<T>`.

3. **Error handling** — `delete` on non-existent node silently does nothing. Should it throw?

4. **No comparison function for objects** — `find(value)` uses `===` — won't work for object types.

```typescript
// Better LinkedList with iterator and comparison support
class LinkedList<T> implements Iterable<T> {
  constructor(private comparator?: (a: T, b: T) => boolean) {}
  
  [Symbol.iterator](): Iterator<T> {
    let current = this.head;
    return {
      next(): IteratorResult<T> {
        if (current) {
          const value = current.value;
          current = current.next;
          return { value, done: false };
        }
        return { value: undefined as any, done: true };
      }
    };
  }
}
```

---

## Critique 3: State Management Pattern

### What the course does

```typescript
type Listener<T> = (items: T[]) => void;

class State<T> {
  protected listeners: Listener<T>[] = [];
  
  addListener(listenerFn: Listener<T>) {
    this.listeners.push(listenerFn);
  }
}

class ProjectState extends State<Project> {
  private static instance: ProjectState;
  
  static getInstance() {
    if (!ProjectState.instance) {
      ProjectState.instance = new ProjectState();
    }
    return ProjectState.instance;
  }
}
```

### Critique

**Singleton pattern** makes testing impossible:
```typescript
// How do you test ProjectList with different state?
// You can't replace the singleton!
const list = new ProjectList("active"); // always uses THE instance
```

**Listeners leak** — no way to unsubscribe. In long-running apps this causes memory leaks.

**No type safety on notifications** — all listeners get ALL updates. Can't subscribe to specific events.

### Better pattern: Injectable state with subscriptions

```typescript
interface Unsubscribe { (): void }

class ProjectState {
  private listeners = new Map<string, Set<Listener<Project>>>();
  
  subscribe(type: "active" | "finished", fn: Listener<Project>): Unsubscribe {
    if (!this.listeners.has(type)) this.listeners.set(type, new Set());
    this.listeners.get(type)!.add(fn);
    return () => this.listeners.get(type)?.delete(fn);
  }
}

// Usage — injectable, testable:
class ProjectList {
  constructor(private state: ProjectState, private type: "active" | "finished") {
    this.unsubscribe = state.subscribe(type, this.renderProjects.bind(this));
  }
  destroy() { this.unsubscribe(); }
}
```

---

## Critique 4: Validation Decorator Design

### What the course does

```typescript
interface ValidatorConfig {
  [property: string]: {
    [validatable: string]: string[]
  }
}

const registeredValidators: ValidatorConfig = {};

function Required(target: any, propName: string) {
  registeredValidators[target.constructor.name] = {
    ...registeredValidators[target.constructor.name],
    [propName]: [...(registeredValidators[target.constructor.name]?.[propName] ?? []), "required"]
  }
}
```

### Critique

1. **`target: any`** — loses all type safety
2. **String-based validation rules** — `"required"` is a string, not a type-safe enum
3. **Mutation of global state** — `registeredValidators` is module-level mutable state
4. **No type inference** for validated class

### Better: Type-safe validation decorators

```typescript
// Using ES2023 decorators (different from experimental)
type Validator<T> = (value: T) => string | undefined;

function Required(): PropertyDecorator {
  return (target, propertyKey) => {
    // Register via Reflect.metadata (proper metadata API)
    const existing: Validator<unknown>[] = Reflect.getMetadata("validators", target, propertyKey) ?? [];
    existing.push((v) => v == null || v === "" ? `${String(propertyKey)} is required` : undefined);
    Reflect.defineMetadata("validators", existing, target, propertyKey);
  };
}
```

---

## Critique 5: Module Organization in Course

### What the course shows

All code in one `app.ts` → split into separate files → use namespaces → migrate to ES modules.

### The namespace approach (L186)

```typescript
// drag-drop-interfaces.ts
namespace App {
  export interface Draggable { ... }
  export interface DragTarget { ... }
}

// app.ts
/// <reference path="drag-drop-interfaces.ts" />
```

**This approach is OBSOLETE.** Never use namespaces for new projects. They exist only for legacy code.

### Expert rule

Always use ES modules (`import/export`). The only exception: augmenting global types in `.d.ts` files where you may need `declare global {}`.

```typescript
// ✅ Modern approach
// drag-drop-interfaces.ts
export interface Draggable {
  dragStartHandler(event: DragEvent): void;
  dragEndHandler(event: DragEvent): void;
}

// app.ts
import type { Draggable } from "./drag-drop-interfaces.js";
```

---

## Architecture Decision Framework

When designing a TypeScript system, ask:

| Question | Options | When to choose |
|---|---|---|
| Object shapes | `interface` vs `type alias` | Interface when others extend it; type for unions/intersections |
| Hierarchy | Abstract class vs interface | Abstract class when sharing implementation; interface for pure contracts |
| State management | Singleton vs injectable | Injectable always for testability |
| Generic constraints | `T extends X` vs overloads | Generic when T is used in return type; overloads for multiple specific shapes |
| Null handling | `T \| null` vs optional vs `!` | `T \| null` for "value that may not exist"; optional for "config that has defaults" |
| Validation | Runtime or compile-time | Compile-time for internal data; runtime (Zod) at system boundaries |
