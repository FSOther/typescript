# E12 — Expert Gaps: Testing & CI

**Expert module** | Covers: L258+ (gaps not in course) | ~60 min

---

## 1. Core Mental Model

> "TypeScript's type system only runs at compile time. `tsc --noEmit` in CI is not optional — it's the guarantee that your types are correct in the codebase, not just in your local editor. Without it, `as any` scattered through the codebase compiles silently in production builds."

Expert insight: "it compiles" and "it's type-safe" are different claims. A codebase full of `as any` compiles. A codebase with Zod schemas at boundaries but `any` everywhere inside them compiles. The type system is only as good as the discipline that maintains it.

---

## 2. Beginner Misunderstanding

> "TypeScript catches my bugs. I don't need tests for type-related things."

What TypeScript doesn't test: (1) runtime values that bypass types via `as` or `any`, (2) return types of generic functions changing silently in a refactor, (3) whether your public API's type signature is what consumers expect. Type tests (`expect-type`, `tsd`) and integration tests with real data complement TypeScript's compile-time checks.

---

## 3. Professional Concern

> "We have a utility library used by 5 teams. When we refactor internals, sometimes public type signatures change in subtle ways that don't break compilation but break callers' type inference."

The solution: snapshot tests for public types using `expect-type` or `tsd`. These run as part of CI and fail when return types change in unexpected ways.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: `tsc --noEmit` passes but bundle has errors**
```typescript
// Your tsconfig says "strict": true
// But your bundler's tsconfig says "strict": false
// npm run build succeeds with type errors in the codebase
// What's the single source of truth for tsconfig?
```

**Trap 2: Type assertions bypass tests**
```typescript
function parseUser(data: unknown): User {
  return data as User; // always "works" — even if data is null
}
// Your test: expect(parseUser(validData)).toEqual(user) — passes
// Production: parseUser({ typo_field: 'x' }) — passes TypeScript, fails at runtime
// What's the correct implementation?
```

**Trap 3: `@ts-ignore` accumulation**
```typescript
// 50 ts-ignore comments in a codebase
// Someone adds a new one. No way to know if old ones are still needed.
// What's the better alternative to @ts-ignore?
// What command tells you which ts-ignores are no longer needed?
```

---

## 5. Expert Shortcut

**CI pipeline with type checking:**
```yaml
# .github/workflows/ci.yml
- name: Type check
  run: npx tsc --noEmit

- name: Test
  run: npm test

- name: Build
  run: npm run build
# Type check is separate from build — Vite build doesn't type-check
```

**`@ts-expect-error` vs `@ts-ignore`:**
```typescript
// @ts-ignore: silently no-ops if the error goes away
// @ts-expect-error: fails compilation if the line NO LONGER has an error
// Always prefer @ts-expect-error — it self-documents and self-cleans
// @ts-expect-error Property doesn't exist on this version
legacyApi.oldMethod();
```

**Type-level test with `expect-type`:**
```bash
npm install --save-dev expect-type
```
```typescript
import { expectTypeOf } from 'expect-type';

// In a test file — fails if return type changes
expectTypeOf(parseUser).returns.toMatchTypeOf<User>();
expectTypeOf(getUserById).parameter(0).toBeString();
```

**Module augmentation for test environment:**
```typescript
// test-setup.ts
declare global {
  namespace jest {
    interface Matchers<R> {
      toBeValidUser(): R;
    }
  }
}
```

---

## 6. Real-world Scenario

Your TypeScript library exports:
```typescript
export function formatDate(date: Date, locale: string): string;
export function parseAmount(raw: string): { value: number; currency: string };
```

A future refactor changes `parseAmount` to return `{ amount: number; currency: string }` (renamed `value` to `amount`). This compiles. But 3 consuming apps break at runtime because they destructure `value`.

Design a CI check that would have caught this before merge. Specifically: what file do you create, what tool do you use, and what does the check look like?

---

## 7. Deliberate Drill

**Constraint:** Write a GitHub Actions workflow step that:
1. Runs `tsc --noEmit` for type checking (separate from build)
2. Fails if there are any `@ts-ignore` comments (grep-based)
3. Runs tests

All three steps should run independently so you can see which one failed.

```yaml
jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      # ... write the steps
```

---

## 8. Expert AC

Before moving on, verify:
- [ ] Know why `tsc --noEmit` in CI is separate from the build step
- [ ] Can explain `@ts-expect-error` vs `@ts-ignore` and why to prefer the former
- [ ] Know what `expect-type` or `tsd` provides that unit tests don't
- [ ] Have a mental model of where TypeScript's type safety ends and runtime validation begins

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. A codebase uses `as unknown as TargetType` instead of `as TargetType`. Why might someone use this double-cast? What does it reveal about the underlying design?

2. You're reviewing a PR. You see `const result = fetchData() as ApiResponse`. What questions do you ask the PR author? What would an acceptable fix look like?

3. `strictNullChecks` catches null/undefined errors at compile time. But what class of null/undefined errors does it NOT catch? Give a concrete example.
