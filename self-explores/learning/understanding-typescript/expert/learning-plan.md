---
type: learning-plan
mode: expert
course_slug: understanding-typescript
created: 2026-05-06
generated_by: typescript-iun
---

# Expert Learning Plan — Understanding TypeScript

**Course:** Understanding TypeScript
**Mode:** Expert (deliberate practice — mental models, mistake traps, architecture critique)
**Prerequisite:** Speed mode completed (or equivalent — can read TS annotations, generics, decorators)
**Estimated time:** 20–25 hours over 12 modules

---

## 1. Expert Mindset Overview

Expert mode is NOT about watching more lectures. It is about building the judgment that separates "can write TS" from "thinks like a TypeScript expert."

**The core mental shift:**
- Beginner asks: "How do I make TypeScript stop showing errors?"
- Expert asks: "What is TypeScript telling me about my design, and how do I fix the design?"

**How expert sessions differ from Speed mode:**
| Speed Mode | Expert Mode |
|-----------|-------------|
| Passive reading + active recall | Mental model confrontation + deliberate drill |
| "What does this syntax do?" | "Why would you choose this over the alternative?" |
| 3 active recall questions | Code review drill + architecture critique |
| ~55 min/session | ~60–75 min/session (harder cognitive load) |
| Progress = answered questions | Progress = passed code review drill |

**The four operations in each expert session:**
1. **Mental Model Challenge** — Confront what you think you know
2. **Code Review Drill** — Find errors in real code WITHOUT compiler hints
3. **Architecture Critique** — "What would I change in production?"
4. **Deliberate Drill** — Constraint exercise (write X without using Y)

**When to use `/viec hoc` in expert mode:**
- After attempting the code review drill (NOT before)
- To get AI feedback on your architecture critique
- To check if your deliberate drill solution is idiomatic

---

## 2. Module Plan

12 modules mapping course concepts to expert-level practice. Each module has:
- Core mental model (what experts actually think)
- Top mistake trap from this concept area
- Deliberate drill challenge

| # | Module | Core Mental Model | Top Mistake | Deliberate Drill |
|---|--------|------------------|-------------|-----------------|
| E01 | Type System Foundation | "The type system is a constraint language, not a documentation layer — it encodes invariants the compiler enforces at compile time" | Writing `any` to escape type errors instead of modeling the actual uncertainty with `unknown` | Rewrite a function that uses `any[]` to use proper generic type parameter |
| E02 | tsconfig & Strict Mode | "`strict: true` is not optional for new projects — it catches entire categories of bugs that `any` + optional parameters silently allow" | Disabling `strictNullChecks` to avoid null errors instead of modeling the nullability correctly | Convert a codebase from `strict: false` to `strict: true`: identify all the locations that now fail and fix them without using `!` or `as any` |
| E03 | Classes & Interfaces | "Interface defines a contract. Class provides an implementation. Using a class where an interface suffices couples callers to the implementation." | Writing `interface` where `type` alias would be clearer and more composable (especially for union types) | Replace all class-based service interfaces in a snippet with `type` aliases + function types where appropriate |
| E04 | Advanced Types & Narrowing | "Discriminated unions with `switch (thing.kind)` give you exhaustiveness checking — the compiler tells you when you've forgotten a branch" | Using `if (x.type === 'foo') { ... }` without type guard functions — creates implicit any in the `else` branch | Add a `never` exhaustiveness check to a switch over a union type that currently silently ignores new union members |
| E05 | Generics | "Generic constraints should be the minimum contract the function needs, not the maximum specificity of what you tested with" | Over-constraining generics to specific classes instead of the minimal interface they actually need | Convert `<T extends MyClass>` constraints to the minimal interface constraint that still satisfies the function body |
| E06 | Type Utilities & Mapped Types | "`Partial<T>`, `Readonly<T>`, `Pick<T, K>` are not magic — they're mapped types you can read in TS lib.d.ts. Understanding how they work lets you write your own utilities" | Using `Partial<T>` for update payloads when the actual type should be `Partial<T> & Pick<T, 'id'>` (requiring id but making rest optional) | Write `DeepReadonly<T>` without using `as const` — only using mapped types and conditional types |
| E07 | Decorators (ECMAScript) | "ECMAScript decorators (2023 spec) operate on class elements after initialization — they're not macros, they're wrappers. The spec is now stable; use these for new code." | Using experimental decorators (`experimentalDecorators: true`) in new code when ECMAScript decorators are now available | Convert an `@autobind` using experimental decorator semantics to ECMAScript decorator semantics |
| E08 | Modules & Import Types | "`import type { X }` is not a style choice — it's a correctness requirement when X is a type-only symbol that should not appear in the emitted JS. Missing it breaks `isolatedModules`." | Mixing `import { type X, Y }` where Y is a runtime value and X is a type — this compiles but signals unclear intent | Audit a module for `import type` violations: find all type-only imports that should have `type` keyword |
| E09 | Declaration Files & @types | "`@types/*` packages are NOT part of TypeScript — they're community-maintained type declarations. They can be wrong. Always verify `@types/X` against the actual JS library behavior for edge cases." | Trusting `@types/express` `Request` type for `req.user` without module augmentation — results in `any` on user data in every route handler | Write module augmentation to add `user: User` to Express `Request` type without modifying `@types/express` |
| E10 | DnD Architecture Critique | "The DnD project uses patterns that are correct for teaching but sub-optimal for production: all-in-one `app.ts`, mutable singleton state, `enum` instead of `const` object" | Treating course project architecture as production-ready — it teaches patterns but not how to modularize or test them | Propose a refactored file structure for the DnD project: separate types file, testable state module, remove `enum` in favor of `const` object |
| E11 | React + TypeScript Patterns | "React's `ComponentProps<typeof X>` is more maintainable than manually rewriting the prop type — it stays in sync as the component changes" | Writing manual prop type interfaces that duplicate what `ComponentProps`, `ReturnType`, or `Parameters` could generate automatically | Convert a manually typed React component to use `ComponentProps<'button'>` and `React.ReactNode` instead of custom prop type |
| E12 | Expert Gaps (Testing + CI) | "TypeScript's type system only runs at compile time. Without `tsc --noEmit` in CI and expect-type tests for public APIs, you have no guarantee the types stay correct over time." | Believing "it compiles" means "it's typed correctly" — compilation succeeds with `as any` scattered throughout | Write a GitHub Actions step that runs `tsc --noEmit` and fails the build if type errors exist |

---

## 3. Expert Red Flags

Patterns that appear in course code but are anti-patterns in production. These are taught because they demonstrate specific TS features — NOT because they're recommended in production codebases.

| # | Red Flag | Where in Course | Production Problem | Expert Alternative |
|---|---------|----------------|-------------------|--------------------|
| RF1 | `as any` for escape hatches | L25–L26 (type casting) | Silences compiler on real errors; every `as any` is a future runtime error | `unknown` + type guard function. Reserve `as` only for DOM element casting with `instanceof` check. |
| RF2 | `enum ProjectStatus` (from DnD) | L172, L14 | TypeScript enums compile to IIFE + reverse mapping, break tree-shaking, add 200 bytes per enum | `const ProjectStatus = { Active: 'Active', Finished: 'Finished' } as const; type ProjectStatus = typeof ProjectStatus[keyof typeof ProjectStatus]` |
| RF3 | All types in single `app.ts` | L162–L179 DnD | Untestable (state management and UI in one file), no separation of concerns | Separate: `types.ts` (interfaces), `state.ts` (ProjectState), `components/` (UI classes) |
| RF4 | `!` non-null assertion without guard | Lines 157, 163, 205 in DnD | Suppresses null check at compile time but not runtime — crashes if DOM element not found | Add a `getElement<T extends HTMLElement>(id: string): T` util that throws with a helpful message if element missing |
| RF5 | `private constructor` singleton | Lines 40–54 in DnD | Singletons prevent dependency injection and make testing impossible | For state management, use React Context (React apps), Redux/Zustand (complex state), or injectable service pattern (NestJS) |
| RF6 | `JSON.parse()` without validation | Not explicitly in course (gap) | `JSON.parse()` returns `any` — no type safety on API responses or localStorage data | Zod `.parse()` (M16 covers Zod) + `safeParse()` for error handling |
| RF7 | Implicit `this` in event listeners | L166 (`@autobind`) | Without `@autobind` or `.bind(this)`, `this` in event handlers is undefined or the wrong scope | Use arrow function class properties (works without decorators), or `@autobind`, or bind in constructor |

---

## 4. Lecture Matrix (Expert Notes)

Expert mode re-examines the same lectures with a critique lens. Key lectures with expert annotations:

| Lectures | Module | Expert Focus | Red Flag to Flag |
|----------|--------|-------------|-----------------|
| L13–L39 | M01 Types | Focus on: why `unknown` is always better than `any`, when `never` appears in exhaustive checks | `any` as escape hatch (RF1) |
| L41–L49 | M02 tsconfig | Focus on: what each strict flag prevents, why `isolatedModules` matters | Disabling strict (Red Flag in rationale) |
| L67–L87 | M05 Classes | Focus on: abstract class vs interface trade-offs, why `private constructor` is a design smell | Singleton anti-pattern (RF5) |
| L88–L99 | M06 Adv Types | Focus on: exhaustiveness checking with `never`, proper discriminated union design | Without exhaustiveness checks (E04 drill) |
| L100–L109 | M07 Generics | Focus on: minimum viable constraint, why `<T extends object>` is almost always wrong | Over-constrained generics (E05 drill) |
| L118–L132 | M09 Utilities | Focus on: how to READ `lib.d.ts` to understand utility types, write your own | Trusting utilities without understanding them |
| L133–L145 | M10 ECMAScript | Focus on: decorator composition order, when decorators are over-engineering | Mixing ECMAScript + experimental syntax |
| L162–L179 | M12 DnD | Focus on: architecture critique of `app.ts`, what you'd refactor in a team codebase | RF2 (enum), RF3 (all in one file), RF4 (! assertions), RF5 (singleton) |
| L214–L223 | M16 @types | Focus on: module augmentation for `req.user`, when .d.ts is wrong | Trusting @types blindly (E09 drill) |
| L232–L245 | M18 React | Focus on: `ComponentProps`, `ReturnType<typeof fn>`, generic components | Manual prop types that duplicate ComponentProps |
| L246–L257 | M19 Node | Focus on: typed middleware, `req.user` augmentation, async error types | Untyped error handling in routes |

---

## 5. Deliberate Practice Schedule

Expert mode sessions are harder than Speed mode. Recommended schedule:

**Session length:** 60–75 minutes
**Frequency:** Maximum 1 session per day (cognitive load is higher, consolidation requires sleep)
**Pre-session requirement:** Have completed the corresponding Speed mode module

| Session | Module | Time Investment |
|---------|--------|----------------|
| Expert S01 | E01 Type System + E02 tsconfig | 70 min |
| Expert S02 | E03 Classes + E04 Advanced Types | 70 min |
| Expert S03 | E05 Generics + E06 Type Utilities | 70 min |
| Expert S04 | E07 Decorators (ECMAScript) | 60 min |
| Expert S05 | E08 Modules + E09 Declaration Files | 60 min |
| Expert S06 | E10 DnD Architecture Critique | 75 min (deep analysis) |
| Expert S07 | E11 React Patterns + E12 Expert Gaps | 70 min |
| Expert S08 | Review + Red Flags audit | 60 min |

**Progress rules for expert mode:**
- Session passes when: code review drill identifies ≥2/3 planted errors, AND deliberate drill compiles without using the forbidden construct
- Session fails when: user used IDE error highlighting during code review drill (defeats the purpose)
- Stuck >10 minutes: write your best attempt, submit with `/viec hoc tra-loi`, get feedback, DO NOT look up the solution first
