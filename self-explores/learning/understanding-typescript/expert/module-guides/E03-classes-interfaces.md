# E03 — Classes & Interfaces

**Expert module** | Covers: L67–L87 | ~60 min

---

## 1. Core Mental Model

> "An interface defines a contract. A class provides one implementation of that contract. Depending on a class where an interface suffices couples callers to the implementation — they can't be tested with fakes, mocked with different implementations, or swapped."

Expert distinction: `interface` and `type` alias are both structural — they describe shapes. Choose `interface` for public API surface (supports declaration merging, extends). Choose `type` for unions, intersections, and computed types (where `interface` syntax doesn't work).

---

## 2. Beginner Misunderstanding

> "I use `interface` for everything because that's what I see in tutorials."

The hidden cost: `interface` can't express union types, mapped types, or conditional types. `interface Animal = Cat | Dog` doesn't compile. Meanwhile, `type` can do everything `interface` does for structural typing. Know when each is appropriate.

---

## 3. Professional Concern

> "Our service layer couples directly to `ProjectState` class. We can't unit test route handlers without spinning up the full state singleton."

The design smell: concrete class dependency instead of interface dependency. Fix: define `interface IProjectState { addProject(...): void; moveProject(...): void; }` — both tests and production inject different implementations.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: Abstract class vs interface choice**
```typescript
abstract class Animal {
  abstract speak(): string;
  move() { console.log('moving'); }
}
interface IAnimal {
  speak(): string;
  move(): void;
}
// When would you choose abstract class over interface?
// What does abstract class force that interface doesn't?
```

**Trap 2: `implements` doesn't guarantee method existence at runtime**
```typescript
interface Logger {
  log(msg: string): void;
}
class SilentLogger implements Logger {
  log(_msg: string): void {} // noop — valid TypeScript
}
// How does this bite you in tests that verify log was called?
```

**Trap 3: Private constructor + static factory (singleton)**
```typescript
class ProjectState {
  private static instance: ProjectState;
  private constructor() {}
  
  static getInstance() {
    if (!this.instance) this.instance = new ProjectState();
    return this.instance;
  }
}
// Why can't you mock ProjectState in tests?
// What pattern would make it testable?
```

---

## 5. Expert Shortcut

**Dependency injection via constructor (testable design):**
```typescript
interface StateManager {
  addProject(title: string, desc: string, people: number): void;
  onUpdate(listener: (projects: Project[]) => void): void;
}

class ProjectInput {
  constructor(private state: StateManager) {} // inject interface, not class
}

// In tests:
const fakeState: StateManager = { addProject: jest.fn(), onUpdate: jest.fn() };
const input = new ProjectInput(fakeState);
```

**`implements` multiple interfaces:**
```typescript
class ProjectItem implements Draggable, Renderable {
  dragStartHandler(e: DragEvent): void { ... }
  dragEndHandler(e: DragEvent): void { ... }
  render(): HTMLElement { ... }
}
// Single class satisfying multiple contracts — TypeScript verifies all at once
```

**Shorthand constructor initialization:**
```typescript
class Project {
  constructor(
    public readonly id: string,    // this.id = id; readonly
    public title: string,          // this.title = title
    private status: ProjectStatus  // this.status = status; private
  ) {}
}
```

---

## 6. Real-world Scenario

The DnD project uses `abstract class Component<T, U>` as a base for all UI components. The abstract class has `attach()` (private), `configure()` (abstract), and `renderContent()` (abstract).

A team member asks: "Why not make Component an interface? It has no shared implementation besides `attach()`."

How would you answer? What would break or change if Component became an interface? When is abstract class the right choice?

---

## 7. Deliberate Drill

**Constraint:** Refactor this singleton to be interface-injectable and testable — **without changing the external API** (callers still get a `ProjectState` via `getInstance()`):

```typescript
class ProjectState {
  private projects: Project[] = [];
  private listeners: ((items: Project[]) => void)[] = [];
  private static instance: ProjectState;
  private constructor() {}
  static getInstance() { ... }
  addProject(title: string, desc: string, people: number) { ... }
  moveProject(id: string, status: ProjectStatus) { ... }
}
```

Define an interface, make the class implement it, and show how a test would inject a fake.

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can explain when to use `abstract class` vs `interface`
- [ ] Know the difference between `implements` and `extends`
- [ ] Can write a dependency-injectable service with interface + class
- [ ] Understand why private constructor breaks testability

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. `class A implements B, C` — what does TypeScript verify at compile time? What does it NOT verify (that JavaScript doesn't enforce)?

2. What is declaration merging for `interface`? Give an example where this is useful. Why can't `type` aliases do this?

3. In the DnD project, `Component<T, U>` has `protected listeners` — wait, actually it doesn't. `State<T>` does. Why are `State<T>`'s listeners `protected` rather than `private`? What does `protected` enable for `ProjectState extends State<Project>`?
