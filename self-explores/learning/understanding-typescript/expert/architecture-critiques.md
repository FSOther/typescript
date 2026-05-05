---
date: 2026-05-06
type: architecture-critique
mode: expert
course_slug: understanding-typescript
generated_by: typescript-cue
---

# Architecture Critiques — Understanding TypeScript (DnD Project)

**Source code:** `resources/179-prj-16-finished/prj-16-finished/src/app.ts`

---

## Table 1: Strengths (What the Course Code Gets Right)

| # | Pattern | Where | Why It's Correct |
|---|---------|-------|-----------------|
| S1 | Generic `State<T>` with `Listener<T>` | Lines 30–38 | Separates state management from state shape. Reusable for any type T. Correctly uses `protected` so subclasses can notify listeners. |
| S2 | Abstract `Component<T, U>` with DOM type params | Lines 143–180 | Generic type parameters flow to subclasses — `element` is properly typed as `HTMLFormElement` in `ProjectInput`, not just `HTMLElement`. Prevents cast-everywhere smell. |
| S3 | `Draggable` and `DragTarget` as separate interfaces | Lines 1–11 | Clean interface segregation. A class only implements what it needs. `ProjectItem` implements `Draggable`; `ProjectList` implements `DragTarget`. |
| S4 | `@autobind` decorator for event handlers | Lines 129–140 | Correctly solves the `this`-loss problem without `bind()` at every call site. The `get()` accessor pattern is the right implementation for experimental decorators. |
| S5 | Defensive copy in `updateListeners()` | Line 78 | `.slice()` prevents listeners from mutating shared state. Shows awareness of shared reference bugs. |
| S6 | `persons` getter for formatted display | Lines 187–192 | Clean API — `this.persons` reads like a property, not a method call. Correct use of TypeScript getter syntax. |

---

## Table 2: Weaknesses & Anti-patterns (Production Alternatives)

| # | Anti-pattern | Where | Problem | Production Alternative |
|---|-------------|-------|---------|----------------------|
| W1 | `enum ProjectStatus { Active, Finished }` | Line 14 | Enums compile to IIFE with reverse mapping, adding ~200 bytes per enum. Break tree-shaking. Fail in native Node.js TS mode. | `const ProjectStatus = { Active: 'Active', Finished: 'Finished' } as const; type ProjectStatus = typeof ProjectStatus[keyof typeof ProjectStatus]` |
| W2 | `const projectState = ProjectState.getInstance()` at module level | Line 83 | Module-level singleton is a global — any file importing the module shares state. Impossible to test in isolation. | Dependency injection: pass `IProjectState` as a constructor argument. Test with a fake implementation. |
| W3 | All code in single `app.ts` (376 lines) | Entire file | No separation of concerns. UI, state management, validation, interfaces all coupled. Impossible to test state without DOM. Can't reuse state logic in other contexts. | Separate files: `types.ts`, `state.ts`, `validation.ts`, `components/project-input.ts`, etc. |
| W4 | `document.getElementById(id)!` non-null assertions in constructor | Lines 155–163 | Constructor crashes at runtime if template/host element doesn't exist. No error message. Fails silently in tests that don't set up DOM. | Factory function pattern: `Component.create(...)` returns `Component \| null` and validates elements before constructing. |
| W5 | `as any` type in `@autobind` decorator signature | Line 130 | `(target: any, _2: string, descriptor: PropertyDescriptor)` — `target` typed as `any` suppresses useful errors. | Type the target correctly: `(target: object)` or use ECMAScript decorator spec with proper generic context type. |
| W6 | `Math.random().toString()` for project IDs | Line 58 | Not collision-resistant. In any real app, IDs must be stable, unique, and ideally server-assigned. | `crypto.randomUUID()` (Web Crypto API, available in browsers and Node.js 15+) — returns a standards-compliant UUID. |
| W7 | `project.status !== newStatus` guard in `moveProject` | Lines 68–73 | This is correct, but the intent is unclear — looks like a performance optimization, not a correctness guard. | Add a comment explaining WHY: "Prevent duplicate listener notifications when status hasn't changed" — or use a separate `canMove` check with a meaningful name. |

---

## Table 3: Missing (What Professionals Would Add)

| # | Missing | Why a Pro Would Add It | Example |
|---|---------|----------------------|---------|
| M1 | Interface for `ProjectState` | Enables dependency injection and testing | `interface IProjectState { addProject(...): void; moveProject(...): void; subscribe(...): () => void; }` — `ProjectState` implements this. |
| M2 | Separation of types file | Breaks circular imports, enables tree-shaking | `src/types.ts` with interfaces only — no imports from other app files. Referenced by both `state.ts` and `components/`. |
| M3 | Unsubscribe from listeners | Current code has no way to remove listeners — memory leak for component cleanup | `addListener` returns an unsubscribe function: `addListener(fn): () => void { this.listeners.push(fn); return () => { this.listeners = this.listeners.filter(l => l !== fn); }; }` |
| M4 | Error boundaries on DOM operations | `querySelector('h2')!.textContent = ...` fails silently if template structure changes | Either validate template structure once at setup with meaningful errors, or use defensive `?.` with fallback. |
| M5 | Type-safe DOM query | `this.element.querySelector('ul')!` returns `Element \| null`, cast away | Extract a typed helper: `function qSelect<T extends Element>(el: Element, sel: string): T { const found = el.querySelector<T>(sel); if (!found) throw new Error(...); return found; }` |
| M6 | `tsc --noEmit` in CI | Course shows manual compilation — no automated type checking | `package.json` script: `"typecheck": "tsc --noEmit"` — run in CI alongside tests. Vite doesn't type-check; this must be explicit. |
| M7 | `const enum` → `const object` migration note | Course uses `enum ProjectStatus` — fine for teaching but warned against in production | Document in code: a `// TODO: migrate to const object for tree-shaking` comment or a linting rule (`no-restricted-syntax` for `enum` declarations). |
