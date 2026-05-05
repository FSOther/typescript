---
date: 2026-05-06
type: real-world-scenarios
mode: expert
course_slug: understanding-typescript
generated_by: typescript-50y
---

# Expert Real-world Scenarios — Understanding TypeScript

**10 scenarios. Each requires architectural thinking, not syntax recall.**

---

## Scenario 1: Webhook Handler with Missing Cases

**Context:** You're building a payment processor webhook handler. The payload can be:
```typescript
type WebhookEvent =
  | { type: 'payment.succeeded'; amount: number; currency: string }
  | { type: 'payment.failed'; reason: string }
  | { type: 'subscription.cancelled'; userId: string };
```

A new team member adds `| { type: 'refund.issued'; amount: number }`. Six days later, production refunds are silently ignored.

**Question:** How would you have structured the handler to make this impossible? Show the specific TypeScript construct that would have produced a compile error when the new event type was added.

**Hint area:** Exhaustiveness checking, `assertNever`, discriminated union switch.

---

## Scenario 2: The Growing `any[]` in a Service Layer

**Context:** A `DataService` was written quickly:
```typescript
class DataService {
  private cache: any[] = [];
  
  async fetchUsers(): Promise<any[]> {
    const res = await api.get('/users');
    this.cache = res.data;
    return this.cache;
  }
  
  getById(id: string): any {
    return this.cache.find(item => item.id === id);
  }
}
```

Three months later, a junior developer calls `service.getById('1').email.toUpperCase()` and gets a runtime crash because the user object actually has `emailAddress`, not `email`. TypeScript said nothing.

**Question:** Redesign `DataService` to be fully type-safe. What is the minimum change that catches the `.email` access at compile time? What is the complete solution that validates API responses at runtime?

---

## Scenario 3: The Express Middleware `req.user` Problem

**Context:** Your team has 40 route handlers. Authentication middleware attaches `req.user` after JWT verification. Every handler accesses `(req as any).user` to avoid TypeScript errors. A new developer forgets to protect a route — TypeScript doesn't catch it.

**Question:** Describe the complete solution:
1. How do you type `req.user` correctly without modifying `@types/express`?
2. How do you distinguish protected routes (where `req.user` is `User`) from unprotected routes (where it's `User | undefined`)?
3. Can TypeScript enforce that protected routes are only accessed after authentication middleware? What pattern achieves this?

---

## Scenario 4: Singleton State in a Testable Architecture

**Context:** The DnD project uses:
```typescript
const projectState = ProjectState.getInstance();
```
at module level. A team wants to write unit tests for `ProjectList` that verify listener behavior. They find that:
- Each test pollutes shared state from previous tests
- Running tests in parallel causes race conditions
- They can't inject a mock that simulates 5 existing projects

**Question:** Without changing `ProjectState`'s external API, how would you make the `ProjectList` component testable? Show the interface you'd extract and how a test would use it.

---

## Scenario 5: Refactoring `enum` to `const object` Without Breaking Callers

**Context:** A codebase has:
```typescript
export enum Status { Active = 'ACTIVE', Finished = 'FINISHED' }
```
Used in 30 files. The team wants to migrate to `as const` object to fix tree-shaking. They can't change all 30 files at once.

**Question:** 
1. Write the new type definition using `as const`
2. Write a backward-compatible re-export that makes both work during the migration
3. What TypeScript feature (if any) detects when callers need updating?

---

## Scenario 6: The Type-Safe Update API

**Context:** Your backend has:
```typescript
interface User { id: string; name: string; email: string; role: 'admin' | 'viewer'; createdAt: Date; }
```

The update endpoint should:
- Always require `id` to identify which user to update
- Accept any subset of other fields (but not `createdAt` — that's read-only)
- Never allow updating `id` itself

**Question:** Define the TypeScript type for the update payload. Then write the Express handler that uses it. The type should make `PATCH /users/1 { id: '2', name: 'Bob' }` a compile error (changing `id` in body), and `PATCH /users/1 { createdAt: new Date() }` a compile error.

---

## Scenario 7: React Component Library Type Safety

**Context:** You're building a design system. The `Button` component is used by 10 teams. Two problems:
1. Teams manually copy the `ButtonProps` interface into their code — when you add a prop, they miss it
2. Teams pass invalid `variant` combinations that only fail at runtime

**Question:**
1. How do you prevent teams from needing to copy `ButtonProps`? What TypeScript utility gives them the type automatically?
2. You want to enforce: when `variant="icon"`, the `icon: ReactNode` prop is required; when `variant="text"`, `icon` is forbidden. How would you model this as a union of prop types?

---

## Scenario 8: Zod at API Boundaries

**Context:** A team uses TypeScript everywhere but doesn't validate API responses. A change to the backend API renames `user.email` to `user.emailAddress`. TypeScript doesn't catch it because the frontend has `as User` casts everywhere. Hundreds of `Cannot read property of undefined` errors appear in production.

**Question:** 
1. Identify all the places in a typical frontend app where data enters from external sources (not just API responses)
2. For each, write the Zod schema that would catch the rename at runtime
3. What is the performance cost of Zod validation on every API call? When is the tradeoff acceptable?

---

## Scenario 9: The Generic Form Hook

**Context:** Your React app has 8 different forms. Each form has:
- A set of field names (different per form)
- Validation rules per field
- A submit handler that receives typed form data

Currently, each form is a separate component with duplicated state management and validation logic.

**Question:** Design a generic `useForm<T>` hook where `T` is the form data type:
- `values: T` — current form values
- `errors: Partial<Record<keyof T, string>>` — per-field errors
- `setValue<K extends keyof T>(field: K, value: T[K]): void` — type-safe field update
- `handleSubmit(e: FormEvent): void` — validates and calls the submit callback

Show the hook's TypeScript signature. The key challenge: `setValue` must prevent setting a `number` field to a `string` value at compile time.

---

## Scenario 10: CI Type Safety Pipeline

**Context:** A team ships a TypeScript library. After a refactor, `getUser` returns `User | undefined` instead of `User`. All internal callers are updated. But two external consumers get runtime crashes because their code assumed non-null return. TypeScript didn't catch it in CI.

**Question:**
1. What test in CI would have caught this return type change?
2. Name a tool that specifically tests TypeScript type signatures (not runtime behavior)
3. Write the specific CI check that would fail when `getUser`'s return type changes from `User` to `User | undefined`
