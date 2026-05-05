---
date: 2026-05-06
type: deliberate-practice-drills
mode: expert
course_slug: understanding-typescript
generated_by: typescript-n2s
---

# Expert Deliberate Practice Drills — Understanding TypeScript

**12 drills. Each has a constraint. No solutions provided.**

Format: **Constraint** (what you can't use) + **Prompt** (what you must build) + **Verify** (what makes it correct).

---

## Drill 1: Rewrite `any[]` with Generics

**Constraint:** No `any`, no `as`, no `unknown`.

```typescript
// Start: fix this
function processItems(items: any[], fn: (item: any) => any): any[] {
  return items.map(fn);
}
```

**Goal:** Make this function fully type-safe. The return type should be inferred from `fn`'s return type. Calling `processItems([1, 2, 3], x => x.toString())` should return `string[]`.

**Verify:** `const result = processItems([1, 2], x => x + 1)` — hover over `result`. It should be `number[]`, not `any[]`.

---

## Drill 2: Strict Migration (no `!` or `as any`)

**Constraint:** No `!` (non-null assertion) and no `as any`. Only `strictNullChecks: true`.

```typescript
function getUserName(id: string): string {
  const users = [{ id: '1', name: 'Alice' }];
  const user = users.find(u => u.id === id);
  return user.name.toUpperCase(); // user could be undefined
}
```

**Goal:** Fix to return `string | undefined` or throw a descriptive error. Your choice — but no assertions.

**Verify:** Callers must handle `undefined` case (TypeScript enforces this).

---

## Drill 3: Replace `if/else` with Discriminated Union + `assertNever`

**Constraint:** No `if/else` in the handler. Use `switch` with `assertNever` default.

```typescript
type Event =
  | { type: 'click'; target: HTMLElement }
  | { type: 'keydown'; key: string }
  | { type: 'resize'; width: number; height: number };

function handleEvent(event: Event): string {
  if (event.type === 'click') return `Clicked ${event.target.tagName}`;
  if (event.type === 'keydown') return `Key: ${event.key}`;
  return 'Unknown';  // BUG: resize not handled, no compile error
}
```

**Goal:** Rewrite so that adding a new `Event` type produces a compile error in `handleEvent`.

**Verify:** Add `| { type: 'scroll'; delta: number }` to `Event`. Confirm compile error location.

---

## Drill 4: Minimum Generic Constraint

**Constraint:** No `extends ProjectItem`, no `extends BaseModel`. Only structural constraints.

```typescript
class ProjectItem {
  id: string;
  title: string;
  status: 'Active' | 'Finished';
  constructor(id: string, title: string, status: 'Active' | 'Finished') {
    this.id = id; this.title = title; this.status = status;
  }
}

function formatItem<T extends ProjectItem>(item: T): string {
  return `[${item.id}] ${item.title}`;
}
```

**Goal:** Change the constraint so that `formatItem` works with any object that has `id: string` and `title: string` — not just `ProjectItem` instances.

**Verify:** `formatItem({ id: '1', title: 'Foo', extra: 'bar' })` should compile.

---

## Drill 5: Write `DeepReadonly<T>` from Scratch

**Constraint:** No `as const`. Only mapped types and conditional types.

**Goal:**
```typescript
type DeepReadonly<T> = ???;  // you write this

type Config = {
  server: { host: string; port: number; };
  features: string[];
};

type FrozenConfig = DeepReadonly<Config>;
// Expected: { readonly server: { readonly host: string; readonly port: number; }; readonly features: readonly string[]; }
```

**Verify:** `const cfg: FrozenConfig = { server: { host: 'localhost', port: 3000 }, features: [] }; cfg.server.host = 'x';` — should produce compile error.

---

## Drill 6: Convert Experimental to ECMAScript Decorator

**Constraint:** No `experimentalDecorators: true` in tsconfig.

```typescript
// Experimental (needs experimentalDecorators: true)
function log(target: any, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function(...args: any[]) {
    console.log(`Calling ${key} with`, args);
    return original.apply(this, args);
  };
  return descriptor;
}
```

**Goal:** Rewrite as an ECMAScript spec decorator using `ClassMethodDecoratorContext`.

**Verify:** Applying `@log` to a class method compiles without `experimentalDecorators: true`. The logged output includes the method name and arguments.

---

## Drill 7: Module Augmentation for `req.user`

**Constraint:** No modification of `node_modules`. No `as any` at call sites.

**Goal:** Write module augmentation so that `req.user` is typed as `AuthenticatedUser` in all Express routes:

```typescript
interface AuthenticatedUser { id: string; email: string; role: 'admin' | 'viewer'; }

// After augmentation:
app.get('/profile', (req, res) => {
  const user = req.user; // should be AuthenticatedUser | undefined, not any
  if (user) { res.json({ email: user.email }); } // .email should be typed
});
```

**Verify:** Accessing `req.user.nonExistentField` should produce a TypeScript error.

---

## Drill 8: Type-safe `pipe` with 3 Overloads

**Constraint:** No `any` in the overload signatures.

**Goal:** Write a `pipe` function that composes 1, 2, or 3 functions:
```typescript
pipe(double)(4)                     // number
pipe(double, toString)(4)           // string
pipe(double, toString, addBang)(4)  // string
```

The intermediate types must flow correctly. `pipe(double, parseFloat)` — `double: (n: number) => number`, `parseFloat: (s: string) => number` — should produce a TypeScript error (number is not string).

**Verify:** Hover over result of each `pipe` call — the return type should be inferred correctly.

---

## Drill 9: `UpdatePayload<T>` Generic Utility

**Constraint:** No manual field listing. Must work for any type `T` with any `IdKey`.

**Goal:**
```typescript
type UpdatePayload<T, IdKey extends keyof T = 'id'> = ???;

interface User { id: string; name: string; email: string; }
type UpdateUser = UpdatePayload<User>; // { id: string } & { name?: string; email?: string; }

interface Order { orderId: number; item: string; quantity: number; }
type UpdateOrder = UpdatePayload<Order, 'orderId'>; // { orderId: number } & { item?: string; quantity?: number; }
```

**Verify:** `const update: UpdateUser = { name: 'Bob' }` should fail (missing `id`). `const update: UpdateUser = { id: '1' }` should succeed.

---

## Drill 10: React `ComponentProps` Extension

**Constraint:** No manual listing of HTML button props (no `disabled`, `onClick`, `type` in your interface).

**Goal:** Build a `PrimaryButton` component that extends all native `<button>` props plus a `variant: 'primary' | 'danger'` prop:

```typescript
// Should work:
<PrimaryButton variant="primary" onClick={handleClick} disabled={loading}>
  Submit
</PrimaryButton>
```

**Verify:** Passing `variant="unknown"` should produce a TypeScript error. Passing `href="/"` (anchor prop, not button prop) should produce a TypeScript error.

---

## Drill 11: Express Route with Zod Validation

**Constraint:** No `req.body as SomeType`. Use Zod to parse the body.

**Goal:** Write an Express POST route for creating a task. The body must have `title: string` (min 1 char), `priority: 1 | 2 | 3`, and optional `dueDate: string` (ISO date format). Return `400` with ZodError details on invalid input.

**Verify:** POST with `{ title: '', priority: 5 }` should return `400`. POST with `{ title: 'Fix bug', priority: 2 }` should return `201`.

---

## Drill 12: CI Type Check GitHub Actions Step

**Constraint:** Must be a separate step from the build step. Must fail on type errors even if the bundler build passes.

**Goal:** Write a GitHub Actions workflow (`ci.yml`) with:
1. A `typecheck` step that runs `tsc --noEmit`
2. A `lint` step that fails if any `@ts-ignore` is found (grep-based)
3. A `test` step that runs `npm test`

All three should run independently so you can see which one failed.

**Verify:** A PR with a `// @ts-ignore` comment should fail the `lint` step. A PR with a type error should fail `typecheck` but potentially pass `build`.
