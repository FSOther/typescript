# P03 — Classes & Interfaces

**Practice module** | Mastery target: L2→L4 | ~90 min

---

## 1. Build Task

> **No solution provided.** Build from this spec — no looking at `app.ts`.

**Objective:** Build the `Component<T, U>` abstract base class from scratch.

**Spec:**

Create `base-component.ts` with an abstract class `Component<T extends HTMLElement, U extends HTMLElement>`:

Properties (all `public`):
- `templateElement: HTMLTemplateElement`
- `hostElement: T`
- `element: U`

Constructor params (in order):
- `templateId: string` — ID of the `<template>` element
- `hostElementId: string` — ID of the element to attach to
- `insertAtStart: boolean` — whether to insert at beginning or end
- `newElementId?: string` — optional ID for the created element

Constructor behavior:
1. Get template element via `document.getElementById(templateId)`, cast to `HTMLTemplateElement`
2. Get host element via `document.getElementById(hostElementId)`, cast to `T`
3. Import template content with `document.importNode`
4. Get `firstElementChild` of the imported node, cast to `U`
5. If `newElementId` is provided, set it as the element's `id`
6. Call `this.attach(insertAtStart)`

Private method `attach(insertAtBeginning: boolean)`:
- Uses `insertAdjacentElement` on `hostElement`
- Uses `'afterbegin'` if insertAtBeginning, `'beforeend'` otherwise

Abstract methods (no implementation here):
- `configure(): void`
- `renderContent(): void`

**Constraint:** Use non-null assertions (`!`) only where necessary — can you reduce them by restructuring?

---

## 2. Break It

1. Remove `abstract` from the class — then try to instantiate `new Component(...)` directly. What error?

2. Remove the `T extends HTMLElement` constraint — then access a property that only exists on `HTMLElement` inside the constructor. What changes?

3. Change `attach` from `private` to `public` — no compile error. But why is this a design problem?

4. Remove the `!` from `document.getElementById(templateId)!` with `strictNullChecks: true`. What TypeScript error appears?

---

## 3. Fix It

> **Hints only — no solution shown.**

```typescript
abstract class Component<T, U> {  // missing constraints
  template: HTMLTemplateElement;
  host: T;
  el: U;

  constructor(tplId: string, hostId: string, atStart: boolean, elId: string) {
    this.template = document.getElementById(tplId);  // error 1
    this.host = document.getElementById(hostId);     // error 2
    const node = document.importNode(this.template.content, true);
    this.el = node.firstElementChild;               // error 3
    this.el.id = elId;                              // error 4 (optional id)
    this.attach(atStart);
  }

  private attach(atStart: boolean) {
    this.host.insertAdjacentElement(atStart ? 'afterbegin' : 'beforeend', this.el);
  }
}
```

**Hint 1:** `document.getElementById` returns `HTMLElement | null`. Two ways to fix: non-null assertion or conditional check.

**Hint 2:** `firstElementChild` returns `Element | null`. Same choice.

**Hint 3:** The `elId` should be optional. What TypeScript syntax marks a parameter as optional?

**Hint 4:** Generic type constraints are missing — what constraints make DOM method calls valid?

---

## 4. Explain It

1. Why must `T extends HTMLElement` (not just `T`)? What specific method/property on `T` requires this constraint?

2. `document.importNode(node, true)` — what does the second argument `true` do? What would happen with `false`?

3. The `attach` method is called in the constructor. In JavaScript, `this` in constructors is the new instance. What could go wrong if a subclass's `configure()` is called in the constructor before the subclass is fully initialized?

---

## 5. Apply It

Extend `Component` with a concrete `Header` component:
- `Header extends Component<HTMLDivElement, HTMLElement>`
- Template ID: `'header-template'`
- Host ID: `'app'`
- No new element ID needed
- `configure()`: adds click listener to the element
- `renderContent()`: sets `h1` text inside the element

Write the full implementation including the HTML template structure that would make this work.

---

## 6. Acceptance Criteria

```
Given: Component class defined as abstract
When: new Component(...) is called directly
Then: compile error "Cannot create an instance of an abstract class"

Given: Component<HTMLDivElement, HTMLFormElement>
When: accessing this.hostElement.style
Then: no error (HTMLDivElement has style from HTMLElement)

Given: optional newElementId not provided
When: constructor runs
Then: no runtime error (conditional check or optional chain handles it)
```

---

## 7. Resources

- Speed guide: [`M05-classes-interfaces.md`](../../../speed/module-guides/M05-classes-interfaces.md)
- DnD code: [`resources/179-prj-16-finished/prj-16-finished/src/app.ts`](../../../../../resources/179-prj-16-finished/prj-16-finished/src/app.ts) — lines 143–180
- Reference lectures: L67–L87
