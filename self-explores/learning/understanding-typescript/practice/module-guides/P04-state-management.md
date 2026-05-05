# P04 — State Management (Generic Observer)

**Practice module** | Mastery target: L2→L4 | ~90 min

---

## 1. Build Task

> **No solution provided.** Build from this spec only.

**Objective:** Build the generic `State<T>` observer and `ProjectState` singleton from scratch.

**Spec:**

File `state.ts`:

1. `type Listener<T> = (items: T[]) => void`
   — A callback that receives an array of T whenever state changes.

2. `class State<T>` (NOT abstract — can be instantiated or extended):
   - `protected listeners: Listener<T>[]` — starts as empty array
   - `addListener(listenerFn: Listener<T>): void` — appends to listeners

3. `class Project`:
   - Constructor params: `id: string`, `title: string`, `description: string`, `people: number`, `status: 'Active' | 'Finished'`
   - All params are public

4. `class ProjectState extends State<Project>`:
   - `private projects: Project[]` — starts empty
   - `private static instance: ProjectState` — singleton
   - `private constructor()` — prevent direct instantiation
   - `static getInstance(): ProjectState` — returns the singleton (create on first call)
   - `addProject(title: string, description: string, numOfPeople: number): void`:
     - Creates a new `Project` with random ID (use `Math.random().toString()`)
     - Pushes to `projects`
     - Calls `updateListeners()`
   - `moveProject(projectId: string, newStatus: 'Active' | 'Finished'): void`:
     - Finds project by id
     - Only updates if found AND status is actually different
     - Calls `updateListeners()`
   - `private updateListeners(): void` — calls each listener with a copy (`slice()`) of projects

**Constraint:** Use `'Active' | 'Finished'` as a union type string literal, NOT an enum.

---

## 2. Break It

1. Change `protected listeners` to `private` — then try to access it in `ProjectState.updateListeners()`. What error?

2. Remove the `private constructor()` — then write `new ProjectState()` in two files. What is the design problem (even though it compiles)?

3. Remove `.slice()` from `updateListeners()` — pass `this.projects` directly. Write a listener that mutates the array. Why is the original code's `.slice()` defensive?

4. Change `Listener<T> = (items: T[]) => void` to `Listener<T> = (items: T) => void` — which existing code breaks? Why?

---

## 3. Fix It

> **Hints only — no solution shown.**

```typescript
type Listener<T> = (items: T[]) => void;

class State<T> {
  private listeners: Listener<T>[] = [];  // ERROR: subclass can't access
  
  addListener(fn: Listener<T>) {
    this.listeners.push(fn);
  }
}

class ProjectState extends State<Project> {
  private projects: Project[] = [];
  private static instance: ProjectState;
  
  static getInstance() {
    if (!ProjectState.instance) {
      ProjectState.instance = new ProjectState();
    }
    return ProjectState.instance;
  }
  
  addProject(title: string, desc: string, people: number) {
    this.projects.push(new Project(Math.random().toString(), title, desc, people, 'Active'));
    this.updateListeners();
  }
  
  private updateListeners() {
    for (const fn of this.listeners) {  // ERROR: listeners is private in State
      fn(this.projects.slice());
    }
  }
}
```

**Hint 1:** The access modifier on `listeners` in `State<T>` needs to change. What access level allows subclasses but not external code?

**Hint 2:** The constructor is not private — `new ProjectState()` can be called from anywhere. What keyword prevents this?

---

## 4. Explain It

1. What is the observer pattern? In `State<T>`, who is the subject? Who are the observers? How does `addListener` register observers?

2. Why does `updateListeners` pass `this.projects.slice()` instead of `this.projects`? What problem does this prevent?

3. `ProjectState.getInstance()` uses `ProjectState.instance` (class name) rather than `this.instance`. Why? What does `this` refer to in a static method?

---

## 5. Apply It

Extend `ProjectState` with:
1. `removeProject(projectId: string): void` — removes a project and notifies listeners
2. `getProjectCount(): number` — returns number of active projects
3. A second state class `UserState extends State<User>` with similar singleton pattern

Write listeners that react to both state changes and log to console.

---

## 6. Acceptance Criteria

```
Given: ProjectState.getInstance() called twice
When: results compared with ===
Then: both references are the same object (singleton)

Given: a listener added via addListener
When: addProject is called
Then: listener is called with a copy (not reference) of the projects array

Given: moveProject called with same status as current
When: state is inspected
Then: listeners were NOT called (no redundant updates)

Given: ProjectState constructor called directly (new ProjectState())
When: TypeScript compiles
Then: compile error "Constructor of class 'ProjectState' is private"
```

---

## 7. Resources

- Speed guide: [`M07-generics.md`](../../../speed/module-guides/M07-generics.md) (State<T> pattern)
- DnD code: [`resources/179-prj-16-finished/prj-16-finished/src/app.ts`](../../../../../resources/179-prj-16-finished/prj-16-finished/src/app.ts) — lines 30–81
- Reference lectures: L100–L109 (generics), L67–L73 (classes)
