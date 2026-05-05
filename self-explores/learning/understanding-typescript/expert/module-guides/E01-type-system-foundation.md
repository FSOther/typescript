# E01 — Type System Foundation

**Expert module** | Covers: L13–L39 | ~60 min

---

## 1. Core Mental Model

> "The type system is a constraint language. You are encoding invariants the compiler enforces at compile time. Every type annotation is a claim about what the program can do — not documentation."

The shift from beginner to expert: stop asking *"how do I satisfy the type checker?"* and start asking *"what invariant am I asserting, and is that invariant actually true?"*

**Type narrowing is the engine:** TypeScript doesn't know your runtime values, but it narrows its model of what's possible as you add checks. This is not magic — it's set-theoretic inference over control flow.

---

## 2. Beginner Misunderstanding

> "I use `any` when TypeScript gets complicated — it's just to silence the error."

What `any` actually does: it **opts out of the type system entirely** for that value, including all values derived from it. `any` is viral — `someAny.field` is `any`, `fn(someAny)` returns `any`. One `any` silences 10 future errors.

The correct model: when you don't know the type, use `unknown`. It forces you to narrow before using the value — exactly what you want when handling external data.

---

## 3. Professional Concern

> "Our codebase has 400 `@ts-ignore` comments and 200 `as any` casts. TypeScript is giving us a false sense of safety."

This is real. TypeScript's value is proportional to how much of the type graph is accurate. A project with `any` at the boundary (API responses, form data) means the entire downstream is untyped in practice. The professional solution: **validate at boundaries** (Zod, io-ts) and propagate real types inward.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: Using `as` to assert instead of to narrow**
```typescript
const el = document.getElementById('app') as HTMLDivElement;
// Problem: what if the element doesn't exist or is wrong type?
// What should you do before asserting?
```

**Trap 2: `union | undefined` vs optional property**
```typescript
interface Config {
  timeout: number | undefined;  // must always be present (even if undefined)
  retries?: number;             // may be absent entirely
}
// When does the difference matter at runtime?
```

**Trap 3: Widening on assignment**
```typescript
const status = 'active';          // type: "active" (literal)
let mutableStatus = 'active';    // type: string (widened)
// Why does this bite you with discriminated unions?
```

---

## 5. Expert Shortcut

**`satisfies` operator (TypeScript 4.9+) — get narrowness AND type safety:**
```typescript
// Problem: 'as const' loses type checking, type annotation loses narrowness
const config = {
  timeout: 5000,
  retries: 3,
} satisfies Partial<AppConfig>;
// config.timeout is narrowed to 5000, but config must be a valid Partial<AppConfig>
```

**Discriminated union exhaustiveness with `assertNever`:**
```typescript
function assertNever(x: never): never {
  throw new Error(`Unhandled case: ${JSON.stringify(x)}`);
}
// Drop this in the default branch of any switch over a union type
// When a new union member is added, the default branch becomes a compile error
```

---

## 6. Real-world Scenario

You're building a webhook handler. The payload can be `UserCreated | OrderPlaced | PaymentFailed`. Each has a `type` discriminant. Your handler currently uses `if/else if/else`. A new event type is added, and the `else` branch silently runs the wrong code in production for 3 days.

How would you restructure the handler to make this impossible at compile time?

*(Think through your answer before opening `/viec hoc`)*

---

## 7. Deliberate Drill

**Constraint:** Rewrite this function with **no `any`, no `as`, no `!`**:
```typescript
function getNestedValue(obj: any, path: string): any {
  return path.split('.').reduce((acc, key) => acc[key], obj);
}
```

Rules:
- The function should be generic
- Return type should reflect actual possible values
- If a key doesn't exist, return `undefined` safely

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can explain why `unknown` is safer than `any` (one sentence, no lookup)
- [ ] Can write a type guard function `isString(x: unknown): x is string`
- [ ] Can write a discriminated union with `assertNever` exhaustiveness check
- [ ] Understand when `satisfies` is preferable to `: Type` annotation

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. TypeScript infers `let x = []` as `never[]`. You push a string and it becomes `string[]`. But you want `Array<string | number>`. Where does TypeScript's inference fail here, and how do you communicate your intent?

2. You have `type Result<T> = { ok: true; data: T } | { ok: false; error: string }`. A colleague writes `if (result.ok) { ... }` without an `else`. What TypeScript feature makes the `if` body have type `{ ok: true; data: T }` automatically?

3. The difference between `interface` and `type` alias matters in one specific situation. What is it, and when would you choose one over the other in a public library API?
