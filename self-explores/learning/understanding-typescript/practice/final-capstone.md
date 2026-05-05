---
date: 2026-05-06
type: capstone-project
mode: practice
course_slug: understanding-typescript
generated_by: typescript-o7e
---

# Final Capstone Project — Understanding TypeScript

**TypeScript Task Manager — end-to-end mastery proof**

**No solution provided.** This is the certification challenge.

---

## 1. Project Goal

Build a full-stack TypeScript task manager with:
- **Backend:** Node.js + Express API with typed routes and Zod validation
- **Frontend:** React component tree with typed props, state, and refs
- **Shared:** Common TypeScript types shared between frontend and backend
- **Patterns:** Generic observer, discriminated unions, decorators, module system

This is not a tutorial — no starter code, no step-by-step guide. Use what you've learned.

---

## 2. Scope

**In scope:**
- Task CRUD (create, read, update status, delete)
- Task status: `'todo' | 'in-progress' | 'done'` (discriminated union, not enum)
- Priority: `1 | 2 | 3` (literal union, not magic numbers)
- Task filtering by status
- Drag-and-drop between status columns (optional stretch)

**Out of scope:**
- Authentication
- Database (use in-memory array)
- Real-time updates (no WebSockets)

---

## 3. Technical Requirements

≥6 TypeScript features must be demonstrably used:

| # | Feature | Where It Must Appear |
|---|---------|---------------------|
| TR1 | Generic class with type parameter | `TaskState<T>` observer or API client |
| TR2 | Discriminated union with exhaustiveness | Task status handler, or API event types |
| TR3 | Mapped type or utility type (`Partial`, custom) | Update payload type |
| TR4 | `import type` | At least 3 imports in the frontend |
| TR5 | Zod schema with `.parse()` | POST /tasks body validation |
| TR6 | React typed props + useRef + useState | Task form component |
| TR7 | At least one decorator OR `as const` enum replacement | Task status or priority definition |
| TR8 | Express typed routes with `@types/express` | All 4 CRUD handlers |

---

## 4. Steps

> Work in this order. Each step should compile before moving to the next.

**Step 1 — Shared types (30 min)**
- Create `shared/types.ts` with Task interface, status union, priority union
- No imports from other app files (pure type definitions)
- Write `UpdateTaskPayload<T>` utility type

**Step 2 — Backend API (60 min)**
- Create `server/app.ts` with 4 Express routes
- Add Zod schemas for POST (create) and PATCH (update) bodies
- In-memory `tasks: Task[]` store
- Test with curl or Postman

**Step 3 — Frontend state (45 min)**
- Create `src/state/task-state.ts` — generic observer pattern
- `TaskState<T>` with `subscribe`, `addTask`, `moveTask`, `deleteTask`
- No DOM imports — pure TypeScript state logic

**Step 4 — React components (60 min)**
- `TaskCard`: receives Task + callbacks, renders task info
- `TaskColumn`: receives status + tasks + callbacks, renders column
- `TaskForm`: controlled form with useRef OR useState, calls `onSubmit(text: string, priority: 1|2|3)`
- `App`: owns state, fetches tasks on mount, renders all columns

**Step 5 — Integration (30 min)**
- Connect React frontend to Express backend via `fetch`
- Validate API responses with Zod before putting into state

---

## 5. Acceptance Criteria

```
Given: POST /tasks with { "title": "Fix bug", "priority": 2 }
When: request is sent
Then: returns 201 with Task object (id, title, priority, status: 'todo', createdAt)

Given: POST /tasks with { "title": "", "priority": 99 }
When: request is sent (Zod validation active)
Then: returns 400 with error details (invalid priority, empty title)

Given: TaskForm submitted with title "Write tests"
When: form's submit handler runs
Then: `onSubmit` receives ("Write tests", priority as number); this.input is accessible (autobind or useRef works correctly)

Given: PATCH /tasks/:id with { "status": "invalid-status" }
When: request is sent
Then: returns 400 (Zod rejects invalid status literal)

Given: TaskState.moveTask called with new status
When: subscribed listener runs
Then: receives updated Task[] with the task in new status

Given: TypeScript compiles the project
When: tsc --noEmit runs
Then: zero type errors
```

---

## 6. Review Checklist

Before submitting (or after completing), verify each item:

**Type Safety**
- [ ] No `any` in source code (search: `grep -r ': any' src/` and `grep -r 'as any' src/`)
- [ ] No `@ts-ignore` or `@ts-expect-error`
- [ ] No non-null assertions `!` except where DOM element is verified to exist at setup
- [ ] `tsc --noEmit` passes

**Architecture**
- [ ] `shared/types.ts` has no imports from `server/` or `src/` (no circular deps)
- [ ] API response is validated before using in frontend state
- [ ] Task status is a literal union type (NOT an enum, NOT a plain string)

**React**
- [ ] All component props have explicit TypeScript interfaces
- [ ] `useRef` has generic type parameter and `null` initial value
- [ ] Event handlers have correct event types (not `Event` for form submit)

**Express**
- [ ] POST and PATCH routes validate body with Zod
- [ ] Error handler middleware is typed with 4 Express error handler parameters
- [ ] `@types/express` is installed as devDependency

---

## 7. Expert Review Criteria

A TypeScript expert would evaluate your submission on these dimensions:

**Pattern Correctness (40%)**
- Is the generic observer correctly implemented? Can it be reused for any type?
- Is the discriminated union exhaustively handled? What happens when a new status is added?
- Is the update payload type correctly derived (required ID, optional rest)?

**Type Safety Rigor (30%)**
- Are there any hidden `any` escapes via function parameters?
- Is API response data validated before being typed?
- Are React event handlers typed correctly (not overused `any`)?

**Production Readiness (20%)**
- Would the shared types work in a monorepo?
- Is the Zod validation at the right boundary (entry points only)?
- Are enum-equivalents using `as const` (not `enum` keyword)?

**Code Clarity (10%)**
- Are interfaces named to communicate intent (not just describe structure)?
- Are generics named with meaningful single letters (T = entity, K = key, E = event)?
- Is `import type` used consistently for type-only imports?
