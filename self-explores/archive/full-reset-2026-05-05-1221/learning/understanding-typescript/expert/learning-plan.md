---
type: learning-plan
course_slug: understanding-typescript
mode: expert
status: ready
created: 2026-05-04
---

# Learning Plan — Expert (Thành thạo TypeScript thực chiến)

## Mode Objective

Nắm TypeScript đến mức có thể: review PR người khác, thiết kế type system cho dự án lớn, giải thích trade-offs, không bị bất ngờ bởi compiler errors. Không chỉ biết syntax — phải biết WHY, WHEN TO AVOID, và ALTERNATIVES.

## Learning Policy

| Rule | Description |
|---|---|
| Question everything | Với mỗi concept: "When would I NOT use this?" |
| Go beyond transcript | Tìm edge cases transcript không đề cập |
| Type-level programming | Phải biết viết complex utility types |
| Error literacy | Biết đọc TypeScript compiler errors không cần Google |
| Architecture thinking | Biết thiết kế type hierarchy cho domain model |

## Expert Standards

Sau khi hoàn thành expert mode:
- [ ] Viết được `ReturnType<T>`, `Parameters<T>`, `ConstructorParameters<T>` từ đầu
- [ ] Giải thích tại sao `string & number = never`
- [ ] Biết khi nào interface tốt hơn type alias (và ngược lại)
- [ ] Nhận ra anti-patterns: `any` abuse, unnecessary generic constraints, over-engineering
- [ ] Đọc TypeScript source code của popular libraries (lodash, express types)
- [ ] Thiết kế được type-safe API cho complex domain

---

## Lecture Action Matrix

### M02 — Type System: Expert Analysis

| Lecture | Action | Expert Angle |
|---|---|---|
| L13-14 | ANALYZE | Tại sao TS không giống Haskell/Rust types? |
| L15 | DEEP_DIVE | Khi nào inference sai? (widening, narrowing pitfalls) |
| L16 | DEEP_DIVE | Parameter bivariance bug — biết vì sao tồn tại |
| L17 | CRITICAL | Tại sao `any` là escape hatch nguy hiểm — concrete examples |
| L18 | DEEP_DIVE | Union distribution rules, excess property checks |
| L19-L20 | ANALYZE | `Array<T>` vs `ReadonlyArray<T>` — covariance implications |
| L21 | NOTE | Generic preview — chuẩn bị cho M08 |
| L22 | ANALYZE | Tuple vs array: khi nào mỗi loại phù hợp hơn |
| L23 | DEEP_DIVE | Structural typing — tại sao `{a:1, b:2}` có thể gán cho `{a:number}` |
| L24 | CRITICAL | `!` operator vs proper null guard — khi nào dùng, khi nào nguy hiểm |
| L25 | ANALYZE | `Record<K,V>` vs index signature `{[key: K]: V}` — subtle differences |
| L26 | CRITICAL | Enum pitfalls: const enum vs regular enum, numeric vs string |
| L27 | DEEP_DIVE | Literal type widening: `let x = "a"` vs `const x = "a"` |
| L28 | CRITICAL | Type alias vs interface — mergeable, extendable, performance |
| L29-L32 | ANALYZE | Function type variance: parameters contravariant, return covariant |
| L33 | DEEP_DIVE | strictNullChecks implications across entire codebase |
| L34-L35 | DEEP_DIVE | Control flow analysis — TypeScript's narrowing algorithm |
| L36 | CRITICAL | `as` casting vs `satisfies` vs `is` guard — know the tradeoffs |
| L37 | ANALYZE | `unknown` vs `any` in public API design |
| L38-L39 | NOTE | Optional chaining compiles to runtime checks |

**M02 Expert Drill:** Viết 5 examples của type widening problems và fixes.

### M06 — Classes & Interfaces: Expert Analysis

| Lecture | Expert Angle |
|---|---|
| L69-L72 | Declaration merging on classes, prototype chain + TS interface |
| L73-L74 | Getter/setter vs plain property — performance, proxies |
| L75 | Static fields not in instance type — `typeof MyClass` vs `MyClass` |
| L76-L77 | `protected` in structural type system — gotchas |
| L78 | Abstract class vs interface: when class is right (shared logic) |
| L79-L82 | Interface declaration merging — intentional vs accidental |
| L83-L87 | Mixins pattern as alternative to multiple inheritance |

**M06 Expert Drill:** Design type hierarchy for a plugin system: `Plugin<Config>`, `BasePlugin`, `PluginRegistry`.

### M07 — Advanced Types: MASTER

| Lecture | Expert Angle |
|---|---|
| L89 | Intersection of object types — property merging rules, conflicting types |
| L90 | Type narrowing algorithm — which checks TypeScript recognizes |
| L91 | Discriminated unions in compiler: exhaustiveness checking |
| L92 | `instanceof` limitations — works only with classes, not interfaces |
| L93 | Type predicates can lie — they suppress errors, not prevent them |
| L94-L95 | Overloads vs union params vs generics — choose wisely |
| L96 | Index signatures block excess property checks |
| L97 | `as const` + `readonly` deep dive — const context propagation |
| L98 | Record type for category-keyed maps |
| L99 | `satisfies` preserves literal types while checking structure |

**M07 Expert Drill:** Implement a typesafe `match()` function like Rust pattern matching.

### M08 — Generics: MASTER

| Lecture | Expert Angle |
|---|---|
| L101 | Covariance of Array<T> — read-only safe, write unsafe |
| L102 | Generic type argument inference — when it works, when it fails |
| L103-L104 | Generic function vs overloaded function — expressiveness trade-off |
| L105 | Multiple constraints: `<T extends A & B>` vs separate params |
| L106-L107 | Constraint propagation — `T extends keyof U` enforces relationship |
| L108 | Generic classes vs generic interfaces — interface is structural |

**M08 Expert Drill:** Implement `pipe()` with full type inference:
```typescript
pipe(double, addOne, toString) // infer: (n: number) => string
```

### M10 — Type Utilities: MASTER ALL

| Lecture | Expert Angle |
|---|---|
| L119-L121 | `typeof` in type position vs value position |
| L122-L123 | `keyof` distributivity — `keyof (A | B)` vs `keyof (A & B)` |
| L124-L125 | Indexed access types — `T[K]` lookup rules |
| L126-L127 | Mapped types: `+/-` modifiers, remapping with `as` |
| L128 | Template literal types + conditional distribution |
| L129-L130 | Conditional type distribution over union types |
| L131 | `infer` in covariant vs contravariant position |
| L132 | Source code of built-in utilities — read `lib.es5.d.ts` |

**M10 Expert Drill:** Implement a typesafe deep-get function:
```typescript
deepGet<T, P extends string>(obj: T, path: P): DeepGet<T, P>
// deepGet({a: {b: {c: 42}}}, "a.b.c") → type is number
```

### M11 — Decorators: UNDERSTAND DESIGN

| Focus | Expert Angle |
|---|---|
| L133-L145 | How decorators interact with TypeScript's type system |
| | When class decorator changes instance type |
| | Method decorator preserving original function type |
| | Field decorator initialization order |

### M13 — DnD Project: ARCHITECTURE CRITIQUE

Instead of just building — analyze the architecture:
- Where does the class hierarchy break down?
- How would you redesign with composition instead of inheritance?
- What would it look like with functional approach?
- How does the state management compare to Redux?

### M17 — React + TS: ADVANCED PATTERNS

| Focus | Expert Angle |
|---|---|
| L233-L245 | Generic components: `<List<T> items={T[]} renderItem={(item: T) => ReactNode}` |
| | Discriminated union props: `type Props = { mode: "simple" } \| { mode: "complex"; config: Config }` |
| | Forward refs with generics: `forwardRef<HTMLInputElement, InputProps>` |
| | Context with type narrowing — avoid `as` on context value |

### M18 — Node + Express: TYPE SAFE APIs

| Focus | Expert Angle |
|---|---|
| L247-L257 | Typed middleware chain — `Request<P, ResBody, ReqBody, Query>` |
| | Zod + Express: validate body and infer type simultaneously |
| | Shared types between frontend and backend (monorepo approach) |

---

## Expert Reading List

After completing lectures, read source code of these `.d.ts` files:

```bash
# TypeScript built-in utilities
cat node_modules/typescript/lib/lib.es5.d.ts | grep -A5 "type Partial"
cat node_modules/typescript/lib/lib.es5.d.ts | grep -A5 "type ReturnType"
cat node_modules/typescript/lib/lib.es5.d.ts | grep -A5 "type Parameters"

# React types
cat node_modules/@types/react/index.d.ts | grep -A5 "type FC"
cat node_modules/@types/react/index.d.ts | grep -A5 "interface RefObject"

# Express types
cat node_modules/@types/express/index.d.ts | grep -A10 "interface Request"
```

---

## Module Plan

| Module | Est. Time | Expert Output |
|---|---|---|
| M02 | ~3h | Type widening drills, structural typing examples |
| M07 | ~3h | Typesafe match(), narrowing algorithm notes |
| M08 | ~3h | pipe() implementation, generic variance notes |
| M10 | ~4h | deepGet(), all built-in utilities manual impl |
| M06 | ~3h | Plugin system type design |
| M13 | ~4h | Architecture critique document |
| M17 | ~3h | Generic React components pattern |
| M18 | ~3h | Typed Express middleware chain |
| M11 | ~2h | Decorator type system notes |
| M09 | ~2h | Generic linked list with full type safety |
| M14 | ~1.5h | Module augmentation, declaration merging |
| M16 | ~2h | Zod schema inference patterns |
| M03 | ~1h | Strict tsconfig with justification for each flag |

**Tổng ước tính (expert mode):** ~34h → ~17 ngày × 2h
