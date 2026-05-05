# E06 — Type Utilities & Mapped Types

**Expert module** | Covers: L118–L132 | ~70 min

---

## 1. Core Mental Model

> "`Partial<T>`, `Readonly<T>`, `Pick<T, K>` are not magic builtins — they're mapped types you can read in `lib.d.ts`. Once you understand how they work, you can compose custom utilities that capture invariants the builtins can't express."

Expert insight: TypeScript's type utilities operate at the **type level** — they transform type shapes the same way functions transform values. Conditional types (`T extends U ? A : B`) are if/else for types. `infer` lets you pattern-match and extract from type shapes.

---

## 2. Beginner Misunderstanding

> "I use `Partial<User>` for my update endpoint because some fields are optional."

The problem: `Partial<User>` makes ALL fields optional, including `id`. Now TypeScript allows updating a user without knowing which user to update. The correct type for an update payload is `{ id: string } & Partial<Omit<User, 'id'>>` — id is required, everything else optional.

---

## 3. Professional Concern

> "We duplicated 20 type definitions that are just variations of the same base type. When we change the base, we update in 20 places and miss some."

The solution: derive types from each other using mapped types and conditional types. A 5-line custom utility type can replace 20 manual definitions and automatically stay in sync with the source type.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: `keyof` returns wider type than expected**
```typescript
interface User { id: number; name: string; }
type UserKey = keyof User;  // "id" | "name"

function getField<T, K extends keyof T>(obj: T, key: K): T[K] { return obj[key]; }
// What is the return type of getField(user, 'id')? What is it for getField(user, 'name')?
// Why is this better than getField(user: User, key: string): any?
```

**Trap 2: Mapped type modifiers misunderstood**
```typescript
type Mutable<T> = { -readonly [K in keyof T]: T[K] };
type Required<T> = { [K in keyof T]-?: T[K] };
// What does the minus sign do? What does -readonly mean vs readonly?
// Can you write a type that makes only one specific field required?
```

**Trap 3: Conditional type distributivity**
```typescript
type IsString<T> = T extends string ? true : false;
type Test1 = IsString<string>;          // true
type Test2 = IsString<string | number>; // boolean (not false!)
// Why does IsString<string | number> return boolean? What is "distributive conditional types"?
// How do you prevent distribution?
```

---

## 5. Expert Shortcut

**Custom utility types — read the implementation, don't memorize:**
```typescript
// Partial<T> — all fields optional
type Partial<T> = { [K in keyof T]?: T[K] };

// Required<T> — remove optionality
type Required<T> = { [K in keyof T]-?: T[K] };

// ReadOnly<T>
type Readonly<T> = { readonly [K in keyof T]: T[K] };

// Pick<T, K>
type Pick<T, K extends keyof T> = { [P in K]: T[P] };

// Omit<T, K>
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```

**Practical update payload pattern:**
```typescript
type UpdatePayload<T, IdKey extends keyof T = 'id'> =
  Pick<T, IdKey> & Partial<Omit<T, IdKey>>;

type UpdateUser = UpdatePayload<User>;
// { id: string } & { name?: string; email?: string; ... }
```

**Extracting return type with `infer`:**
```typescript
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;
type UnwrappedUser = UnwrapPromise<Promise<User>>; // User
type UnwrappedString = UnwrapPromise<string>;      // string
```

---

## 6. Real-world Scenario

Your codebase has:
```typescript
interface Settings {
  theme: 'light' | 'dark';
  language: string;
  notifications: { email: boolean; push: boolean; };
}
```

You need:
1. A type for the update API: theme and language are optional but notifications must be fully specified if present
2. A type for reading settings from localStorage: all values might be `undefined` (not just optional)
3. A readonly version for the settings context provider

Write all three without duplicating field definitions.

---

## 7. Deliberate Drill

**Constraint:** Write `DeepReadonly<T>` that makes every nested property readonly — **using only mapped types and conditional types, no `as const`**:

```typescript
type DeepReadonly<T> = T extends object
  ? { readonly [K in keyof T]: DeepReadonly<T[K]> }
  : T;

// Verify:
type Config = { server: { host: string; port: number }; debug: boolean };
type FrozenConfig = DeepReadonly<Config>;
// FrozenConfig.server.host should be readonly
// FrozenConfig.debug should be readonly boolean
```

Now extend it to handle arrays: `DeepReadonly<string[]>` should be `ReadonlyArray<string>`.

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can implement `Partial<T>` from scratch with mapped types
- [ ] Know what `infer` does in a conditional type
- [ ] Can write `UpdatePayload<T>` with required ID and optional rest
- [ ] Understand why `IsString<string | number>` distributes to `boolean`

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. `Record<string, User>` vs `{ [key: string]: User }` — are they equivalent? When would you prefer one over the other?

2. You have `type ApiKeys = 'timeout' | 'retries' | 'baseUrl'`. Write `ApiConfig` as a mapped type where each key maps to a `string | number | undefined`. Do this without listing the keys again.

3. The `Exclude<T, U>` utility type is defined as `T extends U ? never : T`. Explain why this removes `U` from a union of types. Why does returning `never` in a union effectively remove that branch?
