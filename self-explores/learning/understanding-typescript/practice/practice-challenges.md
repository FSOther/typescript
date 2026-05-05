---
date: 2026-05-06
type: practice-challenges
mode: practice
course_slug: understanding-typescript
generated_by: typescript-a1v
---

# Practice Challenges — Understanding TypeScript

**20 challenges. No solutions. 2 per module (Build + Fix).**

Difficulty: ⭐ Easy | ⭐⭐ Medium | ⭐⭐⭐ Hard

---

| ID | Module | Title | Type | Difficulty | AC (Given → When → Then) | Skills Tested |
|----|--------|-------|------|-----------|--------------------------|--------------|
| C01a | P01 | Build `validate()` from spec | Build | ⭐⭐ | **Given** spec for `Validatable` interface and validation rules / **When** `validate()` is implemented / **Then** compiles with `strict: true`; `validate({ value: '', required: true })` returns false; `validate({ value: 5, min: 1, max: 10 })` returns true | Union types, typeof narrowing, interface |
| C01b | P01 | Fix narrowing bug in validator | Fix | ⭐⭐ | **Given** `validate()` that accesses `.length` without `typeof` guard / **When** TypeScript compiles / **Then** error on `.length` access; fix adds `typeof value === 'string'` before the check | Type narrowing, typeof |
| C02a | P02 | Write tsconfig for Node.js API | Build | ⭐ | **Given** requirements: CJS output, dist/ target, strict, Node.js 20 / **When** `tsc` runs / **Then** emits to `dist/` as CommonJS; no type errors on `require()` usage | tsconfig, module, strict |
| C02b | P02 | Fix 4 bugs in broken tsconfig | Fix | ⭐⭐ | **Given** tsconfig with wrong types for boolean fields, wrong include syntax, missing lib / **When** `tsc --noEmit` runs / **Then** 4+ errors; fixing all makes tsc pass | tsconfig validation |
| C03a | P03 | Build `Component<T, U>` abstract class | Build | ⭐⭐⭐ | **Given** spec: template pattern, generic DOM types, abstract configure + renderContent / **When** `ProjectInput extends Component<HTMLDivElement, HTMLFormElement>` / **Then** `this.element.addEventListener()` compiles; direct `new Component()` is a compile error | Abstract class, generics, DOM types |
| C03b | P03 | Fix wrong access modifiers | Fix | ⭐⭐ | **Given** `State<T>` with `private listeners` and subclass calling `this.listeners` / **When** TypeScript compiles / **Then** error on `this.listeners` in subclass; fix changes to `protected` | Access modifiers, inheritance |
| C04a | P04 | Build `State<T>` observer pattern | Build | ⭐⭐⭐ | **Given** spec for generic observer with `Listener<T>` and singleton `ProjectState` / **When** `projectState.addListener(fn)` then `projectState.addProject(...)` / **Then** listener called with `Project[]`; `new ProjectState()` is compile error | Generic classes, singleton, observer |
| C04b | P04 | Fix broken `Listener<T>` type | Fix | ⭐⭐ | **Given** `Listener<T> = (item: T) => void` (missing array) and `updateListeners` calling `fn(this.projects)` / **When** TypeScript compiles / **Then** type error on listener call; fix makes listeners receive `T[]` | Generic types, type mismatch |
| C05a | P05 | Build discriminated union event handler | Build | ⭐⭐⭐ | **Given** spec for `DomainEvent` union (4 types) and `handleEvent()` with exhaustiveness / **When** new type added to union without handler / **Then** compile error in `handleEvent` default branch | Discriminated union, assertNever, exhaustiveness |
| C05b | P05 | Fix narrowing bug in status handler | Fix | ⭐⭐ | **Given** function that accesses `event.fromStatus` in a case that handles all event types / **When** TypeScript compiles / **Then** error because `fromStatus` only exists on one union member; fix adds correct case narrowing | Discriminated union, narrowing |
| C06a | P06 | Implement `Partial<T>` and `Pick<T, K>` from scratch | Build | ⭐⭐⭐ | **Given** requirement to implement both without using lib.d.ts versions / **When** used with `User` type / **Then** `MyPartial<User>` makes all fields optional; `MyPick<User, 'id'>` removes non-id fields; wrong key in Pick is a compile error | Mapped types, keyof, constraints |
| C06b | P06 | Fix over-constrained generic | Fix | ⭐⭐ | **Given** `<T extends ProjectItem>` constraining a function that only uses `T.title` / **When** called with a plain object `{ title: 'Foo' }` / **Then** error because object doesn't extend `ProjectItem`; fix changes to structural constraint | Generic constraints, structural typing |
| C07a | P07 | Write `@autobind` from spec (experimental) | Build | ⭐⭐ | **Given** spec for experimental decorator using `get()` accessor / **When** applied to event handler passed as callback / **Then** `this` refers to class instance even without direct call | Experimental decorators, PropertyDescriptor |
| C07b | P07 | Fix decorator that doesn't bind `this` | Fix | ⭐⭐⭐ | **Given** decorator that returns `original` (unbound) from `get()` / **When** method passed as callback / **Then** `this` is undefined; fix adds `.bind(this)` inside the accessor | Decorator, this-binding |
| C08a | P08 | Build `ProjectItem` component | Build | ⭐⭐⭐ | **Given** spec for `ProjectItem extends Component<HTMLUListElement, HTMLLIElement> implements Draggable` / **When** constructed with a Project / **Then** `renderContent()` fills h2/h3/p; `dragStartHandler` sets dataTransfer | Multiple patterns, drag events |
| C08b | P08 | Fix null access bugs in `ProjectList` | Fix | ⭐⭐ | **Given** `dropHandler` accessing `event.dataTransfer.getData()` without null check / **When** TypeScript compiles / **Then** error on `event.dataTransfer` (could be null); fix adds assertion or check | Null safety, DragEvent |
| C09a | P09 | Build typed Express CRUD endpoints | Build | ⭐⭐⭐ | **Given** spec for Todo CRUD API (4 routes) with Zod validation / **When** POST with invalid body / **Then** returns 400 with ZodError; TypeScript error if handler returns wrong type | Express, @types, Zod, runtime validation |
| C09b | P09 | Fix React component type errors | Fix | ⭐⭐ | **Given** component with `useRef()` (no type/initial value) and `handleSubmit(e)` (implicit any) / **When** TypeScript compiles / **Then** 3 errors; fix adds `<HTMLInputElement>(null)`, event type, optional chain | useRef, event types, null safety |
| CX1 | Cross | Audit file for `import type` violations | Fix | ⭐⭐ | **Given** Vite project (isolatedModules) with interface imports without `type` keyword / **When** `tsc --noEmit` runs / **Then** errors for type-only imports without `import type`; fix converts each | import type, isolatedModules |
| CX2 | Cross | Convert enum to const object | Fix | ⭐ | **Given** `enum Status { Active = 'ACTIVE', Finished = 'FINISHED' }` used in 3 places / **When** converted to `as const` object / **Then** all 3 call sites still compile; compiled output has no IIFE | const enum, as const, migration |
