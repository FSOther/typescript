---
type: module-guide
mode: speed
module: M06
course_slug: understanding-typescript
lectures: L88-L99
sessions: S06
created: 2026-05-06
---

# M06 — Advanced Types (L88–L99)

## 1. Key Concepts

Advanced types are about making TypeScript smarter at narrowing and making illegal states unrepresentable.

**Discriminated unions:** A union where each member has a common `kind` (or `type`) field that uniquely identifies it.
```typescript
type Shape = 
  | { kind: 'circle'; radius: number }
  | { kind: 'square'; side: number };

function area(s: Shape) {
  switch (s.kind) {
    case 'circle': return Math.PI * s.radius ** 2;
    case 'square': return s.side ** 2;
  }
}
```

**Type guards:** Functions or expressions that narrow a type at runtime.
- `typeof x === 'string'` — primitive narrowing
- `x instanceof MyClass` — class instance narrowing
- Custom type guard: `function isUser(x: unknown): x is User { return ... }`

**Intersection types (`&`):** Combine multiple types into one.
```typescript
type Admin = User & { role: 'admin' };
```

**`satisfies` keyword (TS 4.9+):** Validate an expression against a type without changing the inferred type.
```typescript
const config = {
  host: 'localhost',
  port: 8080,
} satisfies Record<string, string | number>;
// config.host is inferred as 'localhost' (literal), not string
```

## 2. Code Patterns

**Custom type guard function:**
```typescript
interface User { id: string; name: string; }
interface Admin extends User { role: 'admin'; }

function isAdmin(user: User): user is Admin {
  return 'role' in user && user.role === 'admin';
}

const user: User | Admin = getUser();
if (isAdmin(user)) {
  console.log(user.role); // narrowed to Admin
}
```

**Exhaustive check with `never`:**
```typescript
function assertNever(x: never): never {
  throw new Error(`Unhandled case: ${x}`);
}

function processShape(s: Shape) {
  switch (s.kind) {
    case 'circle': return area(s);
    case 'square': return area(s);
    default: return assertNever(s); // TS error if Shape has an unhandled case
  }
}
```

**Intersection for composed types:**
```typescript
type LoggableUser = User & {
  lastLogin: Date;
  logAction: (action: string) => void;
};
```

## 3. Common Gotchas

**Gotcha 1: `typeof null === 'object'`**
JavaScript's `typeof null` returns `'object'` (historical bug). Always check `x !== null` separately when narrowing object types.

**Gotcha 2: `in` operator narrowing**
`'role' in user` narrows to the type that has `role` property. But `in` only checks property existence — it doesn't verify the type. Use this carefully.

**Gotcha 3: Intersection can create impossible types**
`type X = string & number` is technically valid TypeScript but is `never` — no value can be both string and number. Use intersections for object types, not primitives.

**Gotcha 4: Discriminated union requires a COMMON field**
For TypeScript to narrow discriminated unions, all union members MUST have the same discriminant field (e.g., all must have `kind`). If one member is missing it, TypeScript can't narrow.

**Gotcha 5: `satisfies` doesn't change the type**
`satisfies` validates against a type without widening or narrowing. The variable keeps its inferred (precise) type. Different from `as Type` (which forces the type).

## 4. Active Recall Questions

*(No answers here — answer from memory.)*

**Q1:** What is a discriminated union and how does TypeScript use it for narrowing? Write a minimal example from memory.

**Q2:** Write a custom type guard function that checks if an unknown value is a `User` object (has `id: string` and `name: string`).

**Q3:** What is the `never` type? Give 2 use cases where `never` appears in TypeScript code.

## 5. Quick Reference

| Pattern | Syntax | Purpose |
|---------|--------|---------|
| Union | `A \| B` | Either A or B |
| Intersection | `A & B` | Both A and B |
| Type guard | `x is T` return type | Narrow at call site |
| Discriminated union | `{ kind: 'a' } \| { kind: 'b' }` | Narrow by discriminant field |
| `satisfies` | `expr satisfies Type` | Validate without changing inferred type |
| Exhaustive | `assertNever(x: never)` | Catch unhandled union members |

## 6. Done Criteria

Session complete when you can:
1. Write a discriminated union with 3 variants and a function that handles all cases with exhaustiveness check
2. Write a custom type guard function (`x is T`) from memory
3. Explain when `never` appears in your code
