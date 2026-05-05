---
type: module-guide
mode: speed
module: M05
course_slug: understanding-typescript
lectures: L67-L87
sessions: S04-S05
created: 2026-05-06
---

# M05 — Classes & Interfaces (L67–L87)

## 1. Key Concepts

TypeScript classes add type-safety to JavaScript classes. The key additions over JS: access modifiers, readonly, abstract, and implements.

**Access modifiers:**
- `public` (default) — accessible everywhere
- `private` — only within the class (compile-time only, not runtime)
- `protected` — within the class and subclasses
- `readonly` — set once (in constructor or at declaration), never changed

**Shortcut constructor syntax:**
```typescript
class Product {
  constructor(
    public readonly id: string,
    private name: string,
    protected price: number
  ) {}
}
// Equivalent to declaring id, name, price as class fields + assigning in constructor body
```

**Abstract classes:**
- Cannot be instantiated directly
- Define structure (abstract methods/properties) that subclasses MUST implement
- Can contain concrete implementations too (unlike interfaces)

**Interfaces:**
- Define a "shape" contract (properties + method signatures)
- Classes can `implement` multiple interfaces
- Interfaces are erased at compile time — zero runtime cost
- Use `interface` for object shapes and function types in APIs

**Interface vs type alias:**
- Both define types. Main difference: interfaces can be extended with `extends` and support declaration merging
- Prefer `type` for unions and intersections. Prefer `interface` for object shapes that need `extends` or declaration merging.

## 2. Code Patterns

**Abstract base class:**
```typescript
abstract class Component<T extends HTMLElement, U extends HTMLElement> {
  templateElement: HTMLTemplateElement;
  hostElement: T;
  element: U;

  constructor(templateId: string, hostElementId: string) {
    this.templateElement = document.getElementById(templateId)! as HTMLTemplateElement;
    this.hostElement = document.getElementById(hostElementId)! as T;
    const node = document.importNode(this.templateElement.content, true);
    this.element = node.firstElementChild as U;
  }

  abstract configure(): void;
  abstract renderContent(): void;
}
```

**Interface with implements:**
```typescript
interface Draggable {
  dragStartHandler(event: DragEvent): void;
  dragEndHandler(event: DragEvent): void;
}

class ProjectItem extends Component<HTMLUListElement, HTMLLIElement>
  implements Draggable {
  
  dragStartHandler(event: DragEvent) {
    event.dataTransfer!.setData('text/plain', this.project.id);
  }
  
  dragEndHandler(_: DragEvent) {}
}
```

**Singleton pattern:**
```typescript
class ProjectState {
  private static instance: ProjectState;
  
  private constructor() {}
  
  static getInstance(): ProjectState {
    if (!this.instance) {
      this.instance = new ProjectState();
    }
    return this.instance;
  }
}

const state = ProjectState.getInstance();
```

**Getter:**
```typescript
class ProjectItem {
  private project: Project;
  
  get persons(): string {
    return this.project.people === 1 
      ? '1 person' 
      : `${this.project.people} persons`;
  }
}
```

## 3. Common Gotchas

**Gotcha 1: `private` is compile-time only**
TypeScript's `private` keyword is not truly private at runtime — it compiles to a regular property. For runtime privacy, use JavaScript's `#field` private fields syntax.

**Gotcha 2: Abstract classes cannot be instantiated**
```typescript
const component = new Component(...); // ERROR: Cannot create instance of abstract class
```
Abstract classes are templates. You instantiate their concrete subclasses.

**Gotcha 3: Interface declaration merging**
You can define the same interface twice and TypeScript merges the definitions. This is useful for extending third-party types but can cause confusion:
```typescript
interface Window { myCustomProp: string; } // extends existing Window interface
```

**Gotcha 4: `implements` vs `extends`**
- `extends` = subclass (inherits implementation)
- `implements` = promises to implement the interface's API (no inheritance, just contract)
A class can `extends` one class but `implements` multiple interfaces.

**Gotcha 5: Access modifier shortcut vs. explicit declaration**
Both work, but constructor shortcut parameters cannot have default values that reference `this`. If you need that, use explicit field declarations.

## 4. Active Recall Questions

*(No answers here — answer from memory.)*

**Q1:** What is the difference between `extends` and `implements` in TypeScript? When would you use each?

**Q2:** Write a TypeScript class with: a `private` field, a `readonly` field, a `protected` field, and a `get` accessor — from memory without IDE help.

**Q3:** Why can't you instantiate an abstract class? What is the purpose of abstract methods?

## 5. Quick Reference

| Keyword | Purpose |
|---------|---------|
| `public` | Accessible everywhere (default) |
| `private` | Class-only access |
| `protected` | Class + subclass access |
| `readonly` | Set once, never changed |
| `abstract class X` | Template — cannot instantiate directly |
| `abstract method()` | Must be implemented by concrete subclass |
| `implements I` | Class commits to interface contract |
| `extends C` | Subclass inherits from C |
| `get prop()` | Getter — no parentheses when accessed |

## 6. Done Criteria

Session complete when you can write from memory:
1. An abstract base class with 2 generic type parameters and 2 abstract methods
2. A concrete subclass that extends it and implements a separate interface
3. A singleton class with private constructor and static `getInstance()`
