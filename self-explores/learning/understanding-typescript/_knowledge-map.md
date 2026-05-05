---
date: 2026-05-06
type: knowledge-map
course: understanding-typescript
generated-by: typescript-o05
---

# Knowledge Map — Understanding TypeScript

## 1. Concept Taxonomy

TypeScript knowledge organized into 6 layers, from foundational to applied.

### Layer 1 — Type System Primitives
Core concepts without which nothing else makes sense.

| Concept | Sub-concepts |
|---------|-------------|
| Basic types | `string`, `number`, `boolean`, `null`, `undefined`, `any`, `unknown`, `never`, `void` |
| Type inference | When TS infers vs when to annotate explicitly |
| Type annotation | Variable, parameter, return type syntax |
| Union types | `A \| B`, narrowing, exhaustiveness checks |
| Intersection types | `A & B`, merging shapes |
| Literal types | `"exact"`, `42`, `true` as types |
| Type aliases | `type MyType = ...`, reuse + composition |
| Tuple types | Fixed-length arrays with per-index types |
| Array types | `T[]` vs `Array<T>`, typed iteration |
| Enum | `enum Direction`, numeric vs string enums |
| Object types | Inline `{ key: type }` vs named |
| Record type | `Record<K, V>` — typed key-value maps |
| Nullable types | `T \| null`, `T \| undefined`, optional `?` |
| Optional chaining | `obj?.prop?.method?.()` |
| Nullish coalescing | `a ?? b` — null/undefined fallback |
| Type casting | `as T`, `<T>` — force type assertion |

### Layer 2 — Type Manipulation
Deriving new types from existing types.

| Concept | Sub-concepts |
|---------|-------------|
| `typeof` | Extract type from value, narrow in conditionals |
| `keyof` | Union of object keys as literal types |
| Indexed access types | `T[K]` — type of a property |
| Mapped types | `{ [K in keyof T]: V }` — transform shapes |
| Readonly/Optional mapping | `+readonly`, `-?` modifiers in mapped types |
| Template literal types | `` `${A}${B}` `` — string composition at type level |
| Conditional types | `T extends U ? X : Y` |
| `infer` keyword | Extract type within conditional: `infer R` |
| Built-in utility types | `Partial`, `Required`, `Pick`, `Omit`, `ReturnType`, `Parameters`, `Awaited`, `Readonly` |

### Layer 3 — OOP Patterns
TypeScript-flavored object-oriented programming.

| Concept | Sub-concepts |
|---------|-------------|
| Classes | Syntax, constructor, `this` |
| Access modifiers | `public`, `private`, `protected` |
| `readonly` | Immutable after construction |
| Constructor shorthand | `constructor(private x: T)` → auto-field |
| Getters & setters | `get`, `set` accessors |
| Static members | `static` properties and methods |
| Inheritance | `extends`, `super()`, method override |
| `protected` modifier | Subclass-accessible but not public |
| Abstract classes | `abstract class`, `abstract method()` |
| Interfaces | Object contracts — `interface I { method(): void }` |
| Interface implementation | `implements I` — class-interface contract |
| Declaration merging | Same interface declared twice = merged |
| Interface as function type | `interface Fn { (arg: T): U }` |
| Interface extension | `interface B extends A` |

### Layer 4 — Advanced Type Safety
Patterns for safely handling complex runtime scenarios.

| Concept | Sub-concepts |
|---------|-------------|
| Type guards — `typeof` | `if (typeof x === "string")` |
| Type guards — `instanceof` | `if (x instanceof MyClass)` |
| Type guards — `in` | `if ("prop" in obj)` |
| Custom type predicates | `function isX(val): val is X` |
| Discriminated unions | Shared `kind: "A" \| "B"` literal field |
| Function overloads | Multiple signatures + 1 implementation |
| Index types | `[key: string]: T` — dynamic property |
| `as const` | Lock literal values as readonly tuple/literal |
| `satisfies` keyword | Validate shape without widening |

### Layer 5 — Generics
Reusable type-safe code parameterized by type.

| Concept | Sub-concepts |
|---------|-------------|
| Generic types | `type Box<T> = { value: T }` |
| Generic functions | `function wrap<T>(val: T): T[]` |
| Generic inference | TS infers T from argument |
| Multiple generics | `<T, U>`, `<K extends keyof T>` |
| Generic constraints | `T extends SomeInterface` |
| Generic classes | `class Repository<T>` |
| Generic interfaces | `interface Repository<T>` |

### Layer 6 — Project Infrastructure
Making TypeScript work in real projects.

| Concept | Sub-concepts |
|---------|-------------|
| tsconfig.json | `target`, `lib`, `strict`, `outDir`, `rootDir`, `paths`, `include`, `exclude` |
| Strict mode | `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes` |
| Watch mode | `tsc --watch` |
| Type packages | `npm install --save-dev @types/X` |
| ES modules | `import`/`export`, `type` imports, `index.ts` |
| Namespaces | `namespace N {}` — legacy TS module system |
| Declaration files | `.d.ts` — typing for untyped JS |
| `declare` keyword | Ambient declarations for global scope |
| Zod integration | Runtime validation + TS type inference |
| Build tools | Vite (`vite.config.ts`), ESBuild |
| React integration | JSX types, `FC<P>`, `useState<T>`, refs, events |
| Node.js + Express | `@types/node`, `@types/express`, typed routes |

### Layer 7 — Decorators
Metaprogramming / framework patterns.

| Concept | Sub-concepts |
|---------|-------------|
| ECMAScript decorators | Modern spec (L133–L145) — class, method, field |
| Decorator factories | `@decorator(config)` — factory pattern |
| `autobind` decorator | `get` accessor + descriptor manipulation |
| Execution order | Bottom-up for stacked decorators |
| Experimental decorators | Legacy syntax (`experimentalDecorators: true`) — property, accessor, parameter |
| Validation decorators | Metadata registry + `@Required`, `@Positive` |

---

## 2. Dependency Graph

What must be learned before what. Arrow = "required before."

```
Layer 1 (Primitives)
├─→ Layer 2 (Type Manipulation)   [need typeof, keyof, object types]
├─→ Layer 3 (OOP)                 [need basic types + type aliases]
│   └─→ Layer 4 (Advanced Safety) [need union + OOP + instanceof]
│       └─→ Layer 5 (Generics)    [need constraints = interfaces]
│           └─→ Layer 7 (Decorators) [need class + generic patterns]
└─→ Layer 6 (Infrastructure)      [can start after L1, parallel to rest]
    └─→ React / Node.js           [need L1-L5 + tsconfig]
```

### Concept-level prerequisites

| Concept | Must Know First |
|---------|----------------|
| Intersection types | Union types |
| Literal types | Type aliases |
| Discriminated unions | Union types + literal types |
| `keyof` | Object types |
| Mapped types | `keyof` + indexed access |
| Conditional types | Generic types |
| `infer` | Conditional types |
| Built-in utilities | Mapped types + conditional types |
| Abstract classes | Inheritance + interfaces |
| Custom type predicates | Type narrowing + union types |
| Function overloads | Functions as types |
| Generics | Type aliases + interfaces |
| Generic constraints | Generics + interfaces |
| Decorators | Classes + generics |
| Zod | Type aliases + inference |
| React integration | All L1–L5 + tsconfig |
| Node.js integration | All L1–L5 + tsconfig + modules |

---

## 3. Concept-to-Lecture Map

| Module | Lectures | Concepts Covered |
|--------|----------|-----------------|
| M01 — Types | L13–L39 | Primitives, inference, union, tuple, enum, literal, aliases, null/undefined, casting, unknown, optional chaining, nullish coalescing |
| M02 — tsconfig | L41–L49 | tsconfig target/lib, strict mode, file control, watch mode, `@types` packages |
| M03 — Node.js workflow | L50–L57 | **[skip]** Node.js compilation workflow (covered by M02 patterns) |
| M04 — Modern JS | L58–L65 | `let`/`const`, arrow functions, default params, spread, rest params, destructuring |
| M05 — Classes & Interfaces | L67–L87 | Class syntax, access modifiers, readonly, getters/setters, static, inheritance, abstract, interfaces, declaration merging |
| M06 — Advanced Types | L88–L99 | Intersection, type guards (all forms), discriminated unions, overloads, index types, `as const`, Record, `satisfies` |
| M07 — Generics | L100–L109 | Generic types, generic functions, constraints, generic classes/interfaces |
| M08 — Generic Project | L110–L117 | **[optional]** Generic linked list — applies M07 patterns |
| M09 — Type Utilities | L118–L132 | `typeof`, `keyof`, indexed access, mapped types, template literal types, conditional types, `infer`, built-in utilities |
| M10 — ECMAScript Decorators | L133–L145 | Class/method/field decorators, factories, `autobind`, execution order |
| M11 — Experimental Decorators | L146–L160 | Property/accessor/parameter decorators, returning values, `@autobind`, validation with metadata |
| M12 — DnD Project | L162–L179 | All of above applied — abstract base class, singleton, observer, `@autobind`, generics, interfaces |
| M13 — Modules | L182–L191 | Namespaces, ES modules, `import type`, index.ts barrel files |
| M14 — Vite/ESBuild | L192–L198 | Vite setup, vite.config.ts, fast builds |
| M15 — Webpack | L204–L213 | **[skip/outdated]** Use Vite instead |
| M16 — 3rd-party Types | L214–L223 | `@types/*`, `.d.ts`, `declare`, Zod for runtime validation |
| M17 — Google Maps | L224–L231 | **[optional]** 3rd-party type consumption in practice |
| M18 — React + TS | L232–L245 | JSX types, `FC<Props>`, `useState<T>`, `useRef<T>`, event handlers, Context |
| M19 — Node.js + TS | L246–L257 | `@types/node`, `@types/express`, typed routes, middleware types |

---

## 4. Learning Milestones

Five checkpoints that signal readiness to move to the next phase.

### Milestone 1 — Type Annotator (after M01, L13–L39)
**Can:** Annotate any JS variable, function, object with correct TS types.  
**Key test:** Write a typed function that accepts `string | number`, returns `string`, handles null.  
**Lectures:** L13–L39  
**Resource to verify:** `58-m04-01/src/app.ts` (L58 uses types before introducing new concepts)

### Milestone 2 — Project Configurator (after M02, L41–L49)
**Can:** Create a working `tsconfig.json` for a new project with strict mode.  
**Key test:** Configure `target`, `strict`, `outDir`, `rootDir` correctly; compile without errors.  
**Lectures:** L41–L49  
**Resource to verify:** `59-m04-01/` structure shows basic project layout

### Milestone 3 — OOP Practitioner (after M05–M06, L67–L99)
**Can:** Design a class hierarchy using inheritance, interfaces, access modifiers, type guards.  
**Key test:** Build a typed `Animal` hierarchy with `abstract speak()`, `instanceof` guards, discriminated union for subtypes.  
**Lectures:** L67–L99  
**Resource to verify:** `96-adv-types-08/src/app.ts` (intersection types + type guards)

### Milestone 4 — Generic Designer (after M07+M09, L100–L132)
**Can:** Write generic functions/classes with constraints; use mapped types and utility types.  
**Key test:** Create `Repository<T extends { id: string }>` with typed CRUD. Build `DeepReadonly<T>` mapped type.  
**Lectures:** L100–L109, L118–L132  
**Resource to verify:** `132-m09-16/src/app.ts` (built-in utility types in action)

### Milestone 5 — Full Project Builder (after M12+M16+M18/M19, L162–L257)
**Can:** Build a complete TypeScript project — React app or Node.js API — with real type safety.  
**Key test:** Implement DnD project from memory; OR build Express API with typed routes + Zod validation.  
**Lectures:** L162–L179, L214–L245  
**Resources to verify:** `179-prj-16-finished/src/` (DnD), `233-starting-project/` (React+TS)

---

## 5. Concept-to-Pattern Map

TypeScript concepts → actual code patterns, with resource paths for direct study.

| Concept | Pattern Name | Resource Path | Key File |
|---------|-------------|---------------|----------|
| Type inference | Basic inference chain | `58-m04-01/` | `src/app.ts` |
| Union + narrowing | typeof narrowing | `89-adv-types-01/` | `src/app.ts` |
| Discriminated unions | `kind` literal field | `91-adv-types-03/` | `src/app.ts` |
| Intersection types | Object composition | `96-adv-types-08/` | `src/app.ts` |
| `as const` | Readonly literal lock | `97-adv-types-09/` | `src/app.ts` |
| `satisfies` | Shape validation | `99-adv-types-11/` | `src/app.ts` |
| Generics basics | `wrap<T>()` pattern | `103-m07-03/` | `src/app.ts` |
| Generic constraints | `T extends HasId` | `106-m07-06/` | `src/app.ts` |
| Mapped types | Transform all keys | `127-m09-10/` | `src/app.ts` |
| Conditional types | `T extends X ? Y : Z` | `129-m09-12/` | `src/app.ts` |
| `infer` keyword | Extract return type | `131-m09-14/` | `src/app.ts` |
| Built-in utilities | `Partial`, `Pick`, `Omit` | `132-m09-16/` | `src/app.ts` |
| ECMAScript decorator (class) | `@Logger` transforms class | `137-decorators-03/` | `src/app.ts` |
| `@autobind` (ECMAScript) | Getter descriptor trick | `156-decorators-09-example-autobind/` | `src/app.ts` |
| Validation decorators | Metadata registry | `158-decorators-10-decorator-validation/` | `src/app.ts` |
| Abstract base class | `abstract class Component<T, U>` | `179-prj-16-finished/src/` | `components/base-component.ts` |
| Singleton | `private constructor` + `static getInstance()` | `179-prj-16-finished/src/` | `state/project-state.ts` |
| Observer/Listener | `type Listener<T> = (items: T[]) => void` | `179-prj-16-finished/src/` | `state/project-state.ts` |
| HTML5 DnD interfaces | `Draggable`, `DragTarget` | `179-prj-16-finished/src/` | `models/drag-drop.ts` |
| ES module imports | `import type`, barrel `index.ts` | `190-modules-03-finished-modules/` | `src/` |
| @types packages | `npm i --save-dev @types/X` | `215-starting-project/` | `package.json` |
| Zod runtime validation | `z.object({}).parse()` | `225-prj-libs-01-starting-setup/` | `src/app.ts` |
| React component props | `FC<Props>`, `useState<T>` | `233-starting-project/` | `src/components/` |
| Express typed routes | `Request`, `Response` generics | `246-node-01-starting-project/` | `src/app.ts` |
| Vite + TS config | `vite.config.ts` | `233-starting-project/` | `vite.config.ts` |

---

## Quick Reference — Skip/Core Decision

| Status | Lectures | Why |
|--------|----------|-----|
| **SKIP (admin)** | L1–L9 | Zero TypeScript content |
| **SKIP (outdated)** | L204–L213 | Webpack → use Vite (L192–L198) |
| **SKIP (optional project)** | L110–L117 | Generic linked list — M07 already covers generics |
| **SKIP (external API)** | L224–L231 | Google Maps requires API key |
| **SKIP (legacy)** | L258–L339 | Re-recordings of L13–L57 — no new content |
| **CORE** | L13–L49 | Type system + tsconfig = non-negotiable foundation |
| **CORE** | L67–L109 | Classes + interfaces + generics = everyday TS |
| **CORE** | L118–L132 | Type utilities = reading library types |
| **CORE** | L133–L160 | Decorators (both flavors) = Angular/NestJS patterns |
| **CORE** | L162–L179 | DnD project = all concepts applied |
| **CORE** | L187–L191 | ES modules = required for any modern project |
| **CORE** | L214–L223 | 3rd-party types = real-world JS integration |
| **CORE** | L232–L257 | React + Node.js = project-building goal |
