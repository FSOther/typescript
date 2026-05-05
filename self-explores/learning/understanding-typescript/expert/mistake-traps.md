---
date: 2026-05-06
type: mistake-traps
mode: expert
course_slug: understanding-typescript
generated_by: typescript-qcy
---

# Expert Mistake Traps — Understanding TypeScript

**15 TypeScript-specific traps. Each has a drill.**

---

| # | Trap | Who Makes It | Why It Happens | Expert Avoidance | Drill |
|---|------|-------------|---------------|-----------------|-------|
| T01 | `as any` for escape hatches | Intermediate devs hitting a type error they don't understand | TypeScript error feels like an obstacle, not a signal | Use `unknown` + type guard. `any` is viral — every derived value is also `any` | Audit a function using `as any`. Replace with `unknown` + a type guard function |
| T02 | `as TargetType` without `instanceof` check | DOM-heavy code | "I know what this is" (wrong) | For DOM: cast via runtime check or use `instanceof`. For external data: validate with Zod before casting | Write `getTypedElement<T extends HTMLElement>(id: string): T \| null` without using `as` |
| T03 | `enum` in production code | Developers coming from C#/Java | Familiar syntax, but TypeScript enums compile to JS with reverse mapping | `const Status = { Active: 'Active' } as const; type Status = typeof Status[keyof typeof Status]` | Convert an existing enum to const object. Verify compiled output has no IIFE |
| T04 | `!` non-null assertion in constructor | When working with DOM APIs | TypeScript's null warning feels annoying | Validate before asserting, or use factory function pattern that returns null on failure | Find all `!` in a file. For each: write the runtime check that would replace it |
| T05 | Over-constraining generics to concrete classes | Service layer, repositories | Familiarity with OOP hierarchies | Constrain to the minimal structural interface required by the function body | Change `<T extends UserClass>` to the minimum interface constraint. Run the function with an object literal that satisfies the interface |
| T06 | `Partial<T>` for update payloads (forgetting id) | Backend CRUD handlers | "Partial means optional fields" | `Pick<T, 'id'> & Partial<Omit<T, 'id'>>` — ID is always required | Write a type-safe `updateUser` function where the ID is always required but other fields are optional |
| T07 | `import type` omitted with `isolatedModules: true` | Vite projects | Looks redundant if you don't understand the bundler's constraint | Always add `type` keyword for type-only imports when `isolatedModules: true` | Run `tsc --noEmit` on a file after setting `isolatedModules: true`. Fix every error |
| T08 | `interface` where `type` alias needed for union | When creating union types | "I always use interface" | Use `type` for unions, intersections, mapped types. Use `interface` for object shapes and class contracts | Convert 3 misused `interface` declarations to `type` aliases. Verify no behavior changes |
| T09 | Missing `assertNever` in switch over union | Event handlers, state reducers | No explicit error signal when new case added | Always end a union switch with `default: assertNever(x)` | Add a new member to a union type. Observe where the compile error appears |
| T10 | `!= null` vs `!== undefined` confusion | Form handling, API response parsing | Uncertainty about loose vs strict equality | `!= null` catches both `null` and `undefined` (correct for most cases). Use `!== undefined` only when `null` is valid | Write a function where `null` and `undefined` are distinct valid inputs. Show where each check is needed |
| T11 | Using `Function` as a type | Event handlers, callbacks | "I don't care about the signature" | Use specific function type: `(event: MouseEvent) => void`. `Function` loses all parameter and return types | Find usages of `Function` type. Replace each with a correct specific signature |
| T12 | Singleton accessed globally (module-level) | State management | "That's how singletons work" | Pass singleton as constructor argument (dependency injection). Enables testing with fakes | Refactor `const state = State.getInstance()` to be injected into each component |
| T13 | `req.body` typed as `any` in Express | Backend API routes | Express doesn't know your request shape | Use Zod schema to parse body, or write a typed middleware | Write a Zod schema for a POST body. Parse it in the handler. Show ZodError for invalid input |
| T14 | `useRef<T>()` (no initial value) for DOM refs | React event handlers | Doesn't read the error message | `useRef<HTMLInputElement>(null)` — initial value `null` required for DOM attachment | Fix a component where `ref.current` access throws because initial value was omitted |
| T15 | `const enum` in projects using bundlers | Cross-module type sharing | Unfamiliarity with bundler constraints | `const enum` requires full TypeScript compilation. With Vite/ESBuild: use `as const` objects | Replace `const enum` with `as const` object. Verify compiled output produces the same values |

---

## Common Patterns Behind the Traps

**"TypeScript is blocking me"** → T01, T02, T04 — usually means your types don't match your intent, not that TypeScript is wrong

**"I'll cast it and move on"** → T01, T02, T13 — cast accumulates; runtime errors appear weeks later

**"This worked in Java/C#"** → T03, T05, T08, T12 — TypeScript is structurally typed; OOP patterns need adaptation

**"I don't need that annotation"** → T07, T09, T11, T14 — TypeScript's inference is good but not telepathic
