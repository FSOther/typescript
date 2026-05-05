# P05 — Advanced Types & Narrowing

**Practice module** | Mastery target: L2→L4 | ~85 min

---

## 1. Build Task

> **No solution provided.** Build from spec only.

**Objective:** Implement a typed event handler system using discriminated unions with exhaustiveness checking.

**Spec:**

Create `events.ts`:

1. Define a `DomainEvent` discriminated union with these members:
   - `ProjectCreated`: `{ type: 'ProjectCreated'; projectId: string; title: string; timestamp: Date }`
   - `ProjectMoved`: `{ type: 'ProjectMoved'; projectId: string; fromStatus: 'Active' | 'Finished'; toStatus: 'Active' | 'Finished' }`
   - `ProjectDeleted`: `{ type: 'ProjectDeleted'; projectId: string; reason?: string }`
   - `UserAction`: `{ type: 'UserAction'; userId: string; action: string }`

2. `function handleEvent(event: DomainEvent): string` — returns a human-readable description:
   - Uses a `switch` on `event.type`
   - Each case returns a string
   - Default case uses `assertNever(event)` for exhaustiveness

3. `function assertNever(x: never): never` — throws an error with the unhandled value

4. `type EventOf<T extends DomainEvent['type']> = Extract<DomainEvent, { type: T }>` — utility type that extracts one event type from the union

5. `function createHandler<T extends DomainEvent['type']>(type: T, fn: (event: EventOf<T>) => void): (event: DomainEvent) => void` — creates a type-safe handler that only handles one event type

**Constraint:** No `any`. `assertNever` must be called in the default branch. TypeScript must produce a compile error if a new event type is added to `DomainEvent` but not handled in `handleEvent`.

---

## 2. Break It

1. Add `'pending': { type: 'pending'; data: string }` to `DomainEvent` without adding a case in `handleEvent`. What TypeScript error do you get? Where?

2. Remove `assertNever` from the default branch. Re-add the new type. Do you still get an error?

3. Change `switch (event.type)` to `switch (event)` — what error? Why can't TypeScript narrow on the whole object?

4. In the `ProjectMoved` case, access `event.title` — what error? Why does `event` not have `title` in the `ProjectMoved` branch?

---

## 3. Fix It

> **Hints only — no solution shown.**

```typescript
type Status = 'active' | 'inactive' | 'pending';

function describeStatus(status: Status): string {
  if (status === 'active') return 'User is active';
  if (status === 'inactive') return 'User is inactive';
  // Missing: 'pending' case + exhaustiveness
  return ''; // No error — but this should be unreachable
}
```

**Hint 1:** The empty string return silently handles the `'pending'` case. You need a way to make TypeScript error when a case is missed.

**Hint 2:** What type is `status` after the `if (status === 'active')` and `if (status === 'inactive')` branches? Hover over it in your editor or reason about it.

**Hint 3:** What function parameter type would make the default case a compile error if any branch is missing?

---

## 4. Explain It

1. What is a "discriminated union"? What makes the `type` field the "discriminant"?

2. Why does `assertNever(x: never)` cause a compile error when called with `x` that's not `never`? What is TypeScript's `never` type?

3. `Extract<DomainEvent, { type: 'ProjectCreated' }>` — explain what `Extract` does. How would you implement it as a conditional type from scratch?

---

## 5. Apply It

Add a new event type `CommentAdded: { type: 'CommentAdded'; projectId: string; comment: string; author: string }` to `DomainEvent`.

Verify that:
1. TypeScript immediately errors in `handleEvent` (without running the code)
2. `EventOf<'CommentAdded'>` resolves to the correct type
3. `createHandler('CommentAdded', event => event.comment)` — `event.comment` is typed as `string`

---

## 6. Acceptance Criteria

```
Given: DomainEvent union with 4 types
When: handleEvent has switch with all 4 cases + assertNever default
Then: compiles clean

Given: a 5th type added to DomainEvent
When: handleEvent switch is NOT updated
Then: compile error on the assertNever line (or default branch)

Given: inside case 'ProjectCreated' branch
When: accessing event.fromStatus
Then: TypeScript error (fromStatus only exists on ProjectMoved)
```

---

## 7. Resources

- Speed guide: [`M06-advanced-types.md`](../../../speed/module-guides/M06-advanced-types.md)
- Expert guide: [`E04-advanced-types-narrowing.md`](../../../expert/module-guides/E04-advanced-types-narrowing.md)
- Reference lectures: L88–L99
