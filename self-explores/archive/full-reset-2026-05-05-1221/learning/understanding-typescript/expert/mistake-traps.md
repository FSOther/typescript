---
type: mistake-traps
course_slug: understanding-typescript
mode: expert
created: 2026-05-04
---

# Mistake Traps — TypeScript Anti-Patterns & Common Mistakes

Những lỗi sai phổ biến và tại sao chúng sai. Mỗi trap có: BAD code → WHY it's wrong → BETTER code.

---

## Trap 1: `any` Abuse

```typescript
// ❌ BAD — any propagates and kills type safety
function processData(data: any) {
  return data.name.toUpperCase(); // runtime crash if name is undefined
}

// ❌ Worse — any escape hatch hides bugs
const result = fetchUser() as any;
result.nonExistentMethod(); // no error, runtime crash

// ✅ BETTER — use unknown for truly unknown inputs
function processData(data: unknown) {
  if (typeof data === "object" && data !== null && "name" in data) {
    const { name } = data as { name: string };
    return name.toUpperCase();
  }
  throw new Error("Invalid data shape");
}

// ✅ BETTER — use type assertion only when you genuinely know the type
const result = fetchUser() as User; // document WHY this is safe
```

**Why it matters:** `any` turns off type checking for that value AND everything derived from it.

---

## Trap 2: `as` Instead of Type Guards

```typescript
// ❌ BAD — casting without verification
function getUser(response: unknown): User {
  return response as User; // might be { error: "not found" }
}

// ✅ BETTER — validate before casting
function isUser(v: unknown): v is User {
  return typeof v === "object" && v !== null
    && "id" in v && "name" in v;
}

function getUser(response: unknown): User {
  if (!isUser(response)) throw new Error("Invalid user response");
  return response;
}
```

---

## Trap 3: Widening on `let`

```typescript
// ❌ TRAP — let widens to string
let direction = "left"; // type: string (not "left")
function move(dir: "left" | "right") {}
move(direction); // ❌ Error: string not assignable to "left" | "right"

// ✅ FIX 1 — const
const direction = "left"; // type: "left" ✅

// ✅ FIX 2 — explicit type
let direction: "left" | "right" = "left"; // ✅

// ✅ FIX 3 — as const on object
const config = { direction: "left" } as const; // config.direction: "left" ✅
```

---

## Trap 4: Numeric Enum Reverse Mapping

```typescript
// ❌ TRAP — numeric enums have a HUGE gotcha
enum Status { Active, Inactive }
// Status[0] === "Active" (reverse mapping!)
// Status["Active"] === 0

// This means:
type AllStatus = keyof typeof Status; // "Active" | "Inactive" | 0 | 1 — unexpected!

// ✅ BETTER — always use string enums
enum Status { Active = "ACTIVE", Inactive = "INACTIVE" }
// No reverse mapping!
// OR even better:
type Status = "active" | "inactive"; // simpler, no runtime object
```

---

## Trap 5: Forgetting That Interfaces Merge

```typescript
// ❌ ACCIDENTAL MERGE — if you have two files both declaring Window
interface Window {
  myCustomProp: string;
}
// In another file:
interface Window {
  anotherProp: number;
}
// TypeScript MERGES them — no error, may cause confusion

// ✅ Use type alias when you DON'T want merging
type MyConfig = { host: string };
// TypeScript ERROR if you re-declare:
type MyConfig = { port: number }; // ❌ Duplicate identifier

// ✅ Use interface extension intentionally (declaration merging)
// lib.d.ts: interface Array<T> { ... }
// Your augmentation:
interface Array<T> {
  first(): T | undefined; // Add method to Array globally
}
```

---

## Trap 6: Overusing Generic Constraints

```typescript
// ❌ OVERLY CONSTRAINED — limits flexibility unnecessarily
function getLength<T extends { length: number }>(item: T): number {
  return item.length;
}
// This works, but T is unnecessarily preserved when you only need number

// ✅ SIMPLER — constraint is the type itself
function getLength(item: { length: number }): number {
  return item.length;
}

// ✅ GENERICS when you need to PRESERVE the type relationship
function firstElement<T>(arr: readonly T[]): T | undefined {
  return arr[0]; // T must be preserved to return same type as elements
}
```

---

## Trap 7: `!` Non-Null Assertion Without Understanding

```typescript
// ❌ BAD — lying to TypeScript
const el = document.getElementById("app")!;
el.appendChild(child); // crashes if el is actually null

// ❌ ALSO BAD — chaining !
function process(user?: User) {
  return user!.profile!.name!; // any of these can crash
}

// ✅ BETTER — narrow properly
const el = document.getElementById("app");
if (!el) throw new Error("App element not found");
el.appendChild(child); // el: HTMLElement now

// ✅ ACCEPTABLE ! usage — in tests, or when you KNOW the element exists
// (document written by you, element guaranteed by template):
const el = document.querySelector<HTMLElement>(".my-known-element")!;
// Document the WHY if non-obvious
```

---

## Trap 8: Interface for Function Types (Use Type Alias Instead)

```typescript
// ❌ AWKWARD — interface for function type
interface Formatter {
  (value: string): string; // call signature, not a method
}

// ✅ CLEARER — type alias
type Formatter = (value: string) => string;

// Rule of thumb:
// - Object shape with methods → interface
// - Function type → type alias
// - Union/intersection → type alias (can't do with interface)
// - "Should be extendable by others" → interface
```

---

## Trap 9: Generic Classes That Aren't Really Generic

```typescript
// ❌ USELESS GENERIC — T isn't actually used for type safety
class Container<T> {
  constructor(private value: any) {} // ← any defeats the purpose!
  getValue(): any { return this.value; }
}

// ✅ ACTUALLY GENERIC
class Container<T> {
  constructor(private value: T) {}
  getValue(): T { return this.value; }
  map<U>(fn: (v: T) => U): Container<U> {
    return new Container(fn(this.value));
  }
}
```

---

## Trap 10: `instanceof` on Interfaces

```typescript
// ❌ RUNTIME ERROR — interfaces don't exist at runtime
interface Serializable { serialize(): string; }
if (value instanceof Serializable) { ... } // ❌ Error: 'Serializable' only refers to a type

// ✅ FIX 1 — use abstract class when you need instanceof
abstract class Serializable { abstract serialize(): string; }
if (value instanceof Serializable) { ... } // ✅

// ✅ FIX 2 — use type predicate
function isSerializable(v: unknown): v is Serializable {
  return typeof v === "object" && v !== null && "serialize" in v
    && typeof (v as any).serialize === "function";
}
```

---

## Trap 11: Mutable Default in Mapped Type

```typescript
// ❌ TRAP — mapped type copies mutability
type Copy<T> = { [K in keyof T]: T[K] };
type ReadonlyUser = Readonly<User>;
type CopyOfReadonlyUser = Copy<ReadonlyUser>;
// CopyOfReadonlyUser is still readonly!
// Mapped types preserve readonly modifier by default

// ✅ If you want to strip readonly:
type Mutable<T> = { -readonly [K in keyof T]: T[K] };

// ✅ If you want to strip optional:
type Required<T> = { [K in keyof T]-?: T[K] };
```

---

## Trap 12: `keyof any` vs `string | number | symbol`

```typescript
// These are equivalent:
type K1 = keyof any;           // string | number | symbol
type K2 = string | number | symbol;

// This matters for generic index signatures:
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key]; // K is a valid key of T
}

// ❌ WRONG — over-broad constraint
function getProperty<T, K extends string>(obj: T, key: K): any {
  return (obj as any)[key]; // loses type safety
}
```

---

## Trap 13: Conditional Types Don't Distribute Over Non-Unions

```typescript
// Distributive conditional type (T is naked type parameter):
type IsString<T> = T extends string ? true : false;
type R1 = IsString<string | number>; // boolean (true | false) — distributes!

// Non-distributive (T wrapped in tuple):
type IsString2<T> = [T] extends [string] ? true : false;
type R2 = IsString2<string | number>; // false (treats union as whole)

// When to use non-distributive:
// When you want to check "is this type exactly string" vs "does this union contain string"
```

---

## Anti-Pattern Reference Card

| Anti-Pattern | Why Bad | Fix |
|---|---|---|
| `any` everywhere | Disables type checking | `unknown` + type guards |
| `as Type` without verification | Silent runtime crashes | Type predicates |
| `let x = "active"` for literals | Widens to `string` | `const` or explicit type |
| Numeric enum | Reverse mapping, bitwise confusion | String enum or literal union |
| `!` without knowing why | Crashes on null | Null check + narrowing |
| `interface` for function type | Awkward call signature | `type alias = () => void` |
| `instanceof Interface` | Interfaces are erased | Abstract class or type predicate |
| Generic with `any` inside | Pointless generic | Use T throughout |
| Over-constrained generic | Reduces reusability | Only constrain what you use |
| Widening in mapped type | Mutability unexpectedly preserved | Use `-readonly` modifier explicitly |
