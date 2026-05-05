# P06 — Generics Hands-On

**Practice module** | Mastery target: L2→L4 | ~90 min

---

## 1. Build Task

> **No solution provided.** Implement from spec only — no looking at TypeScript's lib.d.ts.

**Objective:** Implement common TypeScript utility types from scratch to understand how they work internally.

**Spec — implement all of these in `my-utils.ts`:**

1. `type MyPartial<T>` — makes all properties of T optional
2. `type MyRequired<T>` — makes all properties of T required (removes optionality)
3. `type MyReadonly<T>` — makes all properties of T readonly
4. `type MyPick<T, K extends keyof T>` — creates a type with only the keys K from T
5. `type MyOmit<T, K extends keyof T>` — creates a type with all keys of T EXCEPT K
6. `type MyRecord<K extends keyof any, V>` — creates a type with keys K and values V
7. `type MyExclude<T, U>` — removes U members from union T
8. `type MyExtract<T, U>` — keeps only T members that extend U

Then implement two generic functions:

9. `function pick<T extends object, K extends keyof T>(obj: T, keys: K[]): MyPick<T, K>` — returns new object with only specified keys
10. `function omit<T extends object, K extends keyof T>(obj: T, keys: K[]): MyOmit<T, K>` — returns new object without specified keys

**Verify** each utility type with at least one usage example that would catch a type error if the implementation is wrong.

---

## 2. Break It

1. In `MyPick`, change `K extends keyof T` to `K extends string` — then call `MyPick<User, 'nonExistentKey'>`. What error do you get? What did the constraint prevent?

2. Remove `extends keyof T` from `MyOmit` — now `MyOmit<User, 'password'>` compiles but what guarantee is lost?

3. In `MyExclude`, try to remove the `? never : T` part and replace with `? false : T`. What breaks?

4. In the `pick` function, change the return type to `object` instead of `MyPick<T, K>`. No error at definition. Write a call site that fails with `object` return type but works with `MyPick<T, K>`.

---

## 3. Fix It

> **Hints only — no solution shown.**

```typescript
// Wrong implementation of Partial-like type
type NotQuitePartial<T> = {
  [K in keyof T]: T[K] | undefined;  // makes value undefined-able but NOT optional
};

interface User { id: number; name: string; }
type PartialUser = NotQuitePartial<User>;
// { id: number | undefined; name: string | undefined; }
// Problem: const u: PartialUser = {}; // Error — id is required (just can be undefined)
```

**Hint 1:** The difference between `T[K] | undefined` and `T[K]?` is the `?` modifier. In a mapped type, how do you add `?` as a modifier?

**Hint 2:** The syntax is `[K in keyof T]?:` — the `?` goes on the left side of the colon, as a key modifier.

---

## 4. Explain It

1. `[K in keyof T]: T[K]` — explain each part: what is `K`, what is `keyof T`, what is `T[K]`?

2. `T extends U ? never : T` in `Exclude<T, U>` — if `T` is a union type `'a' | 'b' | 'c'` and `U` is `'a'`, how does TypeScript evaluate this? What is "distributive conditional types"?

3. `MyRecord<K extends keyof any, V>` — why `keyof any` instead of `string`? What does `keyof any` evaluate to?

---

## 5. Apply It

Build a type-safe `groupBy<T, K extends keyof T>` function:
- Takes an array of T objects and a key K
- Returns `Record<string, T[]>` grouped by the value of key K
- The key must exist on T
- Values must be the correct type

Then build `DeepPartial<T>` — a recursive version of `Partial` that also makes nested objects partial.

---

## 6. Acceptance Criteria

```
Given: MyPick<User, 'id' | 'name'>
When: accessing .email on the result
Then: TypeScript error (email not in pick)

Given: MyExclude<'a' | 'b' | 'c', 'a'>
When: type is inspected
Then: resolves to 'b' | 'c'

Given: pick(user, ['id', 'name'])
When: result.email is accessed
Then: TypeScript error

Given: pick(user, ['nonexistentKey'])
When: TypeScript compiles
Then: compile error (key doesn't exist on User)
```

---

## 7. Resources

- Speed guide: [`M09-type-utilities.md`](../../../speed/module-guides/M09-type-utilities.md)
- Expert guide: [`E06-type-utilities.md`](../../../expert/module-guides/E06-type-utilities.md)
- Reference lectures: L118–L132
