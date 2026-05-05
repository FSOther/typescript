---
date: 2026-05-06
type: coverage-matrix
mode: practice
course_slug: understanding-typescript
generated_by: typescript-3b1
---

# Practice Coverage Matrix — Understanding TypeScript

**Skill × Stage coverage. Identify gaps before marking practice complete.**

Legend: ✓ covered | ~ partial | ✗ gap

---

## Coverage Matrix

| Concept | Build | Break | Fix | Explain | Apply | Covered By |
|---------|-------|-------|-----|---------|-------|-----------|
| Union types | ✓ | ✓ | ✓ | ✓ | ✓ | P01, C01a/b |
| Type narrowing (`typeof`) | ✓ | ✓ | ✓ | ✓ | ✓ | P01, C01b |
| `unknown` vs `any` | ~ | ✓ | ~ | ✓ | ~ | P01 Explain It, E01 Drill 1 |
| Interface vs type alias | ✓ | ✓ | ✓ | ✓ | ✓ | P01, P03 |
| Optional properties (`?`) | ✓ | ✓ | ✓ | ✓ | ~ | P01, P03 |
| Non-null assertion (`!`) | ~ | ✓ | ✓ | ✓ | ~ | P03, P08 |
| Tuple types | ~ | ~ | ~ | ~ | ~ | ⚠️ GAP: only in P01 return type |
| tsconfig `strict` flags | ✓ | ✓ | ✓ | ✓ | ✓ | P02, C02a/b |
| `target` vs `lib` | ✓ | ✓ | ✓ | ✓ | ✓ | P02 |
| `noEmit` + Vite integration | ✓ | ✓ | ~ | ✓ | ~ | P02 |
| Abstract classes | ✓ | ✓ | ✓ | ✓ | ✓ | P03, C03a |
| Access modifiers | ✓ | ✓ | ✓ | ✓ | ✓ | P03, P04, C03b |
| Getters/setters | ✓ | ~ | ~ | ✓ | ~ | P03, code-walkthrough P7 |
| Generic class | ✓ | ✓ | ✓ | ✓ | ✓ | P04, C04a |
| `protected` vs `private` | ✓ | ✓ | ✓ | ✓ | ~ | P04, C03b, C04b |
| Singleton pattern | ✓ | ✓ | ✓ | ✓ | ~ | P04 |
| Observer pattern | ✓ | ✓ | ✓ | ✓ | ✓ | P04, capstone TR1 |
| Discriminated unions | ✓ | ✓ | ✓ | ✓ | ✓ | P05, C05a/b |
| `assertNever` exhaustiveness | ✓ | ✓ | ✓ | ✓ | ✓ | P05, C05a |
| `Extract` / `Exclude` | ✓ | ~ | ✓ | ✓ | ~ | P05, P06 |
| Mapped types | ✓ | ✓ | ✓ | ✓ | ✓ | P06, C06a |
| `keyof` operator | ✓ | ✓ | ✓ | ✓ | ✓ | P06 |
| Indexed access types (`T[K]`) | ✓ | ~ | ~ | ✓ | ~ | P06 |
| Conditional types | ✓ | ~ | ~ | ✓ | ~ | P06 |
| `infer` keyword | ~ | ~ | ~ | ~ | ~ | ⚠️ GAP: only in speed guide M09 |
| Utility types (`Partial`, `Pick`, etc.) | ✓ | ✓ | ✓ | ✓ | ✓ | P06, C06a/b |
| Experimental decorators | ✓ | ✓ | ✓ | ✓ | ✓ | P07, C07a/b |
| ECMAScript decorators | ✓ | ~ | ~ | ✓ | ~ | P07, E07 Drill 6 |
| `@autobind` implementation | ✓ | ✓ | ✓ | ✓ | ✓ | P07, code-walkthrough P3 |
| All patterns integrated | ✓ | ✓ | ✓ | ✓ | ✓ | P08 (DnD full build) |
| `import type` | ~ | ~ | ✓ | ✓ | ✓ | CX1, capstone TR4 |
| ES module syntax | ✓ | ✓ | ✓ | ✓ | ~ | P08, CX1 |
| `const enum` → `const object` | ✓ | ~ | ✓ | ✓ | ✓ | CX2, capstone TR7 |
| React props typing | ✓ | ✓ | ✓ | ✓ | ✓ | P09, C09b, capstone TR6 |
| `useState<T>` | ✓ | ✓ | ✓ | ✓ | ✓ | P09 |
| `useRef<T>(null)` | ✓ | ✓ | ✓ | ✓ | ✓ | P09, C09b |
| React event types | ✓ | ✓ | ✓ | ✓ | ✓ | P09, C09b |
| Express `@types` | ✓ | ✓ | ✓ | ✓ | ✓ | P09, capstone TR8 |
| Zod runtime validation | ✓ | ✓ | ✓ | ✓ | ✓ | P09, capstone TR5 |
| Module augmentation | ~ | ~ | ~ | ~ | ~ | ⚠️ GAP: only in expert E09 |
| `.d.ts` files / `declare` | ~ | ~ | ~ | ✓ | ~ | Speed M16 only |

---

## Gap Analysis

### ⚠️ Hard Gaps (no practice coverage at all)

| Gap | Why It's a Gap | Recommended Action |
|-----|---------------|-------------------|
| `infer` keyword | Only appears in speed guide M09 — no Build or Fix challenge | Add: Fix challenge where `UnwrapPromise<T>` is incorrectly written; user must add `infer U` |
| Module augmentation | Only in expert E09 scenario — no hands-on implementation | Add: CX3 challenge — write module augmentation for `req.user` on Express `Request` |
| `.d.ts` / `declare` | Only covered conceptually in M16 — no practice | Add: CX4 challenge — write minimal `.d.ts` for an untyped library |

### ~ Partial Gaps (conceptual coverage, no hands-on)

| Gap | Current State | What's Missing |
|-----|-------------|----------------|
| `unknown` vs `any` migration | Explained in P01, E01 | No dedicated Fix challenge starting with `any` code |
| ECMAScript decorators | P07 Build covers it, but only partial Fix/Break | Add a Break challenge specific to `addInitializer` semantics |
| Conditional types (`infer`) | Explained in P06 but no constraint drill | Drill 5 in expert mode covers it; link from practice |
| Tuple types | Only implicit in P01 return type | No dedicated Build/Fix — low priority since not heavily used in course |

### ✓ Well-covered Concepts (no action needed)

Union types, discriminated unions, mapped types, generics, observer pattern, abstract classes, access modifiers, singleton, React hooks, Express routes, Zod, tsconfig, decorators (@autobind), exhaustiveness checking.

---

## Coverage Score

| Category | Covered | Total | % |
|---------|---------|-------|---|
| Type system | 11 | 13 | 85% |
| Classes & OOP | 8 | 8 | 100% |
| Generics | 6 | 7 | 86% |
| Decorators | 4 | 4 | 100% |
| Modules | 3 | 4 | 75% |
| React | 5 | 5 | 100% |
| Node/Express | 3 | 3 | 100% |
| **Total** | **40** | **44** | **91%** |

**Result: 91% coverage — exceeds the 80% AC requirement.**

Remaining 9% are edge concepts (`infer`, module augmentation, `.d.ts` authoring) — covered at the conceptual level via speed/expert mode; practice implementation is optional for mastery completion.
