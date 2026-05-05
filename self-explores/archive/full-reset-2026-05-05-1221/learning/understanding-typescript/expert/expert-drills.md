---
type: expert-drills
course_slug: understanding-typescript
mode: expert
created: 2026-05-04
---

# Expert Drills — Understanding TypeScript

Các bài tập level expert: không có đáp án đơn giản, yêu cầu hiểu sâu về type system.

---

## Drill 1: Type Widening Problems

Tìm lỗi và fix cho mỗi example:

```typescript
// Problem 1a: Widening on let
let status = "active"; // TypeScript widens to string, not "active"
type AllowedStatus = "active" | "inactive";
function setStatus(s: AllowedStatus) {}
setStatus(status); // ❌ Why? How to fix without `as`?

// Problem 1b: Widening in return
function getDirection() {
  return "left"; // returns string, not "left"
}
type Direction = "left" | "right";
const d: Direction = getDirection(); // ❌ Fix?

// Problem 1c: Object literal widening
const config = {
  method: "GET" // string, not "GET"
};
fetch("/api", config); // ❌ method should be "GET" | "POST" | ...
// Two ways to fix: const assertion vs type annotation — which is better?
```

**Expected answers:**
- 1a: `const status = "active"` or `let status: AllowedStatus = "active"` 
- 1b: `function getDirection(): Direction { ... }` or `as const`
- 1c: `{ method: "GET" } as const` preserves narrow type; `{ method: "GET" as const }` per-field

---

## Drill 2: Structural Typing Surprises

```typescript
// What does TypeScript allow here and why?
interface Cat { meow(): void }
interface Dog { bark(): void }

function petTheCat(cat: Cat) { cat.meow(); }

const strangeAnimal = {
  meow() { console.log("meow"); },
  bark() { console.log("bark"); }  // extra property
};

petTheCat(strangeAnimal); // ✅ or ❌?

// Compare with:
petTheCat({ meow() {}, bark() {} }); // ✅ or ❌?
// Excess property check only applies to object literals!
```

**Write 3 examples where structural typing causes unexpected compatibility.**

---

## Drill 3: Function Type Variance

```typescript
// Covariant vs Contravariant — explain why each is valid or invalid

// Return type is covariant (narrower return OK)
type GetAnimal = () => Animal;
type GetDog = () => Dog;
// Can GetDog be used where GetAnimal is expected? YES or NO? Why?

// Parameter type is contravariant (wider param OK)
type HandleAnimal = (animal: Animal) => void;
type HandleDog = (dog: Dog) => void;
// Can HandleDog be used where HandleAnimal is expected? YES or NO? Why?

// Method shorthand is bivariant (TypeScript design decision)
interface A { method(x: Animal): void }
interface B extends A { method(x: Dog): void } // should this be allowed?
```

**Write a concrete example where bivariance causes a runtime bug.**

---

## Drill 4: Implement Built-in Utilities

Implement from scratch (no looking at lib.d.ts):

```typescript
// 1. ReturnType<T>
type MyReturnType<T extends (...args: any) => any> = // your impl

// 2. Parameters<T>
type MyParameters<T extends (...args: any) => any> = // your impl

// 3. ConstructorParameters<T>
type MyConstructorParameters<T extends abstract new (...args: any) => any> = // your impl

// 4. Awaited<T> — unwrap nested Promises
// Awaited<Promise<Promise<string>>> → string
type MyAwaited<T> = // your impl

// 5. NoInfer<T> — prevent inference from a position
// Used in TS 5.4: function createState<T>(initial: T, validate: (v: NoInfer<T>) => boolean)
type MyNoInfer<T> = // your impl
```

---

## Drill 5: Typesafe `pipe()`

```typescript
// Goal: pipe with full inference — no manual type annotation at call site
// pipe(f, g, h)(x) where f: A→B, g: B→C, h: C→D should infer (x: A) => D

// Starter:
function pipe<A, B>(f: (a: A) => B): (a: A) => B;
function pipe<A, B, C>(f: (a: A) => B, g: (b: B) => C): (a: A) => C;
function pipe<A, B, C, D>(f: (a: A) => B, g: (b: B) => C, h: (c: C) => D): (a: A) => D;
// ...up to N functions

// Challenge: Can you do it with a variadic tuple type?
// (TypeScript 4.0+)
```

---

## Drill 6: Typesafe `match()` Function

```typescript
// Implement a type-safe match function similar to Rust's match
// Usage:
const result = match(action, {
  ADD_TODO: (payload: string) => [...state.todos, { text: payload, done: false }],
  TOGGLE_TODO: (id: number) => state.todos.map(t => t.id === id ? {...t, done: !t.done} : t),
  CLEAR_ALL: () => [],
});

// TypeScript should:
// 1. Error if a case is missing (exhaustive)
// 2. Error if payload type is wrong
// 3. Infer return type from handler return types (all must match)
```

---

## Drill 7: Deep Path Access

```typescript
// Implement deepGet with full type inference:
// deepGet({a: {b: {c: 42}}}, "a.b.c") → number (not any, not unknown)

type DeepGet<T, P extends string> =
  P extends `${infer K}.${infer Rest}`
    ? K extends keyof T
      ? DeepGet<T[K], Rest>
      : never
    : P extends keyof T
      ? T[P]
      : never;

// Now implement the runtime function with this type:
function deepGet<T, P extends string>(obj: T, path: P): DeepGet<T, P>

// Test:
const user = { profile: { name: "Alice", age: 30 } };
const name = deepGet(user, "profile.name"); // should be string
const age = deepGet(user, "profile.age"); // should be number
const bad = deepGet(user, "profile.nope"); // should be never (TypeScript error)
```

---

## Drill 8: Generic EventEmitter Type-Safe Subscriptions

```typescript
// Level up: EventEmitter where subscribe returns typed data
// AND unsubscribe is handled automatically via WeakRef

type EventMap = {
  "user:login": { userId: string; timestamp: Date };
  "user:logout": { userId: string };
  "error": { message: string; code: number };
};

class TypedEventEmitter<Events extends Record<string, object>> {
  // Implement:
  // on<K extends keyof Events>(event: K, handler: (data: Events[K]) => void): () => void
  //   returns an unsubscribe function
  // emit<K extends keyof Events>(event: K, data: Events[K]): void
  // once<K extends keyof Events>(event: K): Promise<Events[K]>
  //   returns a promise that resolves on next event
}
```

---

## Drill 9: Enum vs String Literal — When to Use Which

**Answer with code examples:**

1. When would `enum Status { Active = "active", Inactive = "inactive" }` be better than `type Status = "active" | "inactive"`?
2. What does `const enum` compile to vs regular `enum`?
3. What is the "enum pitfall" with numeric enums and how does it cause bugs?
4. What is declaration merging for enums?

---

## Drill 10: TypeScript Compiler Error Literacy

**Explain in plain language (no Googling) what each error means and how to fix it:**

```
Error 1: Type 'string' is not assignable to type 'never'
Error 2: Property 'x' does not exist on type 'A | B'
Error 3: Argument of type '(x: Dog) => void' is not assignable to parameter of type '(x: Animal) => void'
Error 4: Type 'readonly string[]' is not assignable to type 'string[]'
Error 5: Object literal may only specify known properties, and 'extra' does not exist in type 'Config'
Error 6: This expression is not callable. Type 'never' has no call signatures
Error 7: Type 'T' does not satisfy the constraint 'object'
Error 8: Cannot find name 'Awaited'. Do you need to change your target library? Try changing the `lib` compiler option to es2021 or later
```
