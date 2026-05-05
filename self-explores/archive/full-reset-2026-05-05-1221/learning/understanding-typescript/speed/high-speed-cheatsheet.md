---
type: cheatsheet
mode: speed
course_slug: understanding-typescript
created: 2026-05-04
---

# High-Speed Cheatsheet — Understanding TypeScript

## 80/20 Concepts (10 concepts làm được 80% công việc TS)

| # | Concept | Why It Matters | Use Case |
|---|---|---|---|
| 1 | Type inference | TS tự biết kiểu — không cần annotate mọi thứ | Mọi nơi |
| 2 | Union types `A \| B` | Giá trị có thể là nhiều kiểu | Props, return values, state |
| 3 | Literal types | Kiểu chính xác là 1 giá trị cụ thể | "admin" \| "user", const |
| 4 | Interface vs Type Alias | Interface: extendable, mergeable. Type: flexible | Object shapes & union |
| 5 | Generic types `<T>` | Code reusable + type-safe | Functions, classes, components |
| 6 | keyof / typeof | Lấy kiểu từ object, type từ value | Mapped types, utility |
| 7 | Discriminated unions | Pattern nhận dạng variant | Action reducers, state machines |
| 8 | Type guards | Narrowing xuống kiểu cụ thể | if/switch với union |
| 9 | Mapped types | Transform từng key của một type | Custom utilities |
| 10 | Conditional types | Type phụ thuộc vào điều kiện | infer, Extract, Exclude |

## Skip List

| Lectures | Reason |
|---|---|
| L1-9 | Setup/intro — biết JS rồi |
| L58-66 | Modern JS — biết rồi |
| L258-339 | Legacy — duplicate của M02-M08 |
| L146-161 | Experimental decorators — legacy API |
| L194-213 | Build tools — dùng Vite/tsc trực tiếp |

## Must-Know Syntax (1 trang)

```typescript
// Type annotation
let name: string = "Alice";        // explicit
let age = 25;                       // inferred — number

// Union
type ID = string | number;

// Literal
type Dir = "left" | "right" | "up" | "down";

// Interface
interface User {
  id: number;
  name: string;
  role?: "admin" | "user";          // optional
  readonly createdAt: Date;         // immutable
}

// Type alias
type Point = { x: number; y: number };

// Generic function
function first<T>(arr: T[]): T | undefined {
  return arr[0];
}

// keyof
type UserKeys = keyof User;         // "id" | "name" | "role" | "createdAt"

// Mapped type
type Optional<T> = { [K in keyof T]?: T[K] };

// Discriminated union
type Result<T> =
  | { ok: true; data: T }
  | { ok: false; error: string };

// Type guard
function isString(val: unknown): val is string {
  return typeof val === "string";
}

// as const
const ROLES = ["admin", "user"] as const;
type Role = typeof ROLES[number];   // "admin" | "user"

// Conditional type
type NonNullable<T> = T extends null | undefined ? never : T;
```

## Mental Models

```
TS type system = set theory
  string | number  → union of sets (OR)
  string & number  → intersection (AND) — rarely possible for primitives
  never            → empty set
  unknown          → universal set (superset of all)

Generic T = "placeholder" — filled in at call site
  function box<T>(val: T): { value: T }
  box(42) → T = number
  box("hi") → T = string

keyof + mapped type = transform every key
  type Readonly<T> = { readonly [K in keyof T]: T[K] }
  Works for any shape T
```
