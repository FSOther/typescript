# Speed Mode Active Recall Bank — Understanding TypeScript

**39 questions, no answers. Review before each session.**

Difficulty: ⭐ Easy | ⭐⭐ Medium | ⭐⭐⭐ Hard

---

| Q# | Question | Module | Difficulty | Tags |
|----|---------|--------|-----------|------|
| 1 | What is the difference between `type string` and `"active" \| "inactive"` as a type? When would you use each? | M01 | ⭐ | union, literal |
| 2 | You write `const x = []`. TypeScript infers `never[]`. You push a string — now it's `string[]`. But you wanted `Array<string \| number>`. How do you communicate that intent to TypeScript? | M01 | ⭐⭐ | inference, widening |
| 3 | What is the difference between `value?.length` and `value!.length`? When is each appropriate? | M01 | ⭐ | null-safety, narrowing |
| 4 | `unknown` vs `any` — TypeScript allows `(x as any).anything` but rejects `(x as unknown).anything`. Why? What must you do before using an `unknown` value? | M01 | ⭐⭐ | unknown, any |
| 5 | A function returns `void`. Can callers use the return value? What TypeScript behavior allows this for callback compatibility? | M01 | ⭐⭐ | void, callbacks |
| 6 | What are the 5 most important flags enabled by `"strict": true`? What class of bug does each one catch? | M02 | ⭐⭐ | tsconfig, strict |
| 7 | `"target": "ES5"` vs `"lib": ["ES2022"]` — what does each control? Can you use `Array.prototype.at(-1)` with this config on Node.js 10? | M02 | ⭐⭐ | tsconfig, target, lib |
| 8 | You want Vite to handle bundling but TypeScript to handle type checking. What two tsconfig flags work together to enable this, and why must they be used together? | M02 | ⭐⭐ | tsconfig, Vite, noEmit |
| 9 | What does `interface` support that `type` alias does not? Give one concrete example where this matters. | M05 | ⭐ | interface, type |
| 10 | You have `class ProjectItem extends Component<HTMLUListElement, HTMLLIElement>`. What does the `HTMLUListElement` type constraint prevent that `extends HTMLElement` alone would not? | M05 | ⭐⭐ | generics, constraints |
| 11 | In the DnD project, `Component<T, U>` has `attach()` as `private`. Could it be `protected`? What would `protected` allow that `private` doesn't? Should it be changed? | M05 | ⭐⭐ | access-modifiers |
| 12 | `abstract class` vs `interface` — when would you choose abstract class? What does abstract class enforce that interface cannot? | M05 | ⭐⭐ | abstract, interface |
| 13 | Write a user-defined type guard function signature for `isProject(x: unknown): x is Project`. What does TypeScript do differently in code after an `if (isProject(x))` check? | M06 | ⭐⭐ | type-guards, narrowing |
| 14 | Explain discriminated unions in one sentence. What makes the `type` field a "discriminant"? | M06 | ⭐ | discriminated-union |
| 15 | `A & B` (intersection) vs `A \| B` (union) — a function parameter that requires BOTH properties should use which? A function that accepts EITHER should use which? | M06 | ⭐ | intersection, union |
| 16 | You have `switch (shape.kind)` over a discriminated union with 3 cases. You add a 4th type to the union but forget to add a case. What must you add to the `default` branch to get a compile error? | M06 | ⭐⭐ | exhaustiveness, never |
| 17 | `type Listener<T> = (items: T[]) => void` — why does `State<T>` use a generic type parameter here instead of `any[]`? What does the compiler check that `any[]` would skip? | M07 | ⭐⭐ | generics, observer |
| 18 | Generic constraint `T extends { length: number }` — can this accept a `string`? A `number[]`? A custom `class Rope { length: number }`? Why? | M07 | ⭐ | constraints, structural |
| 19 | `ReturnType<typeof fn>` — what does this produce? Write the equivalent without using `ReturnType`. | M07 | ⭐⭐⭐ | utility-types, conditional |
| 20 | What is `keyof T`? If `T = { id: string; name: string; age: number }`, what is `keyof T`? | M09 | ⭐ | keyof, mapped-types |
| 21 | Implement `Partial<T>` as a mapped type from scratch. What does the `?` modifier on the left side of `:` do? | M09 | ⭐⭐ | mapped-types, modifiers |
| 22 | `Exclude<'a' \| 'b' \| 'c', 'a'>` returns `'b' \| 'c'`. Explain how conditional type distributivity makes this work. | M09 | ⭐⭐⭐ | conditional-types, distributivity |
| 23 | `type UnwrapPromise<T> = T extends Promise<infer U> ? U : T` — what does `infer U` do here? Why is it needed instead of just writing `Promise<U>`? | M09 | ⭐⭐⭐ | infer, conditional-types |
| 24 | `UpdatePayload<User>` where `User = { id: string; name: string; email: string }` — write the type without using `UpdatePayload`. What should the result look like? | M09 | ⭐⭐ | utility-types, Pick, Omit |
| 25 | ECMAScript decorators (2023 spec) vs experimental decorators — what's the key difference in how they bind `this`? Which uses `context.addInitializer`? | M10 | ⭐⭐ | decorators, ECMAScript |
| 26 | What does `context.addInitializer(function(this) { ... })` do? When exactly does the initializer run? | M10 | ⭐⭐ | decorators, initializer |
| 27 | A class method is passed as `element.addEventListener('click', this.handleClick)`. Why does `this` get lost, and what does `@autobind` solve? | M11 | ⭐ | this, autobind |
| 28 | The experimental `@autobind` uses a `get()` accessor instead of replacing `descriptor.value`. Why? What would happen if you replaced `descriptor.value` directly with a bound function? | M11 | ⭐⭐⭐ | decorators, accessor |
| 29 | Why are namespaces considered inferior to ES modules for large codebases? Give one concrete failure scenario. | M13 | ⭐ | modules, namespaces |
| 30 | `import { DragTarget } from './drag-drop.js'` — `DragTarget` is an interface (no runtime code). What flag in tsconfig requires you to write `import type { DragTarget }` instead? Why? | M13 | ⭐⭐ | import-type, isolatedModules |
| 31 | Even in `.ts` source files, ES module imports need `.js` extension: `'./file.js'`. Why does TypeScript require `.js` when the actual file is `.ts`? | M13 | ⭐⭐ | ESM, import-paths |
| 32 | `npm create vite@latest` — you choose Vanilla + TypeScript. The generated tsconfig has `"noEmit": true`. Why? What actually bundles the code? | M14 | ⭐ | Vite, noEmit |
| 33 | Vite doesn't type-check. What command should be in your CI pipeline to catch type errors, and why doesn't `npm run build` catch them? | M14 | ⭐ | Vite, CI, tsc |
| 34 | `const enum Direction { Up, Down }` fails in a Vite project. Why? What is the `as const` alternative? | M14 | ⭐⭐ | const-enum, isolatedModules |
| 35 | You install `lodash`. You get "Could not find a declaration file". What is the exact npm command to fix it? Where do these types come from? | M16 | ⭐ | @types, DefinitelyTyped |
| 36 | `@types/express` provides type information for Express. What is a `.d.ts` file? Does it contain runtime code? | M16 | ⭐ | .d.ts, declaration-files |
| 37 | TypeScript says your `User` type is correct. But `UserSchema.parse(apiResponse)` throws. Explain why TypeScript didn't catch this and what Zod adds. | M16 | ⭐⭐ | Zod, runtime-validation |
| 38 | In React, `useRef<HTMLInputElement>(null)` vs `useRef<HTMLInputElement>()` — what's the difference in the `current` type? Which should you use for DOM element refs? | M18 | ⭐⭐ | useRef, React |
| 39 | Node.js 22.6+ can run TypeScript natively. You have `enum Status { Active, Finished }`. It fails at runtime. Why? What is the `as const` alternative that works? | M19 | ⭐⭐ | enum, Node-native-TS |

---

## Usage Instructions

**Before a speed session:**
- Pick 5 questions at random from the modules you're about to study
- Answer from memory (write or speak out loud)
- Mark ones you got wrong — review after the session

**After completing a module:**
- Go through all questions tagged with that module
- If you can't answer without hints: re-read the module guide

**Spaced repetition:**
- Day 1: answer all questions from today's modules
- Day 3: re-answer questions you got wrong
- Day 7: answer all questions again (should be faster)
- Day 14: full pass — any failure means re-read that module guide
