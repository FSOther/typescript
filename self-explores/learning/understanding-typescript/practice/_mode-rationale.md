---
type: learning-rationale
mode: practice
course_slug: understanding-typescript
created: 2026-05-06
generated_by: typescript-6uv
depends_on: _learning-rationale.md (A6)
---

# Practice Mode Rationale — Understanding TypeScript

This document explains WHY practice mode is designed the way it is. Every design decision traces back to `_learning-rationale.md`.

---

## 1. Mode Objective

**One-sentence goal:** Convert TypeScript reading fluency (Speed mode) into writing fluency by building, breaking, and fixing real TypeScript code using the course's own resources.

**User profile served:** User has completed Speed mode (can read TS annotations, generics, decorators) and now needs hands-on pattern literacy — the ability to write correct TypeScript from memory, debug type errors without help, and design typed interfaces for a real feature.

**Definition of "done" for practice mode:**
- User can implement the DnD project from scratch without referencing the solution (`resources/179-prj-16-finished/`)
- User can fix a TypeScript type error given only the error message (no hints)
- User can write a Zod validation schema for a given API response shape
- User can type Express routes and middleware without looking at documentation
- User completes the final capstone: a typed CRUD API with React frontend

**Why Build→Break→Fix is the right design:**
Research on deliberate practice (Ericsson, 2016) shows that skill improvement requires operating at the edge of competence, not in the comfort zone. Simply doing more Speed mode exercises only reinforces passive recognition. Build→Break→Fix forces **production** of TypeScript (Build), **analysis** of failure (Break), and **diagnosis** from error messages alone (Fix) — three distinct cognitive operations that Speed mode never exercises.

**Why NOT give solutions:**
The core learning event in Fix happens when the user is stuck for 2–5 minutes and must reason about the type system to find the answer. Providing solutions immediately eliminates this event. Practice mode withholds solutions during the attempt and shows them only after the user submits their answer (to enable comparison).

---

## 2. Build→Break→Fix Rationale

The three operations are not arbitrary — they map to the three situations TypeScript developers encounter daily:

### Build — Creating typed code from a specification
Equivalent to: "We need a `ProjectState` class that manages a list of projects."
- User must translate English spec into TypeScript interfaces, classes, generic types
- No starter code provided (only a spec and the relevant module guide section)
- User's output is evaluated against an Acceptance Criteria checklist
- Example build task: "Implement the `Validatable` interface and `validate` function from `resources/167-prj-05-fetching-user-input-with-validation/`"

**Why Build first:**
Building reveals what the user doesn't know yet. A user who thinks they understand `abstract class Component<T, U>` will discover they don't when they can't write it from a spec. Build tasks create the knowledge gaps that Break and Fix then address.

### Break — Identifying type errors in deliberately broken code
Equivalent to: "Here's a modified version of `app.ts` with 3 type errors. Find them."
- User is given a broken TS file and must identify all errors WITHOUT running `tsc`
- Broken files are derived from actual resource code with targeted mutations
- Error types cover: wrong generic constraints, missing non-null assertion, `as any` misuse, enum vs string confusion
- Example break task: "Find all type errors in this modified version of `ProjectList.configure()`"

**Why Break before Fix:**
Break requires recognizing what IS an error (static analysis skill). Fix requires producing the solution (synthesis skill). Doing Break first calibrates the user's error recognition before asking for synthesis — if a user can't find the error, they can't fix it.

### Fix — Correcting type errors given only the compiler message
Equivalent to: "Your CI pipeline failed with: `TS2345: Argument of type 'string' is not assignable to parameter of type 'ProjectStatus'.` Fix it."
- User sees only the compiler error message (no line highlight, no IDE tooltip)
- This replicates the experience of reading CI/CD output
- User must trace the error to root cause and write the fix
- Example fix task: "Fix the error: `TS2339: Property 'persons' does not exist on type 'Project'.`"

**Why Fix is the hardest:**
Fix without IDE support requires the user to hold the full type graph in their head. This is the operation that separates "can write TS with IDE help" (which the job requires) from "can debug TS in CI output" (which the job ALSO requires). Practice mode targets both.

---

## 3. Key Code Paths

The primary resource paths used in practice mode. These are the actual files users study, modify, and build from.

| Path | Lectures | What It Demonstrates | Practice Use |
|------|----------|---------------------|--------------|
| `resources/179-prj-16-finished/prj-16-finished/src/app.ts` | L162–L179 | Full DnD project: abstract base class, singleton, observer with generics, `@autobind`, drag interfaces | The "reference solution" — Build tasks compare against this. Study for code walkthrough. |
| `resources/163-prj-00-initial-starting-setup/` | L162 | Empty project structure (HTML only) | Starting point for DnD Build challenge |
| `resources/173-prj-11-inheritance-and-generics/` | L173 | State<T> + generic observer pattern | Build Challenge 3: "Add generic Listener<T> to State class" |
| `resources/176-prj-14-draggable-list-item/` | L176 | Draggable interface implementation | Build Challenge 5: "Implement Draggable interface on ProjectItem" |
| `resources/158-decorators-10-decorator-validation/` | L158 | Full decorator-based validation | Fix challenge source: break the decorator signature |
| `resources/156-decorators-09-example-autobind/` | L156 | `@autobind` decorator implementation | Build Challenge: "Write `@autobind` from spec without hints" |
| `resources/215-starting-project/` | L232+ | React + TS starting setup | React practice build challenges |
| `resources/184-prj-16-finished/` | L184 | Alternative DnD version | Cross-check for code walkthrough |

**Why the DnD project (`resources/179-prj-16-finished/`) is the centerpiece:**

From `_learning-rationale.md`: "It's the only project in the course that applies 6+ TS concepts simultaneously."

Specifically, `app.ts` demonstrates:
1. **Generic base class:** `abstract class Component<T extends HTMLElement, U extends HTMLElement>` (line 143) — demonstrates both generic type parameters and abstract members simultaneously
2. **Singleton pattern:** `private static instance: ProjectState` + `static getInstance()` (lines 42–54) — the only correct TS singleton implementation pattern
3. **Generic observer:** `type Listener<T> = (items: T[]) => void` + `class State<T>` (lines 30–38) — shows generic callbacks, a pattern used in every typed event system
4. **Interface segregation:** `Draggable` + `DragTarget` (lines 2–11) — two separate interfaces for drag and drop roles, shows interface segregation principle
5. **`@autobind` decorator:** lines 130–140 — the most commonly copied decorator pattern; requires understanding of PropertyDescriptor and `get()` accessor
6. **Non-null assertions with `!`:** lines 157, 163, 205 — shows when `!` is justified (known DOM query pattern) vs when it's a smell

No other resource in the course combines all 6 simultaneously.

---

## 4. Challenge Design Principles

Rules governing how practice challenges are written and sequenced.

**Principle 1: Mastery Ladder must be respected**
Challenges are sequenced: L1 (Understand via module guide) → L2 (Replicate from spec) → L3 (Debug from error) → L4 (Design from scratch) → L5 (Apply in new context). A user cannot attempt a L4 (Design) challenge without evidence of L2 (Replicate) completion. This prevents the "I watched a tutorial, I can build it" illusion.

**Principle 2: No solution visible during attempt**
Module guides contain Build/Break/Fix tasks WITHOUT solutions. The solution file is a separate `_solution.md` that `/viec hoc` shows only after the user submits their answer. This is not optional — solutions visible during the attempt eliminate the key learning event.

**Principle 3: All challenges reference actual resource files**
Every challenge must point to a specific `resources/NNN-*/` path. "Write a TypeScript class" is a bad challenge. "Implement the `State<T>` class from the spec in `resources/173-prj-11-inheritance-and-generics/` WITHOUT looking at the file" is a good challenge. The resource path is both the validation benchmark and the fallback if the user gives up.

**Principle 4: Error messages must be real compiler output**
Fix challenges must use exact error messages from `tsc` output (TypeScript 5.x format: `TSxxxx: message at file:line:col`). Invented error messages teach users to recognize fake errors. Real messages teach them to recognize actual CI output.

**Principle 5: Every challenge has an AC Given/When/Then**
No challenge is written without an Acceptance Criteria in Given/When/Then format. This makes verification objective — user and `/viec hoc` both know when the challenge is done.

---

## 5. Known Limitations

| # | Limitation | Impact | Mitigation |
|---|-----------|--------|-----------|
| 1 | No runtime execution environment in practice mode | Build challenges can't auto-verify TypeScript compilation | User runs `tsc` locally and shares output. `/viec hoc tra-loi` accepts "tsc output: no errors" as evidence. |
| 2 | DnD project requires browser for visual verification | User can't verify drag-and-drop behavior from terminal alone | Challenge AC specifies: "TypeScript compiles without errors" as the primary criterion. Browser test is secondary ("bonus"). |
| 3 | `resources/` folder paths are git-untracked (seen in `git status`) | Paths may not exist if user hasn't cloned with resources | All challenge descriptions include: "Verify file exists at `resources/NNN-*/` before starting." |
| 4 | React practice (L232+) requires `npm install` setup | React challenges can't be attempted without `resources/215-starting-project/` being initialized | Module guide pre-step: "Run `npm install` in `resources/215-starting-project/` before this module." |
| 5 | Course uses `experimentalDecorators` in M11 but ECMAScript decorators in M10 | `@autobind` in DnD uses experimental decorator semantics — won't compile with `experimentalDecorators: false` | Challenge explicitly states: "Ensure `experimentalDecorators: true` in tsconfig for this challenge." |
