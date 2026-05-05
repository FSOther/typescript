---
type: learning-plan
course_slug: understanding-typescript
mode: practice
status: ready
created: 2026-05-04
---

# Learning Plan — Practice (Học bằng cách làm)

## Mode Objective

Học để dùng được ngay. Build được, sửa được, test được. Transcript dùng để giải thích, không phải trọng tâm. Mỗi concept phải có bài thực hành kèm — dùng resource folders trong `resources/`.

## Learning Policy

| Rule | Description |
|---|---|
| Code first | Mở resource folder trước, đọc code, đoán concept, rồi mới đọc transcript để confirm |
| Per concept | Phải có bài tự code (không copy) sau khi học |
| Per module | Phải có challenge hoặc mini task kết hợp các concept |
| Resource folders | Ưu tiên dùng starter code trong `resources/` làm sandbox |
| Transcript depth | Chỉ đọc phần giải thích concept cốt lõi, bỏ qua phần setup/config |

## Practice Strategy per Action

| Action | How to Practice |
|---|---|
| STUDY | Đọc code → tự type lại từ đầu → so sánh |
| DEEP_DIVE | Đọc → implement → tìm edge case → break it on purpose |
| BUILD | Làm từ starter → hoàn thiện feature → refactor |
| SKIM | Xem qua code structure, không cần implement |
| SKIP | Bỏ qua hoàn toàn |

---

## Lecture Action Matrix

### M01 — Setup (L1-9): SKIP
Biết JS, skip toàn bộ setup.

### M02 — Type System (L10-39): CODE-ALONG

| Lecture | Title | Action | Practice Task |
|---|---|---|---|
| L10-12 | Setup | SKIP | — |
| L13 | Working with Types | STUDY | Tạo file `types-intro.ts`, annotate 5 biến different types |
| L14 | Vanilla JS Has Types | SKIM | — |
| L15 | Type Inference vs Assignment | STUDY | Viết 5 ví dụ inference + 5 explicit annotation, so sánh hover types |
| L16 | Assigning Types To Fn Params | STUDY | Viết 3 functions với typed params + return types |
| L17 | The "any" Type | STUDY | Viết hàm dùng `any`, convert sang typed version |
| L18 | Union Types | STUDY | Viết function xử lý `string \| number \| boolean` input |
| L19 | Arrays & Types | STUDY | Typed array + multi-type array examples |
| L20 | Advanced Array Types | STUDY | `ReadonlyArray`, spread với typed arrays |
| L21 | Generic Types preview | SKIM | — |
| L22 | Tuples | STUDY | Viết coordinate tuple, RGB tuple, key-value pair |
| L23 | Object Types | STUDY | Define 2 object types với nested objects |
| L24 | "Must Not Be Null" | STUDY | Viết 3 ví dụ `!` operator, biết khi nào dùng |
| L25 | Record Type | STUDY | `Record<string, number>` cho dictionary, `Record<"a"\|"b", boolean>` |
| L26 | Enums | STUDY | Tạo enum cho Direction, Status — compile xem JS output |
| L27 | Literal Types | STUDY | `type Status = "open" \| "closed" \| "pending"`, viết function với literal param |
| L28 | Type Aliases | STUDY | Tạo 5 type aliases, dùng trong function signatures |
| L29-L31 | Function Types (return, void, never) | STUDY | Typed function signatures, void vs never examples |
| L32 | Functions As Types | STUDY | `type Callback = (event: Event) => void`, higher-order function |
| L33 | null & undefined | STUDY | Strict null checks examples, guard patterns |
| L34-L35 | Narrowing & Not-Null | STUDY | 3 narrowing patterns: typeof, truthiness, not-null assertion |
| L36 | Type Casting | STUDY | `as` syntax + angle bracket syntax, when to use |
| L37 | unknown Type | STUDY | `unknown` vs `any`, narrowing before use |
| L38-L39 | Optional & Nullish | STUDY | `?.` optional chaining, `??` nullish coalescing in practice |

**M02 Challenge:** Tạo `practice/m02-type-system.ts` — một small data model với users, posts, comments. Dùng: union types, literal types, tuples, Record, enums, optional chaining, nullish coalescing.

### M03 — tsconfig (L40-49): CONFIGURE

| Lecture | Action | Practice |
|---|---|---|
| L40 | SKIP | — |
| L41-48 | SKIM | Đọc qua tsconfig options, note 5 options thường dùng nhất |
| L49 | STUDY | Install 1 @types package, verify type hints |

### M04 — Mini Project (L50-57): BUILD

| Lecture | Action | Practice |
|---|---|---|
| L50-57 | BUILD | Dùng `resources/` starter, hoàn thiện project, add own features |

**M04 Challenge:** Extend mini project với 2 features không có trong course.

### M05 — Modern JS (L58-66): SKIP

### M06 — Classes & Interfaces (L67-87): CODE-ALONG

| Lecture | Title | Action | Practice |
|---|---|---|---|
| L67-68 | Intro | SKIP | — |
| L69 | First Class | STUDY | Tạo class `Person` với constructor + methods |
| L70 | Shortcut & Compiling | STUDY | Constructor shorthand: `constructor(public name: string)` |
| L71 | public/private | STUDY | Refactor Person — private fields + public methods |
| L72 | readonly | STUDY | `readonly id`, `readonly createdAt = new Date()` |
| L73-74 | Getters/Setters | STUDY | Getter computed property, setter với validation |
| L75 | Static | STUDY | `static count`, `static create()` factory method |
| L76 | Inheritance | STUDY | `Employee extends Person`, `super()`, override |
| L77 | protected | STUDY | `protected` field accessible in subclass |
| L78 | Abstract Classes | DEEP_DIVE | Abstract `Shape` → `Circle`, `Rectangle` implement `area()` |
| L79 | Introducing Interfaces | DEEP_DIVE | Interface `Printable`, `Serializable`, implement in class |
| L80-81 | Interface basics | STUDY | Interface với optional props, readonly props |
| L82 | Interfaces vs Type Aliases | DEEP_DIVE | Viết cùng shape 2 cách, tìm 3 differences thực tế |
| L83 | Interfaces for Function Types | STUDY | Interface như function type vs type alias |
| L84-87 | Implementing, extending | DEEP_DIVE | Class implement 2 interfaces, interface extends interface |

**M06 Challenge:** Tạo `practice/m06-classes.ts` — OOP hierarchy: `Animal → Dog/Cat`, `Serializable` interface, `toJSON()` method.

### M07 — Advanced Types (L88-99): DEEP_DIVE

| Lecture | Title | Action | Practice |
|---|---|---|---|
| L88 | Intro | SKIP | — |
| L89 | Intersection Types | DEEP_DIVE | `Admin & User` intersection — viết helper merge function |
| L90 | Type Guards | DEEP_DIVE | 3 guard patterns: typeof, instanceof, custom predicate |
| L91 | Discriminated Unions | DEEP_DIVE | `type Shape = Circle \| Square` với `kind` discriminant |
| L92 | instanceof guard | DEEP_DIVE | Class hierarchy + instanceof narrowing |
| L93 | Type Predicates | DEEP_DIVE | `function isString(val): val is string` — custom guard |
| L94-95 | Function Overloads | STUDY | Overloaded function signature cho string + number input |
| L96 | Index Types | STUDY | `interface StringMap { [key: string]: string }` |
| L97 | as const | STUDY | `["a","b"] as const`, `const obj = {...} as const` |
| L98 | Record Type revisited | STUDY | `Record<Status, number>` với custom enum key |
| L99 | satisfies | STUDY | `satisfies` vs `as` — when to prefer which |

**M07 Challenge:** Tạo `practice/m07-adv-types.ts` — Redux-style action reducer với discriminated unions + type guards + overloads.

### M08 — Generics (L100-109): DEEP_DIVE

| Lecture | Title | Action | Practice |
|---|---|---|---|
| L100 | Intro | SKIP | — |
| L101 | Generic Types We Know | STUDY | `Array<T>`, `Promise<T>` — use built-ins |
| L102 | Understanding Generics | DEEP_DIVE | Viết `identity<T>(val: T): T`, explain T inference |
| L103 | Creating Generic Type | DEEP_DIVE | Generic `Box<T>`, `Pair<A,B>` |
| L104 | Generic Functions & Inference | DEEP_DIVE | `merge<T, U>(a: T, b: U): T & U` |
| L105 | Multiple Generic Parameters | STUDY | 3-param generic function |
| L106-107 | Constraints | DEEP_DIVE | `<T extends { length: number }>`, `<T extends keyof U>` |
| L108 | Generic Classes & Interfaces | DEEP_DIVE | Generic `Stack<T>`, `Repository<T>` interface |
| L109 | Summary | SKIM | — |

**M08 Challenge:** Tạo `practice/m08-generics.ts` — Generic `EventEmitter<T>` class với `on/off/emit` methods, type-safe callbacks.

### M09 — Generic Project (L110-117): BUILD

| Lecture | Action | Practice |
|---|---|---|
| L110 | SKIP | — |
| L111-117 | BUILD | Đọc code linked list → implement `DoublyLinkedList<T>` from scratch |

### M10 — Type Utilities (L118-132): DEEP_DIVE

| Lecture | Title | Action | Practice |
|---|---|---|---|
| L118 | Intro | SKIP | — |
| L119-121 | typeof | DEEP_DIVE | `typeof config`, `ReturnType<typeof fn>`, `InstanceType<typeof Cls>` |
| L122-123 | keyof | DEEP_DIVE | `keyof User`, `type Getter<T> = { [K in keyof T]: () => T[K] }` |
| L124-125 | Indexed Access | DEEP_DIVE | `User["name"]`, `User[keyof User]` |
| L126-127 | Mapped Types | DEEP_DIVE | Tự implement `Readonly<T>`, `Partial<T>`, `Required<T>` |
| L128 | Template Literal | STUDY | `type EventName = \`on${Capitalize<string>}\`` |
| L129-130 | Conditional Types | DEEP_DIVE | `T extends string ? "string" : "other"` — 3 examples |
| L131 | infer | DEEP_DIVE | `infer R` trong conditional types, `ReturnType<T>` manual impl |
| L132 | Built-in Utilities | STUDY | Practice: `Pick`, `Omit`, `Exclude`, `Extract`, `NonNullable` |

**M10 Challenge:** Tạo `practice/m10-utilities.ts` — Implement 5 utility types từ đầu: `DeepPartial<T>`, `DeepReadonly<T>`, `FlattenObject<T>`, `PickByValue<T, V>`, `RequireAtLeastOne<T>`.

### M11 — ECMAScript Decorators (L133-145): STUDY

| Lecture | Action | Practice |
|---|---|---|
| L133-134 | STUDY | Đọc transcript về motivation |
| L135-145 | STUDY | Implement: class decorator `@singleton`, method decorator `@log` |

### M12 — Experimental Decorators (L146-161): SKIM

Xem qua, note differences vs ECMAScript decorators.

### M13 — DnD Project (L162-181): BUILD (quan trọng nhất!)

| Lecture | Action | Practice |
|---|---|---|
| L162 | SKIP | — |
| L163-181 | BUILD | Dùng starter trong `resources/163-prj-00-initial-starting-setup/` |

**Goal:** Complete drag-and-drop project INDEPENDENTLY trước khi xem solution.

**Build steps:**
1. Đọc L163-165 transcript (hình dung UI) → build form
2. L166-168: Implement validation without looking at course code
3. L169-172: Add project sections + state management
4. L173-175: Inheritance + DnD interface
5. L176-179: Visual feedback + finishing

### M14 — Modules (L182-193): STUDY

| Lecture | Action | Practice |
|---|---|---|
| L182-191 | STUDY | Convert 1 file project → multi-file with ES modules |
| L192-193 | SKIP | — |

### M15 — Build Tools (L194-213): SKIM

| Lecture | Action | Practice |
|---|---|---|
| L194-199 | SKIM | — |
| L200-201 | STUDY | Tạo `.d.ts` file cho 1 small module |
| L202-213 | SKIM | — |

### M16 — 3rd Party (L214-231): STUDY

| Lecture | Action | Practice |
|---|---|---|
| L214-220 | STUDY | Install lodash + @types/lodash, use 3 typed functions |
| L221-223 | STUDY | Implement Zod schema cho form validation |
| L224-231 | SKIM | — |

### M17 — React + TS (L232-245): DEEP_DIVE

| Lecture | Action | Practice |
|---|---|---|
| L232 | SKIP | — |
| L233-245 | DEEP_DIVE | Viết TodoList component với: typed props, useState<T>, useRef<T>, event handlers |

**M17 Challenge:** Typed custom hook `useFetch<T>(url: string): { data: T \| null; loading: boolean; error: string \| null }`.

### M18 — Node + Express (L246-257): DEEP_DIVE

| Lecture | Action | Practice |
|---|---|---|
| L246 | SKIP | — |
| L247-257 | DEEP_DIVE | REST API: typed Request, Response, middleware |

**M18 Challenge:** Express router với typed request body + response — POST `/users`, GET `/users/:id`.

### LEGACY (L258-339): SKIP

---

## Module Plan

| Module | Action | Est. Time | Challenge |
|---|---|---|---|
| M02 | CODE-ALONG | ~4h | `m02-type-system.ts` |
| M06 | CODE-ALONG | ~4h | `m06-classes.ts` |
| M07 | DEEP_DIVE | ~3h | `m07-adv-types.ts` |
| M08 | DEEP_DIVE | ~3h | `m08-generics.ts` |
| M10 | DEEP_DIVE | ~4h | `m10-utilities.ts` |
| M13 | BUILD | ~6h | DnD project |
| M17 | DEEP_DIVE | ~4h | React hook |
| M18 | DEEP_DIVE | ~3h | Express router |
| M04 | BUILD | ~2h | Extended mini project |
| M09 | BUILD | ~2h | DoublyLinkedList |
| M11 | STUDY | ~2h | `@singleton`, `@log` |
| M14 | STUDY | ~2h | Multi-file modules |
| M16 | STUDY | ~2h | Zod + @types |
| M03 | SKIM | ~0.5h | — |
| M12 | SKIM | ~0.5h | — |
| M15 | SKIM | ~0.5h | — |

**Tổng ước tính (practice mode):** ~46h → ~23 ngày × 2h
