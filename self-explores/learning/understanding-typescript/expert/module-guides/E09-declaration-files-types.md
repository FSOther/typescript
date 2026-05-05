# E09 — Declaration Files & @types

**Expert module** | Covers: L214–L223 | ~55 min

---

## 1. Core Mental Model

> "`@types/*` packages are community-maintained best-effort type declarations. They are NOT part of the library they describe. They can lag, be wrong, or be incomplete for edge cases. Always verify against actual JS library behavior for anything non-trivial."

Expert insight: `.d.ts` files are the bridge between the JavaScript ecosystem (runtime code) and TypeScript's type system (compile-time). Understanding how to write them lets you type any library, extend existing types, and debug type errors that come from incorrect declarations.

---

## 2. Beginner Misunderstanding

> "`req.user` is typed as `any` in Express routes. I just cast it: `const user = req.user as User`. Problem solved."

The problem: this is a runtime lie. TypeScript will happily let you access properties that don't exist. The correct approach: module augmentation to add `user: User` to Express's `Request` interface. Then `req.user` is typed everywhere automatically, and you get compile-time validation.

---

## 3. Professional Concern

> "@types/lodash doesn't include the correct overload for our usage. We get `any` return types on some lodash functions."

This happens. Solutions: (1) write a local module augmentation to override the specific type, (2) contribute the fix to DefinitelyTyped, (3) use a better-typed alternative. "Just cast it" is not a solution — it hides the type unsafety.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: Module augmentation scope**
```typescript
// user-augment.ts
declare module 'express' {
  interface Request {
    user?: User;
  }
}
// This file must be imported somewhere in your app for the augmentation to apply
// What happens if you define this in a .d.ts file that tsconfig doesn't include?
```

**Trap 2: `declare global` vs module augmentation**
```typescript
// In a module file (has import/export)
declare global {
  interface Window {
    myApp: { version: string };
  }
}
// In a .d.ts file without import/export (ambient module)
interface Window { myApp: { version: string }; }
// What's the difference? When does each apply?
```

**Trap 3: Version mismatch between library and @types**
```typescript
// express@5 has different API than express@4
// @types/express might be for v4
// How do you check which version of @types/express you have?
// What breaks at runtime when types are for wrong version?
```

---

## 5. Expert Shortcut

**Module augmentation for Express request:**
```typescript
// src/types/express.d.ts
import { User } from '../models/user';  // must be a module (has import)

declare module 'express' {
  interface Request {
    user?: User;
    requestId: string;
  }
}
```

**Minimal `.d.ts` for an untyped JS library:**
```typescript
// types/legacy-lib.d.ts
declare module 'legacy-lib' {
  export function process(data: string): Promise<{ result: string; status: number }>;
  export const version: string;
  export default class LegacyProcessor {
    constructor(config: { timeout: number });
    run(input: unknown): void;
  }
}
```

**Zod for runtime-to-compile-time bridge:**
```typescript
// API response — unknown at runtime
const UserSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  createdAt: z.string().datetime(),
});

type User = z.infer<typeof UserSchema>;

async function fetchUser(id: string): Promise<User> {
  const raw = await api.get(`/users/${id}`);
  return UserSchema.parse(raw); // throws ZodError if shape is wrong
}
```

---

## 6. Real-world Scenario

You're adding authentication to an Express app. Every authenticated route needs `req.user` to be typed as your `AuthenticatedUser` interface. Currently it's `any`. You have 60 route handlers.

Describe the full solution:
1. Where do you put the module augmentation?
2. How do you ensure it's loaded?
3. What tsconfig setting controls `.d.ts` file inclusion?
4. How do you handle routes that don't require authentication (user might be undefined)?

---

## 7. Deliberate Drill

**Constraint:** Write module augmentation for a fictional `@company/logger` library that currently only exports:
```typescript
// Existing types (from @types/company__logger)
declare module '@company/logger' {
  export function log(msg: string): void;
}
```

You need to add:
1. A `setContext(ctx: Record<string, unknown>): void` method
2. A `LogLevel` enum/union type: `'debug' | 'info' | 'warn' | 'error'`
3. Update `log` to accept an optional `LogLevel` second argument

Write this as module augmentation without modifying the original declaration.

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can write a module augmentation for Express `Request`
- [ ] Know the difference between `declare module` and `declare global`
- [ ] Can write a minimal `.d.ts` for an untyped JS library
- [ ] Understand when `@types/X` is needed vs when a library ships its own types

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. What is DefinitelyTyped? How does `npm install @types/lodash` relate to DefinitelyTyped?

2. You install `@types/express@4` but your `express` is version 5. List two ways this version mismatch could cause runtime bugs that TypeScript doesn't catch.

3. Zod and TypeScript's type system both provide type safety — but at different times. Describe a scenario where TypeScript says "this is fine" but Zod would throw at runtime.
