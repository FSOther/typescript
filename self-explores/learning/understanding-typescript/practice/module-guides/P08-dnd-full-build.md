# P08 — DnD Full Build

**Practice module** | Mastery target: L3→L5 | ~3 hours (split into 3 sessions)

---

## 1. Build Task

> **No solution during build.** `app.ts` exists — do NOT open it until Fix It stage.

**Objective:** Build the complete drag-and-drop project manager from a written spec. This is the capstone of Practice mode — all patterns simultaneously.

**Spec:**

Build `my-app.ts` — a single-file TypeScript project that renders a project management UI:

**Data Model:**
- `Project` class with: `id: string`, `title: string`, `description: string`, `people: number`, `status: 'Active' | 'Finished'`
- `Listener<T>` type alias and `State<T>` base class (generic observer from P04)
- `ProjectState` singleton extending `State<Project>` (from P04)

**Interfaces:**
- `Draggable`: `dragStartHandler(event: DragEvent): void`, `dragEndHandler(event: DragEvent): void`
- `DragTarget`: `dragOverHandler(event: DragEvent): void`, `dropHandler(event: DragEvent): void`, `dragLeaveHandler(event: DragEvent): void`
- `Validatable`: same as P01

**Base Class:**
- `abstract Component<T extends HTMLElement, U extends HTMLElement>` (from P03)

**Components (implement all 3):**

1. `ProjectItem extends Component<HTMLUListElement, HTMLLIElement> implements Draggable`:
   - `persons` getter: returns `'1 person'` or `'N persons'`
   - `dragStartHandler`: sets drag data as project ID, effectAllowed = `'move'`
   - `dragEndHandler`: logs 'DragEnd'
   - `renderContent()`: fills h2 (title), h3 (persons + ' assigned'), p (description)

2. `ProjectList extends Component<HTMLDivElement, HTMLElement> implements DragTarget`:
   - Takes `'active' | 'finished'` in constructor
   - Registers listener on `projectState` to filter and render relevant projects
   - `dragOverHandler`: adds `'droppable'` class to ul if text/plain
   - `dropHandler`: calls `projectState.moveProject` with correct status
   - `dragLeaveHandler`: removes `'droppable'` class
   - `renderProjects()`: clears and re-renders `ProjectItem` for each project

3. `ProjectInput extends Component<HTMLDivElement, HTMLFormElement>`:
   - Gets `titleInputElement`, `descriptionInputElement`, `peopleInputElement` via querySelector
   - `gatherUserInput()`: validates all 3 fields, returns tuple `[string, string, number]` or void
   - `clearInputs()`: resets all fields
   - `submitHandler()`: decorated with `@autobind`, calls addProject on valid input

**Entry point (bottom of file):**
```typescript
const prjInput = new ProjectInput();
const activePrjList = new ProjectList('active');
const finishedPrjList = new ProjectList('finished');
```

**Constraint:** No `any`. Use `@autobind` for all event handlers. `strict: true` must pass.

---

## 2. Break It

After completing the build (or after giving up and reading `app.ts`):

1. Remove `@autobind` from `dragOverHandler` in `ProjectList` — drag a project over the list. What breaks at runtime? Does TypeScript catch this?

2. Remove the `project.status !== newStatus` check in `moveProject` — drag a project to the same list it's already in. What happens to the listener count? Why is this inefficient?

3. Change `listenerFn(this.projects.slice())` to `listenerFn(this.projects)` — then modify the array inside a listener. What happens to state?

4. Remove the `event.preventDefault()` in `dragOverHandler` — what happens to the drag-and-drop behavior? (This is a browser behavior, not TypeScript.)

---

## 3. Fix It

> **Hints only — no solution shown.**

This is from a student implementation with 3 bugs:

```typescript
class ProjectList extends Component<HTMLDivElement, HTMLElement>
  implements DragTarget {
  
  @autobind
  dropHandler(event: DragEvent) {
    const prjId = event.dataTransfer.getData('text/plain'); // Error 1
    projectState.moveProject(
      prjId,
      this.type === 'active' ? 'Finished' : 'Active' // Error 2: inverted
    );
  }
  
  private renderProjects() {
    const listEl = document.getElementById(`${this.type}-projects-list`);
    listEl.innerHTML = ''; // Error 3
    for (const prjItem of this.assignedProjects) {
      new ProjectItem(this.element.querySelector('ul').id, prjItem);
    }
  }
}
```

**Hint 1:** `event.dataTransfer` is `DataTransfer | null`. How do you assert it's non-null in a safe way?

**Hint 2:** Re-read the mapping: `'active'` list should move to `'Active'` status — not `'Finished'`.

**Hint 3:** `document.getElementById` returns `HTMLElement | null`. Two options: non-null assertion (if you know it exists) or optional chain.

---

## 4. Explain It

1. Why does `ProjectItem` extend `Component<HTMLUListElement, HTMLLIElement>` (ul as host, li as element) rather than the other way around?

2. The `persons` getter is defined in `ProjectItem` — why a getter instead of a method? What's the difference in usage?

3. `event.dataTransfer!.setData('text/plain', this.project.id)` — the `!` non-null assertion. In what scenario would this actually be null? Is the `!` safe here?

---

## 5. Apply It

Extend the project to support:
1. A "delete" button on each `ProjectItem` that removes the project (add `removeProject` to state)
2. A project count badge showing "N active, M finished" that updates when state changes

These are new features — there's no course code for them. Design and implement from scratch.

---

## 6. Acceptance Criteria

```
Given: complete my-app.ts
When: tsc --strict compiles it
Then: zero type errors

Given: a project dragged from active to finished
When: drop event fires
Then: project appears in finished list, removed from active list

Given: @autobind on submitHandler
When: form submitted (button clicked, not method called directly)
Then: this.titleInputElement is accessible (not undefined)
```

---

## 7. Resources

- Speed guide: all M-series guides relevant
- DnD source: [`resources/179-prj-16-finished/prj-16-finished/src/app.ts`](../../../../../resources/179-prj-16-finished/prj-16-finished/src/app.ts)
- DnD HTML: [`resources/163-index.html/`](../../../../../resources/163-index.html/)
- Reference lectures: L162–L181
