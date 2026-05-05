---
type: learning-plan
mode: practice
course_slug: understanding-typescript
created: 2026-05-06
generated_by: typescript-7h6
---

# Practice Learning Plan — Understanding TypeScript

**Course:** Understanding TypeScript
**Mode:** Practice (Build→Break→Fix — hands-on pattern literacy)
**Prerequisite:** Speed mode completed (or equivalent)
**Estimated time:** 25–30 hours over 9 modules
**Primary resource:** `resources/179-prj-16-finished/prj-16-finished/src/app.ts`

---

## 1. Overview

Practice mode converts reading fluency (Speed mode) into writing fluency. Every session ends with user code — either built from scratch, broken for analysis, or fixed from error messages.

**What changes from Speed mode:**
- Speed: you read module guides and answer recall questions
- Practice: you write TypeScript code without looking at solutions

**Progression principle:** Mastery Ladder — L1 → L2 → L3 → L4 → L5
- L1: Understand (read the concept and explain it)
- L2: Replicate (implement from spec without peeking at solution)
- L3: Debug (find errors in broken code from error messages only)
- L4: Design (write typed interface for a new feature from description)
- L5: Apply (integrate pattern in a real mini-project)

Practice mode takes you from L2 to L5 for all core TS patterns.

**Tooling setup (required before starting):**
```bash
cd resources/163-prj-00-initial-starting-setup/prj-00-initial-starting-setup/
npm install      # (if package.json exists — check first)
tsc --version    # must be ≥5.x
```

For React practice:
```bash
cd resources/215-starting-project/starting-project/
npm install
```

---

## 2. Module Breakdown

9 practice modules, sequenced by concept dependency.

| # | Module | Concepts | Build Target | Fix Source |
|---|--------|----------|-------------|-----------|
| P01 | Type System in Action | Union, tuple, type guards, unknown | Write `validate()` function without hints | Break `Validatable` interface |
| P02 | tsconfig from Scratch | strict, target, lib, module | Write `tsconfig.json` from memory | Fix tsconfig with wrong settings |
| P03 | Classes & Interfaces | Abstract, readonly, implements | Build `Component<T, U>` base class | Break class with wrong access modifiers |
| P04 | State Management | Generic observer, singleton | Build `State<T>` + `ProjectState` | Fix broken `Listener<T>` type |
| P05 | Advanced Types | Discriminated unions, type guards | Write `moveProject` with exhaustiveness check | Fix narrowing bug in status check |
| P06 | Generics Hands-on | Constraints, utility generics | Write `Partial<T>` from scratch (no lib.d.ts) | Fix over-constrained generic |
| P07 | Decorators in Practice | `@autobind`, PropertyDescriptor | Write `@autobind` from scratch | Fix decorator compiled at wrong stage |
| P08 | DnD Full Build | All patterns simultaneously | Build complete `app.ts` from spec | N/A (Build is the challenge) |
| P09 | React + Node Capstone | Props, events, typed routes | Build typed CRUD API + React form | Fix Express route type errors |

---

## 3. Build→Break→Fix Guide

Each stage has specific rules. Not following the rules eliminates the learning.

### Build Stage (3+ steps per challenge)

**What it is:** User implements TypeScript from a written specification — no starter code.

**Step 1 — Read the spec completely before writing any code.**
The spec describes what the code must do, what types it must accept, and what the acceptance criteria are. Read the entire spec before starting. Do not start with "I'll figure it out as I go."

**Step 2 — Write the type/interface definitions first.**
Before writing any function body or class implementation, define all types, interfaces, and function signatures. This forces you to think about the type graph before the logic.
Example: If building `Validatable` interface, write the interface first:
```typescript
interface Validatable {
  value: string | number;
  required?: boolean;
  // ... rest of spec
}
```

**Step 3 — Implement the logic, run tsc, fix compilation errors before checking solution.**
Write the implementation, run `tsc` (or `tsc --noEmit`), fix any errors using only the error messages — no IDE tooltips, no looking at solutions. Only after `tsc` shows zero errors do you check the solution.

**Step 4 — Compare against solution (resources/ path provided in module guide).**
After your implementation compiles, compare against the reference solution. Note: different from the solution is NOT wrong — but understand WHY your approach differs.

**Step 5 — Submit evidence to `/viec hoc tra-loi`.**
Evidence = `tsc` output showing zero errors + description of what you built. `/viec hoc` records this and marks the Build challenge done.

### Break Stage (3+ steps per challenge)

**What it is:** User is given a modified resource file with 2–4 deliberate type errors and must find all of them WITHOUT running `tsc`.

**Step 1 — Read the broken file completely before starting.**
Note all type annotations, interfaces, and class structures. Build a mental model of what the code SHOULD look like.

**Step 2 — Find and list all errors from mental analysis only.**
Work through the code systematically. For each potential error, write: "Line X: I think Y is wrong because Z." Do this for ALL suspected errors before verifying any.

**Step 3 — Run `tsc` to verify your findings.**
Check how many errors you found vs how many exist. Note any you missed — these reveal gaps in your mental model.

**Step 4 — Explain the root cause of each error.**
Not "the type is wrong" but "the type is wrong because `string` is not assignable to `ProjectStatus` — `ProjectStatus.Active` is an enum value, not a string."

**Step 5 — Submit to `/viec hoc tra-loi` with your findings.**
List the errors you found (before running tsc), errors you missed, and your root cause explanation.

### Fix Stage (3+ steps per challenge)

**What it is:** User receives only a `tsc` error message and must fix the code WITHOUT seeing the full file context.

**Step 1 — Read the error message fully.**
`TSxxxx: Description at file.ts:line:col`. The error code, message, and location all contain useful information. Read all of it.

**Step 2 — Identify the error type from the message.**
Common types: TS2345 (argument type mismatch), TS2339 (property doesn't exist), TS2322 (type not assignable), TS2304 (cannot find name), TS2416 (property not assignable to base).

**Step 3 — Write the fix from the error message alone.**
Without looking at the file, write what you THINK the fix is based on the error message. Example: `TS2339: Property 'persons' does not exist on type 'Project'` → fix = add `persons` property to `Project` class OR use `ProjectItem.persons` (getter) instead.

**Step 4 — Verify by looking at the relevant section of the file.**
Check if your fix is correct. If not, understand why.

**Step 5 — Submit to `/viec hoc tra-loi`** with your reasoning and fix.

---

## 4. Progression Rules

**Hard rule: No advancing without meeting the criteria.**

These rules exist because "I understand it conceptually" and "I can implement it" are different skills. Practice mode enforces the difference.

| Level | Advancement Criteria | Evidence Required |
|-------|---------------------|-----------------|
| L1 → L2 | Can explain the concept (Speed mode requirement) | Active Recall passed in Speed mode |
| L2 → L3 | Build challenge compiles with zero errors | `tsc --noEmit` output showing 0 errors |
| L3 → L4 | Break challenge: found ≥2/3 planted errors WITHOUT running tsc first | Submitted error list before verification |
| L4 → L5 | Design challenge: types correct, no `any`, passes AC | Full implementation review via `/viec hoc` |

**The "struggle time" rule:**
Being stuck for up to 10 minutes on a Build or Fix challenge is CORRECT. This is deliberate practice. If stuck >10 minutes:
1. Make your best attempt (even if wrong)
2. Submit it with `/viec hoc tra-loi`
3. Get feedback
4. Do NOT look at the solution until after submitting

**The "no peeking" rule:**
During Build challenges, the reference solution (`resources/NNN-*/`) is OFF LIMITS until after you've achieved `tsc` zero errors. Looking at the solution during the attempt eliminates the primary learning event.

Exception: You MAY look at the solution if you are completely blocked after 15 minutes AND have submitted your attempt to `/viec hoc`. This is the "give up with reflection" path — still valuable.

**Module progression requirement:**
Must complete L2 (Build) before attempting L4 (Design) for the same concept area. Cannot skip levels.

---

## 5. Resource Usage

Key resources used in each practice module. These are the actual files from the course.

### Primary resources

**Full DnD project (reference solution):**
`resources/179-prj-16-finished/prj-16-finished/src/app.ts` — 376 lines
Used for: Architecture critique, code review drills, Break challenges (modified versions), Build challenge comparison

**DnD project starting point:**
`resources/163-prj-00-initial-starting-setup/prj-00-initial-starting-setup/` — empty HTML
Used for: P08 Full DnD Build challenge starting point

**Decorator validation reference:**
`resources/158-decorators-10-decorator-validation/` 
Used for: P07 decorator challenges — Break stage source

**`@autobind` reference:**
`resources/156-decorators-09-example-autobind/`
Used for: P07 Build challenge — implement `@autobind` without looking at this file

**React starter:**
`resources/215-starting-project/starting-project/` (if exists)
Used for: P09 React capstone challenges

### How to use resources correctly

**Rule 1 — Resources are AFTER the attempt, not during.**
`resources/NNN-*` files are reference solutions. Open them to compare AFTER you've built your version, not to copy from.

**Rule 2 — Resources/ paths in module guides are navigation aids.**
When a module guide says "reference: `resources/173-prj-11-inheritance-and-generics/`", it means: "This is the state of the code at lecture 173. After you've built the challenge, compare against this."

**Rule 3 — Incremental resources show the build process.**
The DnD project has 16 incremental versions (`resources/163-*` through `resources/179-*`). If completely stuck on a Build step, you can look at the PREVIOUS incremental version (not the final version) for a hint about where to start.

**Rule 4 — Type check resources before using as reference.**
Resource files may have compile errors depending on tsconfig settings. Always verify with `tsc --noEmit` before using as your reference. If a resource doesn't compile, note it as a known limitation (it may be intentionally broken for the exercise, or it may predate modern TS).
