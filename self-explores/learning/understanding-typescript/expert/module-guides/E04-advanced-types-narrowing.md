# E04 — Advanced Types & Narrowing

**Expert module** | Covers: L88–L99 | ~65 min

---

## 1. Core Mental Model

> "Type narrowing is TypeScript's model of what values are possible at a given point in control flow. Every `if`, `typeof`, `instanceof`, or user-defined guard reduces the set of possible types. Exhaustiveness checking is when the type system proves you've handled all cases."

Expert insight: discriminated unions aren't just "a pattern" — they're a way to **encode state machines in the type system** so impossible states are impossible to represent.

---

## 2. Beginner Misunderstanding

> "I use type guards, but I still get `any` in my else branch. I add another check."

The problem: forgetting `assertNever` in the default branch of a union switch. Without it, adding a new union member silently falls through. The else branch is not "remaining cases" — it's "cases you forgot to handle."

---

## 3. Professional Concern

> "We have 15 different `if (response.status === 'error')` scattered throughout the codebase. Adding a new status means updating 15 places and hoping you found them all."

The solution: centralize the union type + switch/match to one location. The compiler makes every add-site visible. This is called "making illegal states unrepresentable."

---

## 4. Mistake Traps (no solution shown)

**Trap 1: Structural check without discriminant**
```typescript
type Cat = { meow(): void; };
type Dog = { bark(): void; };
type Animal = Cat | Dog;

function makeNoise(animal: Animal) {
  if ('meow' in animal) {
    animal.meow();
  } else {
    animal.bark(); // What happens if Animal gains a third type?
  }
}
// What's missing that would catch a new Animal type at compile time?
```

**Trap 2: Mixing `|` intersection with `&`**
```typescript
type A = { x: string };
type B = { y: number };
type Both = A | B;  // must have x OR y
type Either = A & B; // must have x AND y
// Which one should you use for a function that requires both fields?
```

**Trap 3: User-defined type guard returns wrong type**
```typescript
function isError(val: unknown): val is Error {
  return typeof val === 'string'; // BUG: returns true for strings, claims Error
}
// TypeScript trusts this. What runtime behavior does this create?
```

---

## 5. Expert Shortcut

**Discriminated union + exhaustiveness pattern:**
```typescript
type Shape =
  | { kind: 'circle'; radius: number }
  | { kind: 'square'; side: number }
  | { kind: 'triangle'; base: number; height: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case 'circle': return Math.PI * shape.radius ** 2;
    case 'square': return shape.side ** 2;
    case 'triangle': return 0.5 * shape.base * shape.height;
    default:
      const _exhaustive: never = shape; // compile error if new Shape added
      throw new Error(`Unhandled: ${JSON.stringify(_exhaustive)}`);
  }
}
```

**Branded types for semantic safety:**
```typescript
type UserId = string & { readonly _brand: 'UserId' };
type OrderId = string & { readonly _brand: 'OrderId' };

function getUser(id: UserId) { ... }
getUser('abc' as UserId);           // explicit cast required
// getUser(orderId);                // compile error even though both are strings
```

---

## 6. Real-world Scenario

You're building a payment flow. States: `Pending → Processing → Succeeded | Failed`. Each state has different data. A bug report says "sometimes we call `chargeCard()` on an already-succeeded payment."

How would you model this as a discriminated union so the function signature makes `chargeCard(succeeded)` a compile error?

---

## 7. Deliberate Drill

**Constraint:** Rewrite this function to use a discriminated union with exhaustiveness — **no `if/else`, no `switch` without `default: assertNever`**:
```typescript
function formatResult(result: { success: boolean; data?: string; error?: string }) {
  if (result.success) {
    return `OK: ${result.data}`;
  } else {
    return `Error: ${result.error}`;
  }
}
```

New requirement: add a third case `{ success: 'partial'; data: string; warnings: string[] }`. Show that your new union forces callers to handle the partial case.

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can write `assertNever` and explain why the `never` parameter makes it work
- [ ] Know the difference between `|` (union) and `&` (intersection) semantically
- [ ] Can write a correct user-defined type guard with `x is T`
- [ ] Understand why discriminated unions are better than `{ success: boolean }` shapes

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. You have `type Status = 'active' | 'inactive' | 'pending'`. A new `'suspended'` status is added. What change to your switch statement would cause a compile error if you forget to handle the new case?

2. What is the difference between `instanceof` narrowing and `'key' in object` narrowing? When would one fail but the other succeed?

3. Explain the "impossible state" problem with this type: `{ isLoggedIn: boolean; user?: User }`. What discriminated union would make the impossible state (logged in but no user) unrepresentable?
