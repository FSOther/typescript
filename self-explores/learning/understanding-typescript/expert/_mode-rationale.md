---
type: learning-rationale
mode: expert
course_slug: understanding-typescript
created: 2026-05-06
generated_by: typescript-ep0
depends_on: _learning-rationale.md (A6)
---

# Expert Mode Rationale — Understanding TypeScript

This document explains WHY expert mode is designed the way it is. Every design decision traces back to `_learning-rationale.md`.

---

## 1. Mode Objective

**One-sentence goal:** Build the deep mental models and production instincts that the course explicitly does NOT teach — closing the gap between "can read TS" (Speed mode) and "thinks like a TypeScript expert."

**User profile served:** JavaScript developer who has completed Speed mode (or equivalent), can read and write TypeScript, and now wants to understand WHY the type system works the way it does, recognize anti-patterns before they bite, and apply TypeScript correctly in real team codebases.

**Definition of "done" for expert mode:**
- User can explain each major TS concept in their own words (Feynman test)
- User can identify the 5 most common TypeScript mistakes in code review without prompting
- User understands at least 3 of the expert gaps (testing, module augmentation, monorepo references)
- User can critique the DnD project's architecture and explain what they would change in production

**Why expert mode needs a separate design:**
Speed mode teaches syntax. Expert mode teaches judgment. The same concepts that Speed mode presents ("you can use `as any` when the type system gets in the way") expert mode challenges ("when `as any` is a smell, and what alternatives exist"). Without this inversion, users form beginner habits that accumulate as tech debt.

---

## 2. Top Mistake Traps

The most common TypeScript errors that experienced JS developers make when transitioning to TS. Each is sourced from course lectures + real-world evidence.

| # | Mistake | Root Cause | Evidence in Course | Fix |
|---|---------|-----------|-------------------|-----|
| 1 | **Overusing `as any`** to escape type errors | Impatience with the type system; treating TS like a linter to shut up | L25–L26 (type casting), L88 (unknown vs any): course teaches `as any` as "last resort" but beginners use it as "first resort" | Use `unknown` + type narrowing. `as any` should appear 0 times in new code. |
| 2 | **Using `!` non-null assertion blindly** | Not understanding when runtime values CAN be null | L23 (optional types), M05 (class property initialization): `!` suppresses the compiler but not the runtime NullPointerEquivalent | Use optional chaining `?.` + nullish coalescing `??`. Reserve `!` for cases where you've already narrowed. |
| 3 | **Writing interfaces for everything** instead of `type` aliases | Interface vs type confusion — beginners default to `interface` as "the OOP way" | L18–L22 (interfaces, type aliases): course covers both but doesn't state the modern preference | Use `type` for unions, intersections, mapped types. Use `interface` only when declaration merging or `extends` is needed. |
| 4 | **Generic constraints too narrow** (`<T extends object>` instead of `<T extends Record<string, unknown>>`) | Not understanding what constraints actually do | L100–L109 (generics): course teaches constraints but beginners under-constrain OR over-constrain | Constraint should match the minimum contract the function needs, not the maximum specificity. |
| 5 | **`strictNullChecks` disabled** in tsconfig | Pain avoidance — null errors feel annoying; turning off `strict` feels like "fixing" them | L41–L49 (tsconfig strict): course shows strict=true is recommended but doesn't explain WHY it prevents real bugs | `strict: true` is non-negotiable in new code. Disable it only to migrate existing JS code, then turn it back on module by module. |
| 6 | **Using `any[]` for array types** instead of proper generics | Not connecting array types to generic types | L13–L39 (type system): arrays covered early but generic array notation comes later in M07 | `Array<T>` and `T[]` are equivalent. Never write `any[]` — it defeats the purpose. Use `unknown[]` + narrowing if content type is genuinely unknown. |
| 7 | **Type assertion instead of type guard** | Confusion between compile-time and runtime | M06 (type guards, L88–L99): course covers type guards but beginners reach for `as T` instead | Write runtime type guard functions (`function isUser(x: unknown): x is User`) for values from APIs and user input. Never use `as` on external data. |
| 8 | **Ignoring `readonly`** on class properties and object types | "I'll be careful" mindset | M05 (readonly, L67–L87): course introduces `readonly` but doesn't hammer mutation safety | Add `readonly` to all class properties that should not change after construction. Use `ReadonlyArray<T>` for arrays that are only read. |

---

## 3. Expert Gap Analysis

What this course does NOT teach, sourced from `_learning-rationale.md` Section 6. Expert mode must address these explicitly.

| # | Gap | Priority | Expert Mode Treatment |
|---|-----|----------|----------------------|
| 1 | **Testing TypeScript** (ts-jest, vitest, expect-type) | Critical | Module E-GAP-1: "Testing TS code — what the course skips." Covers vitest configuration, type-level testing with `expect-type`. |
| 2 | **Module augmentation** (`declare module` for req.user, Fastify plugins) | High | Module E-GAP-4: Extends the M16 @types content. Real example: adding `user: User` to Express `Request`. |
| 3 | **Error handling in async TS** (`Result<T, E>` pattern, `neverthrow`) | High | Module E-GAP-12: "Typed error propagation." Shows why `try/catch` with `catch (e: unknown)` is the only correct pattern. |
| 4 | **CI/CD with TypeScript** (`tsc --noEmit` in pipeline) | High | Module E-GAP-13: "Shipping TypeScript." Pipeline configuration, why `tsc --noEmit` is different from `tsc`. |
| 5 | **Monorepo type sharing** (project references, `composite: true`) | Medium | Module E-GAP-3: "Monorepo configuration." Turborepo/nx patterns. |
| 6 | **Generic React components** (`<T extends ...>` in JSX) | Medium | Extends M18 React content. DnD project pattern applied to component libraries. |
| 7 | **Context API typing** (`createContext<T>`, `useContext` non-null) | Medium | Gap noted in `_learning-rationale.md` Q1 — verify coverage in L238–L240. |

**Expert gap coverage principle:** Each expert module introduces one gap using the DnD project or M18/M19 code as the "missing ingredient" — showing specifically WHERE in the course code the gap would matter.

---

## 4. Deliberate Practice Design

Expert mode is structured around a core insight from deliberate practice research: **experts improve by working at the edge of their current competence, not by repeating what they already know.**

### The four practice types used in expert mode

**1. Mental Model Challenges**
Each module starts by presenting a "beginner mental model" and asking the user to identify what's wrong with it before reading the expert version.
- Example: "Beginners think: `interface` and `type` are interchangeable. What's wrong with this?"
- User writes their answer. Expert explanation follows.
- This forces confrontation of existing (often wrong) beliefs rather than passive absorption.

**2. Code Review Drills**
User is shown a TypeScript code snippet with 2–4 mistakes and asked to identify them WITHOUT hints.
- Snippets are constructed from patterns in the DnD project (`resources/179-prj-16-finished/`) and M18/M19 starter code.
- Mistakes range from stylistic (preferring `type` over `interface`) to correctness (`as any` on JSON.parse output).
- This replicates the mental load of real code review.

**3. Architecture Critiques**
User is shown the DnD project's full codebase structure and asked: "What would you change for a production team codebase?"
- Good answers cover: separating domain types into a separate file, adding `readonly` to state, using discriminated unions for project status, adding an error boundary type.
- This exercises the synthesis skill that separates "can write TS" from "can design TS."

**4. Constraint Exercises (No `any` mode)**
User is given a broken TS file with `any` in 3 places and asked to fix it WITHOUT using `any`.
- This exercises `unknown` + type guard, generic constraints, and conditional types as alternatives.
- Based on patterns in `resources/147-decorators-01-starting-setup/` and M06 advanced types.

### Why these four types vs. simply "more exercises"
Speed mode already tests recall. Expert mode tests judgment, which is a different cognitive operation. The four types above map to four cognitive operations:
1. Belief confrontation (mental models)
2. Error recognition (code review)
3. Design synthesis (architecture)
4. Constraint satisfaction (deliberate no-any)

A session that only uses type 1 produces theory without application. A session that only uses type 4 produces anxiety without mental model. The four types are combined in each session.

---

## 5. Red Flags in Course

Patterns the course teaches that expert mode explicitly flags as "correct in isolation, dangerous in production."

| # | Course Pattern | Why It's Taught | Expert Flag | Production Reality |
|---|--------------|----------------|-------------|-------------------|
| 1 | `as any` for escape hatches | Pragmatic: "when you KNOW the type but TS doesn't" | Anti-pattern: use `unknown` + narrow instead | Production TS code with `as any` = tech debt. Each `as any` is a future runtime error waiting to happen. |
| 2 | Using `enum` for named constants | OOP familiarity: "enums group related values" | Prefer `const object as const` | TypeScript enums compile to IIFE objects with reverse mapping and break tree-shaking. `const obj = { A: 'A', B: 'B' } as const` achieves the same with zero runtime overhead. |
| 3 | Class-heavy architecture (DnD singleton, abstract base) | Demonstrates OOP features in TypeScript | Over-engineering for most apps | React + functional components + hooks rarely need classes. The DnD architecture patterns (singleton, observer) are useful for teaching but usually replaced by state management libraries in real apps. |
| 4 | `strictPropertyInitialization: false` temporarily | "Turn it off if you get frustrated" | Short-term solution that stays | Once disabled, class property initialization errors are never found. Enable it from day 1. Use `!` sparingly only for properties initialized in `ngOnInit` or similar lifecycle hooks. |
| 5 | JSON parsing without validation | Covered briefly with `unknown` but no runtime validation shown | Runtime vs compile-time safety | `JSON.parse()` returns `any`. Without runtime validation (Zod, io-ts, or manual type guards), you have no actual type safety on external data. Course introduces Zod in M16 but doesn't connect it to this JSON parsing pattern. |

---

## Known Risks (for completeness — referenced by `/viec hoc`)

| # | Risk | Mitigation |
|---|------|------------|
| 1 | Expert mode feels "theoretical" if user hasn't done Speed mode | Expert mode modules explicitly reference Speed mode concept IDs and refuse to re-teach basics. Pre-requisite check in `/viec hoc` |
| 2 | Red flags contradict what user learned in Speed mode | Each red flag is introduced with: "You learned X in Speed mode. Here's the nuance:" framing. |
| 3 | Gap analysis modules are self-directed (no course content) | Expert mode gap modules point to specific external resources (vitest docs, neverthrow README, React TypeScript Cheatsheet) — not general suggestions. |
| 4 | Deliberate practice exercises are harder than active recall | Module guides explicitly note difficulty: "This is harder than Speed mode. Being stuck for 5 minutes is correct." |
