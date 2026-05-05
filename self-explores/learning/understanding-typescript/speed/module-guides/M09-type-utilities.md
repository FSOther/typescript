---
type: module-guide
mode: speed
module: M09
course_slug: understanding-typescript
lectures: L118-L132
sessions: S08
created: 2026-05-06
---

# M09 — Type Utilities & Mapped Types (L118–L132)

## 1. Key Concepts

Type utilities let you transform types programmatically. The built-in utilities like `Partial<T>`, `Readonly<T>`, and `ReturnType<F>` appear everywhere in library code — understanding how they work lets you read and write them.

**`typeof` in type position:** Get the type of a VALUE.
```typescript
const config = { host: 'localhost', port: 3000 };
type Config = typeof config; // { host: string; port: number }
```

**`keyof`:** Get the union of keys of a type.
```typescript
type UserKeys = keyof User; // 'id' | 'name' | 'email'
```

**Indexed access types:** Get the type of a specific property.
```typescript
type ID = User['id']; // string
type ArrayItem = string[0]; // string (element type of string array)
```

**Mapped types:** Transform every key in a type.
```typescript
type Optional<T> = { [K in keyof T]?: T[K] }; // same as Partial<T>
type Immutable<T> = { readonly [K in keyof T]: T[K] }; // same as Readonly<T>
```

**Conditional types:** Type-level if/else.
```typescript
type IsString<T> = T extends string ? true : false;
type Flatten<T> = T extends Array<infer Item> ? Item : T;
```

**`infer` keyword:** Extract a type within a conditional type.
```typescript
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
type ElementType<T> = T extends (infer E)[] ? E : never;
```

## 2. Code Patterns

**Built-in utility types you must know:**
```typescript
type Partial<T>     = { [K in keyof T]?: T[K] };           // all optional
type Required<T>    = { [K in keyof T]-?: T[K] };           // all required
type Readonly<T>    = { readonly [K in keyof T]: T[K] };    // all readonly
type Pick<T, K extends keyof T> = { [P in K]: T[P] };       // select keys
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>; // remove keys
type Record<K extends string, T> = { [P in K]: T };         // key → value map
type ReturnType<F extends (...args: any[]) => any> = ...;   // fn return type
type Parameters<F extends (...args: any[]) => any> = ...;   // fn params tuple
```

**Practical patterns:**
```typescript
// Update payload: id required, rest optional
type UpdateUser = Pick<User, 'id'> & Partial<Omit<User, 'id'>>;

// Extract return type of existing function
const createUser = (name: string, email: string): User => ...;
type CreatedUser = ReturnType<typeof createUser>; // User

// Extract array element type
type Users = User[];
type OneUser = Users[number]; // User
```

## 3. Common Gotchas

**Gotcha 1: `typeof` in type vs value position**
`typeof x` in a VALUE context returns a runtime string ('string', 'number', etc.).
`typeof x` in a TYPE context (after `:` or in a type alias) returns the TypeScript type.

**Gotcha 2: `keyof` on a union gives the intersection of keys**
```typescript
type AB = { a: 1 } | { b: 2 };
type Keys = keyof AB; // 'a' | 'b' → actually NEVER (no keys common to both)
// Actually: keyof (A | B) = (keyof A) & (keyof B)
```

**Gotcha 3: `infer` only works in conditional types**
`infer R` can only appear in the `extends` clause of a conditional type. You can't use it outside.

**Gotcha 4: Mapped type modifiers**
`-?` removes optional. `-readonly` removes readonly. `+?` adds optional (same as `?`). These are rarely used but appear in `Required<T>` implementation.

## 4. Active Recall Questions

*(No answers here — answer from memory.)*

**Q1:** What does `Partial<User>` produce? Write its implementation using a mapped type from memory.

**Q2:** Write a type `MyReturnType<F>` that extracts the return type of a function — without using TypeScript's built-in `ReturnType`. Use `infer`.

**Q3:** What is `keyof T`? If `T = { name: string; age: number }`, what is `keyof T`?

## 5. Quick Reference

| Utility | Effect |
|---------|--------|
| `Partial<T>` | All properties optional |
| `Required<T>` | All properties required |
| `Readonly<T>` | All properties readonly |
| `Pick<T, K>` | Only include keys K from T |
| `Omit<T, K>` | Exclude keys K from T |
| `Record<K, V>` | Object type with keys K and values V |
| `ReturnType<F>` | Return type of function F |
| `Parameters<F>` | Parameters of function F as tuple |
| `Exclude<T, U>` | Remove U from union T |
| `Extract<T, U>` | Keep only types assignable to U |

## 6. Done Criteria

Session complete when you can:
1. Explain the difference between `Partial<T>` and `Omit<T, K>` with examples
2. Write a `Partial<T>` implementation using mapped types from memory
3. Use `infer` to extract the element type of an array type
