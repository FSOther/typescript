---
date: 2026-05-05
type: lecture-resource-map
course: understanding-typescript
generated-by: typescript-avw
---

# Lecture → Resource → Code Map

Maps every lecture to its resource folder and key TypeScript files. Covers L1–L257 (skip legacy L258–L339).

**Legend:**
- `resources/N-slug/` — actual folder path
- `src/app.ts` — primary TS file (shorthand, full path from course root)
- ⭐ = finished/complete project state (best for study)
- No resource = transcript only (`lectures/N.Title.txt`)

---

## M00 — Course Introduction (L1–L9)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L1 | Welcome to the Course! | — | — | Admin |
| L2 | What Is TypeScript? | — | — | Admin |
| L3 | Why Would You Use TypeScript? | `resources/3-demo-app/` | (empty demo) | No code |
| L4–L9 | Community, Setup, About | — | — | Admin — skip |

---

## M01 — TypeScript Basics & Core Types (L10–L39)

*No resource folders for M01. All examples in transcript files.*

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L10 | Module Introduction | — | — | Module intro |
| L11 | Using Node.js To Run JavaScript Code | — | — | `ts-node`, `tsc` CLI |
| L12 | Project Setup | — | — | `tsconfig.json` init |
| L13 | Working with Types & Exploring Built-in Types | — | transcript | `string`, `number`, `boolean` annotations |
| L14 | Vanilla JavaScript Has Types, Too! | — | transcript | JS type coercion vs TS type checking |
| L15 | Type Inference vs Type Assignment | — | transcript | `let x = 5` (inferred) vs `let x: number` |
| L16 | Assigning Types To Function Parameters | — | transcript | `function add(a: number, b: number)` |
| L17 | The "any" Type | — | transcript | `any` — escape hatch, avoid |
| L18 | Understanding Union Types | — | transcript | `string \| number` |
| L19 | Arrays & Types | — | transcript | `string[]`, `Array<string>` |
| L20 | Advanced Array Types | — | transcript | Mixed arrays, typed push |
| L21 | A First Glimpse At Generic Types | — | transcript | `Array<string>` as generic |
| L22 | Making Sense of Tuples | — | transcript | `[string, number]` — fixed-length typed array |
| L23 | Object Types | — | transcript | `{ name: string; age: number }` inline type |
| L24 | Tricky: The "Must Not Be Null" Type | — | transcript | Non-null assertion `!`, `null` type |
| L25 | Flexible Objects with the Record Type | — | transcript | `Record<string, number>` |
| L26 | Working with Enums | — | transcript | `enum Direction { Up, Down }` |
| L27 | Being Specific With Literal Types | — | transcript | `"left" \| "right"` literal union |
| L28 | Type Aliases & Custom Types | — | transcript | `type ID = string \| number` |
| L29 | Function Return Value Types | — | transcript | `: string` return annotation |
| L30 | The "void" Type | — | transcript | `void` for functions with no return |
| L31 | The "never" Type | — | transcript | `never` — unreachable code, throw only |
| L32 | Functions As Types | — | transcript | `(a: number) => string` function type |
| L33 | null & undefined - Special Types | — | transcript | `strictNullChecks` behavior |
| L34 | Inferred null & Type Narrowing | — | transcript | `if (x !== null)` narrowing |
| L35 | Forced "Not Null" And Optional Chaining | — | transcript | `obj?.prop`, non-null `!` |
| L36 | Type Casting | — | transcript | `as HTMLInputElement`, `<HTMLInputElement>` |
| L37 | The "unknown" Type | — | transcript | `unknown` vs `any` — need type guard before use |
| L38 | Optional Values & TypeScript | — | transcript | `param?: string` optional parameter |
| L39 | Nullish Coalescing | — | transcript | `value ?? "default"` |

---

## M02 — TypeScript Compiler & Configuration (L40–L49)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L40 | Module Introduction | — | — | Module intro |
| L41 | TypeScript Project Setup & Creating a tsconfig.json | — | transcript | `tsc --init`, `tsconfig.json` |
| L42 | Exploring tsconfig Options: Target & Libs | — | transcript | `"target": "ES2020"`, `"lib": [...]` |
| L43 | Controlling File Input & Emission | — | transcript | `include`, `exclude`, `outDir`, `rootDir` |
| L44 | Configuring Type Checking | — | transcript | `strict`, `noImplicitAny`, `strictNullChecks` |
| L45 | Configuring Code Quality Checks | — | transcript | `noUnusedLocals`, `noUnusedParameters` |
| L46 | Example tsconfig.json & Deep Dive | — | transcript | Full tsconfig walkthrough |
| L47 | Compiling with tsconfig | — | transcript | `tsc` (no args, uses tsconfig) |
| L48 | Using TypeScript's Watch Mode | — | transcript | `tsc --watch` / `tsc -w` |
| L49 | Installing Type Packages | — | transcript | `npm i -D @types/node` |

---

## M03 — Node.js Workflow (L50–L57)

*Optional module — demonstrates running TS in Node without browser.*

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L50 | Module Introduction | — | — | Module intro |
| L51–L57 | Node.js workflow example | — | transcript | Custom types, union return, ts-node |

---

## M04 — Next-Generation JavaScript (L58–L66)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L58 | Module Introduction | — | — | Module intro |
| L59 | "let" and "const" | `resources/58-next-gen-01-starting-setup/` | `src/app.ts` | `const`, block-scoped `let` |
| L60 | Arrow Functions | `resources/60-next-gen-02-let-conts-arrow-functions/` | `src/app.ts` | `(x: number) => x * 2` |
| L61 | Default Function Parameters | — | transcript | `function greet(name = "World")` |
| L62 | The Spread Operator | `resources/62-next-gen-03-spread-operator/` | `src/app.ts` | `[...arr1, ...arr2]`, `{...obj}` |
| L63 | Rest Parameters | `resources/63-next-gen-04-spread-and-rest/` | `src/app.ts` | `function sum(...nums: number[])` |
| L64 | Array & Object Destructuring | `resources/64-next-gen-05-destructuring/` | `src/app.ts` | `const [a, b] = arr`, `const { name } = obj` |
| L65 | How Code Gets Compiled & Wrap Up | `resources/65-next-gen-06-finished/` ⭐ | `src/app.ts` | Finished: all modern JS patterns |
| L66 | Useful Resources & Links | — | — | Resources link only |

---

## M05 — Classes & Interfaces (L67–L87)

*No dedicated resource folder for M05. Patterns visible in M12 DnD project.*

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L67 | Module Introduction | — | — | Module intro |
| L68 | What are Classes? | — | transcript | `class Person { name: string; }` |
| L69 | Creating a First Class | — | transcript | Constructor, `this`, instance creation |
| L70 | A Useful TypeScript Shortcut | — | transcript | `constructor(private name: string)` shorthand |
| L71 | Making Sense of "public" and "private" | — | transcript | `private`, `public` access modifiers |
| L72 | Marking Fields as "readonly" | — | transcript | `readonly id: string` |
| L73 | Understanding Getters | — | transcript | `get name() { return this._name; }` |
| L74 | Setting Values with Setters | — | transcript | `set name(value: string) { ... }` |
| L75 | Exploring Static Properties & Methods | — | transcript | `static count = 0`, `ClassName.count` |
| L76 | Understanding Inheritance | — | transcript | `class Employee extends Person`, `super()` |
| L77 | The "protected" Modifier | — | transcript | `protected` — accessible in subclasses |
| L78 | Making Sense Of Abstract Classes | — | transcript | `abstract class Shape`, `abstract draw(): void` |
| L79 | Introducing Interfaces | — | transcript | `interface Greetable { greet(): void; }` |
| L80 | Creating a First Interface | — | transcript | Interface as object type blueprint |
| L81 | Interfaces As Object Types | — | transcript | `const obj: Greetable = { ... }` |
| L82 | Interfaces vs Type Aliases | — | transcript | Interface: extendable, declaration merging. Type: more flexible |
| L83 | Using Interfaces To Define Function Types | — | transcript | `interface AddFn { (a: number, b: number): number; }` |
| L84 | Implementing Interfaces | — | transcript | `class User implements Greetable { ... }` |
| L85 | Ensuring Base Types with Interfaces | — | transcript | Multiple interface implementation |
| L86 | Extending Interfaces | — | transcript | `interface Admin extends Employee { ... }` |
| L87 | How Interfaces Get Compiled | — | transcript | Interfaces = zero runtime, erased by compiler |

---

## M06 — Advanced Types (L88–L99)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L88 | Module Introduction | — | — | Module intro |
| L89 | Intersection Types | — | transcript | `type ElevatedEmployee = Admin & Employee` |
| L90 | More on Type Guards | — | transcript | `typeof`, `in`, custom predicates |
| L91 | Discriminated Unions | — | transcript | `type Shape = Circle \| Square` with `kind` field |
| L92 | Type Guards via "instanceof" | — | transcript | `if (animal instanceof Dog)` |
| L93 | "Outsourcing" Type Guards & Type Predicates | — | transcript | `function isString(val: any): val is string` |
| L94 | Function Overloads | — | transcript | Multiple `function add` signatures |
| L95 | Working with Function Overloads | — | transcript | Overload + implementation signature pattern |
| L96 | Making Sense of Index Types | `resources/96-adv-types-08-optional-chaining-nullish-coalescing/` | `src/app.ts` | `[key: string]: string` index signature, intersection types |
| L97 | Constant Types with "as const" | — | transcript | `const dirs = ["left", "right"] as const` |
| L98 | Revisiting the "Record" Type | — | transcript | `Record<"admin" \| "user", Person>` |
| L99 | The "satisfies" Keyword | — | transcript | `const palette = { red: [255,0,0] } satisfies Record<string, [number, number, number]>` |

---

## M07 — Generics (L100–L109)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L100 | Module Introduction | — | — | Module intro |
| L101 | A Generic Type We Already Know | — | transcript | `Array<string>` = `string[]` |
| L102 | Understanding Generic Types | — | transcript | `function identity<T>(arg: T): T` |
| L103 | Creating & Using a Generic Type | — | transcript | `type Pair<T, U> = { first: T; second: U }` |
| L104 | Generic Functions & Inference | — | transcript | TS infers `T` from argument |
| L105 | Working with Multiple Generic Parameters | — | transcript | `<T, U>` multiple type params |
| L106 | Generics & Constraints | — | transcript | `<T extends object>` constraint |
| L107 | Constraints & Multiple Generic Types | — | transcript | `<T extends object, U extends keyof T>` |
| L108 | Working with Generic Classes & Interfaces | — | transcript | `class Stack<T> { items: T[] = [] }` |
| L109 | Summary | — | — | Wrap-up |

---

## M08 — Generic Linked List Project (L110–L117)

*Optional project module. No dedicated resources — project built incrementally.*

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L110–L117 | Linked List project | — | transcript | `class LinkedList<T>`, `class Node<T>`, generic `add<T>()` |

---

## M09 — Advanced Type Utilities (L118–L132)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L118 | Module Introduction | — | — | Module intro |
| L119 | Using "typeof" | — | transcript | `type Config = typeof defaultConfig` |
| L120 | "typeof" & A More Useful Example | — | transcript | `ReturnType<typeof fn>` |
| L121 | Another Great Use-case for "typeof" | — | transcript | `typeof` in conditional types |
| L122 | Extracting Keys with "keyof" | — | transcript | `type Keys = keyof Person` → `"name" \| "age"` |
| L123 | "keyof" & A More Useful Example | — | transcript | `function getProperty<T, K extends keyof T>(obj: T, key: K)` |
| L124 | Understanding Indexed Access Types | — | transcript | `type NameType = Person["name"]` |
| L125 | Accessing Array Elements with Indexed Access Types | — | transcript | `type Element = string[][number]` |
| L126 | Introducing Mapped Types | — | transcript | `type ReadOnly<T> = { readonly [K in keyof T]: T[K] }` |
| L127 | Readonly Types & Optional Mapping | — | transcript | `{ [K in keyof T]+?: T[K] }` — adding/removing modifiers |
| L128 | Exploring Template Literal Types | — | transcript | `` `get${Capitalize<K>}` `` |
| L129 | Introducing Conditional Types | — | transcript | `T extends string ? "yes" : "no"` |
| L130 | Conditional Types - Another Example | — | transcript | Nested conditional types |
| L131 | Making Sense of the "infer" Keyword | — | transcript | `T extends Promise<infer U> ? U : T` |
| L132 | TypeScript's Got You Covered: Built-in Utility Types | — | transcript | `Partial<T>`, `Required<T>`, `Pick<T, K>`, `Omit<T, K>`, `ReturnType<F>`, `Parameters<F>` |

---

## M10 — ECMAScript Decorators (L133–L145)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L133 | Module Introduction | — | — | Module intro |
| L134 | What Are Decorators? | — | transcript | ECMAScript decorators vs experimental. `@decorator` syntax |
| L135 | Exploring Different Types of Decorators | — | transcript | Class, method, field, accessor decorators |
| L136 | Building a First Decorator | — | transcript | `function Logger(target: Function)` class decorator |
| L137 | Building a Class Decorator That Edits | — | transcript | Decorator that returns modified class |
| L138 | Decorator Code Execution Order | — | transcript | Bottom-up execution order for stacked decorators |
| L139 | Creating a Method Decorator | — | transcript | `(target, key, descriptor: PropertyDescriptor)` |
| L140 | Using Decorators To Solve A Common Problem | — | transcript | `autobind` method decorator use case |
| L141 | Implementing A Decorator-based Solution: autobind | — | transcript | `function autobind(_, __, descriptor)` — bind via getter |
| L142 | Replacing Methods with Decorators | — | transcript | Decorator replacing method descriptor |
| L143 | Introducing the Field Decorator | — | transcript | Field initializer decorator pattern |
| L144 | Building Configurable Decorators with Factories | — | transcript | `function Logger(message: string) { return function(...) }` |
| L145 | Onwards to Experimental Decorators | — | transcript | Transition note to legacy API |

---

## M11 — Experimental Decorators (L146–L161)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L146 | Module Introduction | — | — | Module intro |
| L147 | A First Class Decorator | `resources/147-decorators-01-starting-setup/` `resources/147-decorators-02-first-class-decorator/` | `src/app.ts` | `experimentalDecorators: true`, class decorator |
| L148 | Working with Decorator Factories | `resources/148-decorators-03-decorator-factories/` | `src/app.ts` | `@Logger("string")` factory pattern |
| L149 | Building More Useful Decorators | `resources/149-decorators-04-adv-decorators/` | `src/app.ts` | `@WithTemplate(html, hookId)` — returns modified class |
| L150 | Adding Multiple Decorators | `resources/150-decorators-05-finished-class-decorators/` ⭐ | `src/app.ts` | Multiple decorators on one class, execution order |
| L151 | Diving into Property Decorators | `resources/151-decorators-06-different-decorators/` | `src/app.ts` | `(target: any, propertyName: string)` |
| L152 | Accessor & Parameter Decorators | `resources/152-decorators-07-non-class-decorators-finished/` ⭐ | `src/app.ts` | `PropertyDescriptor` accessor decorator, `position: number` param decorator |
| L153 | When Do Decorators Execute? | — | transcript | Build time vs runtime execution |
| L154 | Returning a Class in a Class Decorator | — | transcript | Decorator returns `class extends originalConstructor` |
| L155 | Other Decorator Return Types | `resources/155-decorators-08-returning-values-in-decorators/` | `src/app.ts` | Method decorator returning new descriptor |
| L156 | Example: Creating an "Autobind" Decorator | `resources/156-decorators-09-example-autobind/` ⭐ | `src/app.ts` | `@autobind` — `get() { return this.originalMethod.bind(this) }` |
| L157 | Validation with Decorators - First Steps | — | transcript | Validation metadata approach |
| L158 | Validation with Decorators - Finished | `resources/158-decorators-10-decorator-validation/` ⭐ | `src/app.ts` | `@Required`, `@Positive` — metadata registry, `validate()` function |
| L159 | Fixing a Validator Bug | — | transcript | Bug fix in validation logic |
| L160 | Wrap Up | — | — | Module summary |
| L161 | Useful Resources & Links | — | — | Resources link only |

---

## M12 — Drag & Drop Project (L162–L181)

*Full OOP application — most valuable resource for practice/expert modes.*

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L162 | Module Introduction | — | — | Module intro |
| L163 | Getting Started | `resources/163-prj-00-initial-starting-setup/` | `src/app.ts` | Project bootstrap, HTML template setup |
| L164 | DOM Element Selection & OOP Rendering | `resources/164-prj-02-prj-input-form/` | `src/app.ts` | `HTMLFormElement` typing, `document.querySelector<T>()` |
| L165 | Interacting with DOM Elements | `resources/165-prj-03-form-access-and-bind-this/` | `src/app.ts` | `this` binding problem with event listeners |
| L166 | Creating & Using an "Autobind" Decorator | `resources/166-prj-04-autobind-decorator/` | `src/app.ts` | `@autobind` decorator applied, `private submitHandler` |
| L167 | Fetching User Input | `resources/167-prj-05-fetching-user-input-with-validation/` | `src/app.ts` | `[string, string, number] \| void` return type, tuple |
| L168 | Creating a Re-Usable Validation Functionality | `resources/168-prj-06-more-elaborate-validation/` | `src/app.ts` | `interface Validatable`, validation function |
| L169 | Rendering Project Lists | `resources/169-prj-07-rendering-a-project-section/` | `src/app.ts` | `ProjectList` class, `insertAdjacentHTML` |
| L170 | Managing Application State with Singletons | `resources/170-prj-08-basic-list-rendering-basic-state-mgmt/` | `src/app.ts` | `private constructor()` + `static getInstance()` singleton |
| L171 | More Classes & Custom Types | `resources/171-prj-09-project-and-listener-types/` | `src/app.ts` | `type Listener = (items: Project[]) => void`, listener array |
| L172 | Filtering Projects with Enums | `resources/172-prj-10-filtering-added/` | `src/app.ts` | `enum ProjectStatus { Active, Finished }` |
| L173 | Adding Inheritance & Generics | `resources/173-prj-11-inheritance-and-generics/` | `src/app.ts` | `abstract class Component<T extends HTMLElement, U extends HTMLElement>` |
| L174 | Rendering Project Items with a Class | `resources/174-prj-12-added-projectitem-class/` | `src/app.ts` | `class ProjectItem extends Component<HTMLUListElement, HTMLLIElement>` |
| L175 | Using a Getter | `resources/175-prj-13-added-a-getter/` | `src/app.ts` | `get persons(): string` getter on class |
| L176 | Utilizing Interfaces to Implement Drag & Drop | `resources/176-prj-14-draggable-list-item/` | `src/app.ts` | `interface Draggable { dragStartHandler(event: DragEvent): void }` |
| L177 | Drag Events & Reflecting the Current State | `resources/177-prj-15-visual-drag-and-drop-feedback/` | `src/app.ts` | `dragOverHandler`, `classList.add("droppable")` |
| L178 | Adding a Droppable Area | — | transcript | `event.preventDefault()` in dragOver — required for HTML5 DnD |
| L179 | Finishing Drag & Drop | `resources/179-prj-16-finished/` ⭐ | `src/app.ts` | **COMPLETE PROJECT**: all patterns integrated |
| L180 | Wrap Up | — | — | Module summary |
| L181 | Useful Resources & Links | — | — | Resources link only |

**⭐ Key pattern reference — `resources/179-prj-16-finished/prj-16-finished/src/app.ts`:**
- Abstract generic base class: `abstract class Component<T extends HTMLElement, U extends HTMLElement>`
- Singleton: `private constructor()` + `static getInstance()`
- Observer: `type Listener<T> = (items: T[]) => void`
- `@autobind` decorator avoiding `.bind(this)` in constructor
- `event.preventDefault()` in `dragOverHandler` — REQUIRED for HTML5 DnD drop zones
- `Draggable` and `DragTarget` interfaces for DnD

---

## M13 — Modules & Namespaces (L182–L193)

*Multi-file project demonstrating ES modules refactoring of DnD app.*

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L182 | Module Introduction | `resources/182-starting-project/` | `src/app.ts`, `src/components/*.ts` | Multi-file starting point |
| L183 | Writing Module Code - Your Options | — | transcript | Namespaces vs ES modules comparison |
| L184 | Working with Namespaces | `resources/184-prj-16-finished/` | `src/app.ts` | `namespace App { }` pattern (not recommended) |
| L185 | Organizing Files & Folders | — | transcript | File structure for namespace approach |
| L186 | A Problem with Namespace Imports | `resources/186-modules-01-namespaces/` | `src/components/base-component.ts` | Namespace triple-slash reference issues |
| L187 | Using ES Modules | `resources/187-modules-02-es-modules-basics/` | `src/components/project-input.ts` | `import { ProjectState } from '../state/project-state.js'` — note `.js` extension |
| L188 | Configuring tsconfig | — | transcript | `"module": "ESNext"`, `"moduleResolution": "bundler"` |
| L189 | Understanding various Import & Export Syntaxes | — | transcript | Named, default, namespace import/export |
| L190 | How Does Code In Modules Execute? | `resources/190-modules-03-finished-modules/` ⭐ | `src/app.ts`, `src/components/base-component.ts`, `src/components/project-input.ts`, `src/components/project-item.ts`, `src/components/project-list.ts`, `src/state/project-state.ts` | **COMPLETE ES MODULES PROJECT** |
| L191 | Understanding "type" Imports | — | transcript | `import { type ProjectStatus }` — `verbatimModuleSyntax` |
| L192 | Wrap Up | — | — | Module summary |
| L193 | Useful Resources & Links | — | — | Resources link only |

---

## M14 — Modern Build Tools (L194–L202)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L194 | Module Introduction | — | — | Module intro |
| L195 | The Problem With The TypeScript Compiler | `resources/195-starting-project/` | `src/app.ts` | TS compiler limitations for modern tooling |
| L196 | Building with Webpack or ESBuild | — | transcript | Comparison: Webpack vs ESBuild vs Vite |
| L197 | Introducing Vite | — | transcript | `npm create vite@latest`, config basics |
| L198 | Using Vite | — | transcript | Vite + TS: `vite.config.ts`, `.ts` extension handling |
| L199 | Installing TypeScript on a Per-Project Basis | — | transcript | `npm i -D typescript`, `./node_modules/.bin/tsc` |
| L200 | Making Sense of .d.ts Files | — | transcript | `.d.ts` — type declarations without JS, how Vite generates them |
| L201 | Importing non-TS Files | — | transcript | Importing `.css`, `.svg` with type declarations |
| L202 | Wrap Up | — | — | Module summary |
| L203 | About This Section | — | — | Separator — old Webpack section below |

---

## M15 — Webpack (Legacy Build Tool) (L204–L213)

*Outdated — Vite (M14) is the modern alternative. Skip unless maintaining legacy project.*

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L204 | Module Introduction | — | — | Module intro |
| L205 | What is Webpack & Why do we need it? | — | transcript | Webpack concepts |
| L206 | Installing Webpack & Important Dependencies | — | transcript | `webpack`, `webpack-cli`, `ts-loader` install |
| L207 | Adding Entry & Output Configuration | `resources/207-webpack-01-basic-setup/` | `src/app.ts`, `src/components/*.ts` | `webpack.config.js` entry/output |
| L208 | Adding TypeScript Support with ts-loader | `resources/208-webpack-02-added-ts-loader/` | `src/app.ts`, `src/components/*.ts` | `ts-loader` configuration |
| L209 | Adjust Webpack Config | — | transcript | Optimization config |
| L210 | Finishing the Setup & Adding webpack-dev-server | `resources/210-webpack-03-finished-dev-setup/` ⭐ | `src/app.ts`, `src/components/base-component.ts`, `src/decorators/autobind.ts`, `src/models/drag-drop.ts`, `src/models/project.ts`, `src/state/project-state.ts`, `src/util/validation.ts` | **BEST STRUCTURED CODEBASE** — separated by concern |
| L211 | Adding a Production Workflow | `resources/211-webpack-04-added-prod-workflow/` | Same as L210 + prod webpack config | Production build with `clean-webpack-plugin` |
| L212 | Wrap Up | — | — | Module summary |
| L213 | Useful Resources & Links | — | — | Resources link only |

**⭐ Note on `resources/210-webpack-03-finished-dev-setup/`:**
This has the best-organized codebase structure: `state/`, `models/`, `decorators/`, `components/`, `util/` separation. Use this for understanding production TypeScript project structure.

---

## M16 — Third-Party Libraries & TypeScript (L214–L223)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L214 | Module Introduction | — | — | Module intro |
| L215 | Problem: Using JS Libraries with TypeScript | `resources/215-starting-project/` | `app.ts` | `lodash` without types — error |
| L216 | Solution: Installing @types Packages | — | transcript | `npm i -D @types/lodash` |
| L217 | Exploring .d.ts Files | — | transcript | `node_modules/@types/lodash/index.d.ts` anatomy |
| L218 | Diving Deeper Into .d.ts Files | — | transcript | Type declaration patterns in .d.ts |
| L219 | Using "declare" Manually | — | transcript | `declare var GLOBAL: string` — manual global declaration |
| L220 | There Are Libraries With Great TypeScript Support! | — | transcript | Libraries with bundled types (no @types needed) |
| L221 | Exploring Zod As a TypeScript-first Library | — | transcript | `z.string().min(5)`, `z.object({...})` |
| L222 | Diving Deeper Into Zod | — | transcript | `z.infer<typeof schema>` — derive TS type from Zod schema |
| L223 | Runtime vs Compile-time Types with Zod | — | transcript | Zod bridges TS static types + runtime validation |

---

## M17 — Google Maps Project (L224–L231)

*Optional — requires Google Maps API key. Demonstrates @types/google.maps usage.*

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L224 | Module Introduction | — | — | Module intro |
| L225 | Project Setup | `resources/225-prj-libs-01-starting-setup/` `resources/225-prj-libs-02-basic-form-and-markup/` | `src/app.ts` | Axios install, form typing |
| L226 | Getting User Input | — | transcript | Form event typing `FormEvent` |
| L227 | Setting Up a Google API Key | — | transcript | Environment variable `process.env` typing |
| L228 | Using Axios to Fetch Coordinates | `resources/228-prj-libs-03-fetching-coordinates+2/` | `src/app.ts` | `axios.get<ResponseType>(url)` — generic Axios call |
| L229 | Rendering a Map with Google Maps | `resources/229-prj-libs-04-finished/` ⭐ | `src/app.ts` | `@types/google.maps`, `new google.maps.Map()` typed |
| L230 | Working with Maps without a Credit Card | — | transcript | Alternative map API approach |
| L231 | Useful Resources & Links | — | — | Resources link only |

---

## M18 — React + TypeScript (L232–L245)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L232 | Module Introduction | — | — | Module intro |
| L233 | Setting Up a React + TypeScript Project | `resources/233-starting-project/` | `vite.config.ts`, `src/App.tsx`, `src/main.tsx` | `npm create vite@latest -- --template react-ts` |
| L234 | Typing Components & Props | — | transcript | `interface Props { name: string; age?: number }`, `function Comp({ name }: Props)` |
| L235 | Using Components & Props | — | transcript | `<Header title="My App" />` — props passing typed |
| L236 | The Special "children" Prop & Optional Props | — | transcript | `children: React.ReactNode`, `PropTypes.node`, optional `?` |
| L237 | Adding Another Component With Props | — | transcript | Multiple props interfaces |
| L238 | Component Functions & Types - An Alternative | — | transcript | `React.FC<Props>` vs plain function (avoid FC pattern) |
| L239 | Managing State | — | transcript | `useState<Goal[]>([])` — explicit generic for empty arrays |
| L240 | Passing Functions As Props | — | transcript | `onDelete: (id: string) => void` function prop type |
| L241 | Handling Form Submission | — | transcript | `FormEvent<HTMLFormElement>` event type |
| L242 | Working with Refs | — | transcript | `useRef<HTMLInputElement>(null)` — explicit generic |
| L243 | More Function Passing & State Updating | — | transcript | Derived state from props pattern |
| L244 | Fixing a Typo | — | — | Minor fix |
| L245 | Wrap Up | — | — | Module summary |

---

## M19 — Node.js + Express + TypeScript (L246–L257)

| Lecture | Title | Resource Folder | Key Files | Code Patterns |
|---------|-------|-----------------|-----------|---------------|
| L246 | Module Introduction | — | — | Module intro |
| L247 | Node & TypeScript: Options & Alternatives | — | transcript | Compilation, `ts-node`, native strip, Bun |
| L248 | Setting up a Project | — | transcript | `@types/node`, `@types/express` separate install |
| L249 | First Server & Working with Node Types | — | transcript | `import { Request, Response } from "express"` |
| L250 | Adding & Using Express (+ Types) | — | transcript | `app.get('/path', (req: Request, res: Response) => {...})` |
| L251 | Managing Data with Help of TypeScript | — | transcript | Typed in-memory data store, `Todo` interface |
| L252 | Adding & Testing a First Route | — | transcript | POST route with typed body parsing |
| L253 | Finishing The Basic App | — | transcript | PUT, DELETE routes typed |
| L254 | Native Node.js TypeScript Support | — | transcript | `node --input-type=module-sync` (Node 22+), `--experimental-strip-types` |
| L255 | Type Stripping vs Type Transformation | — | transcript | Strip-only: no enums, no experimental decorators. Transform: full |
| L256 | Exploring Alternative Runtimes Like Bun | — | transcript | Bun: full TS support, `bun run app.ts` |
| L257 | Thanks for being part of the course! | — | — | Course outro |

---

## LEGACY (L258–L339) — SKIP

Re-recordings of M01, M02, M05, M07 with older content. No new patterns.

| Range | Content | Equivalent New Lectures |
|-------|---------|------------------------|
| L259–L282 | TypeScript Basics (LEGACY) | Use L10–L39 instead |
| L284–L299 | TS Compiler Config (LEGACY) | Use L40–L49 instead |
| L301–L326 | Classes & Interfaces (LEGACY) | Use L67–L87 instead |
| L328–L339 | Generics (LEGACY) | Use L100–L109 instead |

**LEGACY resources** (L260–L322 folders in `resources/`): Same patterns as modern equivalents but older code style. Use modern lecture resources instead.

---

## Summary: Lectures With Code Resources

| Module | Lectures with Resources | Key Patterns |
|--------|------------------------|--------------|
| M04 Modern JS | L59–L65 | Spread, rest, destructuring, arrow fns |
| M06 Advanced Types | L96 | Intersection types, optional chaining |
| M11 Experimental Decorators | L147–L158 | Class, property, method, param decorators; autobind; validation |
| M12 DnD Project | L163–L179 | Abstract generic base class, singleton, observer, DnD interfaces |
| M13 Modules | L182–L190 | Namespaces → ES modules, multi-file structure |
| M14 Build Tools | L195 | Vite starting project |
| M15 Webpack (Legacy) | L207–L211 | Best-structured codebase (state/models/decorators separation) |
| M16 3rd-Party | L215, L228–L229 | @types usage, Axios generic call, Google Maps types |
| M18 React+TS | L233 | React+TS Vite starter |

**Lectures without resources (transcript only):** M01, M02, M03, M05, M07, M08, M09, M10, M18 (L234–L245), M19 (all)  
**Coverage:** 92 resource folders mapped, ~85% of lectures L58–L229 have entries
