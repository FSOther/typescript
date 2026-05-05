# E10 — DnD Architecture Critique

**Expert module** | Covers: L162–L181 (DnD project) | ~75 min

---

## 1. Core Mental Model

> "The DnD project teaches TypeScript patterns in isolation — each feature is clear and visible. But teaching clarity requires making tradeoffs: all code in one file, mutable singleton state, `enum` for status. These tradeoffs are wrong for production. Knowing which patterns to keep and which to discard is the expert's job."

Expert perspective: read course project code as "here is how this TypeScript feature works" — not "here is how to structure a real app." The architecture decisions are teaching decisions, not production decisions.

---

## 2. Beginner Misunderstanding

> "The DnD project uses all these patterns together, so I should structure my real apps like this."

What beginners copy from course projects:
- Single file for everything
- Singleton state accessed globally
- `enum` (compiles to IIFE + reverse mapping)
- Abstract base class for all components (even when they share no implementation)

These are fine for learning. They're problematic in production.

---

## 3. Professional Concern

> "Our app grew from one file to 50 files but we kept the same `app.ts` structure. Now every component knows about every other component, tests are impossible, and adding a feature requires touching 5 unrelated files."

The DnD project's `ProjectState.getInstance()` is a global singleton. Any component can call it. Any component can modify state. There's no contract between state and UI — they're tightly coupled. In production, this causes the "spaghetti state" problem.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: `enum` in production code**
```typescript
enum ProjectStatus { Active, Finished }
// What does this compile to in JavaScript?
// Why does this break tree-shaking?
// Why is it a problem when the enum is used in a switch that runs in the browser
// but the enum definition is in a shared package?
```

**Trap 2: Singleton state as global**
```typescript
const projectState = ProjectState.getInstance(); // module-level

// Any file that imports this module gets the same instance
// What are two problems this causes for testing?
// How would dependency injection fix this?
```

**Trap 3: `abstract class Component<T, U>` with DOM assumptions**
```typescript
abstract class Component<T extends HTMLElement, U extends HTMLElement> {
  templateElement: HTMLTemplateElement;
  hostElement: T;
  // ...
  constructor(templateId: string, hostElementId: string, ...) {
    this.templateElement = document.getElementById(templateId)! as HTMLTemplateElement;
    // Calls document.getElementById in constructor
  }
}
// What breaks when you try to unit test a subclass?
// What would a testable version look like?
```

---

## 5. Expert Shortcut

**`const object` instead of `enum` (tree-shakable, works in native Node TS mode):**
```typescript
// Instead of:
enum ProjectStatus { Active, Finished }

// Use:
const ProjectStatus = { Active: 'Active', Finished: 'Finished' } as const;
type ProjectStatus = typeof ProjectStatus[keyof typeof ProjectStatus];
// 'Active' | 'Finished'

// In code:
project.status = ProjectStatus.Active; // same usage
if (project.status === ProjectStatus.Finished) { ... } // same narrowing
```

**Separate types file (breaks circular imports, enables testing):**
```typescript
// types.ts — no DOM imports, no business logic
export interface IProject {
  readonly id: string;
  title: string;
  description: string;
  people: number;
  status: 'Active' | 'Finished';
}

export interface IStateManager {
  addProject(title: string, desc: string, people: number): IProject;
  moveProject(id: string, status: IProject['status']): void;
  subscribe(listener: (projects: IProject[]) => void): () => void;
}
```

---

## 6. Real-world Scenario

You need to propose a refactored file structure for the DnD project that:
1. Allows unit testing `ProjectState` without a browser
2. Makes it impossible to call `addProject` without an authenticated user
3. Replaces `enum ProjectStatus` with a tree-shakable alternative
4. Separates rendering from state management

Sketch the folder structure and key interfaces. You don't need to implement everything — the architecture diagram matters more.

---

## 7. Deliberate Drill

**Constraint:** Rewrite `ProjectState` as a **pure function module** (no class, no singleton, no mutable module-level state):

```typescript
// Old: class ProjectState { static getInstance() {...} }
// New: functions that take state as argument and return new state

// Goal: these should be pure (no side effects, no global state)
export function createState(): ProjectStateData { ... }
export function addProject(state: ProjectStateData, title: string, ...): ProjectStateData { ... }
export function moveProject(state: ProjectStateData, id: string, status: ...): ProjectStateData { ... }
```

Show how a React `useState` or a test can use this pure module.

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can explain why `enum` is problematic in production (3 reasons)
- [ ] Know what `as const` object + `typeof` pattern achieves
- [ ] Can describe how to refactor singleton state to be injectable
- [ ] Have critiqued at least 3 architectural decisions in the DnD project

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. `abstract class Component<T, U>` forces all subclasses to extend it. What's one scenario where this would be a problem if you wanted to reuse `renderContent()` in a non-DOM context?

2. The DnD project uses `document.importNode(this.templateElement.content, true)` in the `Component` constructor. Why is calling DOM APIs in a constructor a testing anti-pattern? What's the standard alternative?

3. In production, what would you replace the observer pattern in `State<T>` with? Name two state management solutions (framework-agnostic or React-specific) and explain what each offers over the custom `State<T>` implementation.
