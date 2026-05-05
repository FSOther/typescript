---
type: practice-challenges
course_slug: understanding-typescript
mode: practice
created: 2026-05-04
---

# Practice Challenges — Understanding TypeScript

Các bài tập thực hành theo từng module. Mỗi challenge tự code từ đầu, không xem đáp án trong course.

---

## M02 — Type System

### Challenge: User/Post Data Model

**File:** `practice/m02-type-system.ts`

**Goal:** Model a small blog app with full TypeScript types.

```typescript
// Requirements:
// 1. UserRole enum: "admin" | "editor" | "viewer"
// 2. User type with: id, name, email, role, createdAt (readonly)
// 3. Post type with: id, title, content, authorId, tags (tuple or array?), status literal
// 4. Comment type with optional parentId (for nested comments)
// 5. Function: getPostsByAuthor(posts: Post[], authorId: User["id"]): Post[]
// 6. Function: summarize(post: Post): string (uses optional chaining + nullish coalescing)
// 7. Record<UserRole, string[]> mapping role -> allowed actions
```

**Acceptance Criteria:**
- [ ] No `any` types anywhere
- [ ] At least 2 literal types, 1 enum, 1 tuple, 1 Record
- [ ] Optional chaining and nullish coalescing in at least 1 function
- [ ] All functions have typed params AND return types

---

## M06 — Classes & Interfaces

### Challenge: Animal Hierarchy

**File:** `practice/m06-classes.ts`

```typescript
// Requirements:
// 1. Abstract class Animal with: name (readonly), abstract speak(): string, move(distance: number): void
// 2. Dog extends Animal: speak returns "Woof!", fetch(item: string): void
// 3. Cat extends Animal: speak returns "Meow!", protected lives: number = 9
// 4. Interface Serializable: toJSON(): string, static fromJSON(json: string): unknown
// 5. Interface Printable: print(): void
// 6. Class PetRecord implements Serializable, Printable
//    - stores list of animals
//    - toJSON(): serializes all animals
//    - print(): logs each animal's name + speak()
```

**Acceptance Criteria:**
- [ ] Abstract method enforced (TypeScript error if not implemented)
- [ ] `implements` 2 interfaces simultaneously
- [ ] `protected` modifier correctly scoped
- [ ] `readonly` on constructor param (shorthand)

---

## M07 — Advanced Types

### Challenge: Redux-Style Reducer

**File:** `practice/m07-adv-types.ts`

```typescript
// Requirements:
// Discriminated union actions:
//   { type: "ADD_TODO"; payload: string }
//   { type: "TOGGLE_TODO"; payload: number }
//   { type: "DELETE_TODO"; payload: number }
//   { type: "CLEAR_ALL" }
//
// State: { todos: Array<{ id: number; text: string; done: boolean }> }
//
// Function todoReducer(state: State, action: Action): State
//   - exhaustive switch (TypeScript should error if case missing)
//
// Type guard: function isToggleAction(a: Action): a is ToggleAction
//
// Overloaded function: dispatch(type: "CLEAR_ALL"): void
//                      dispatch(type: "ADD_TODO", payload: string): void
//                      dispatch(type: "TOGGLE_TODO" | "DELETE_TODO", payload: number): void
```

**Acceptance Criteria:**
- [ ] Exhaustive switch on discriminated union (add `never` check)
- [ ] Type predicate narrows correctly in if-branch
- [ ] Overload signatures compile, implementation signature is broader

---

## M08 — Generics

### Challenge: Generic EventEmitter

**File:** `practice/m08-generics.ts`

```typescript
// Requirements:
// Generic EventEmitter<Events extends Record<string, unknown[]>>
//   - on<K extends keyof Events>(event: K, handler: (...args: Events[K]) => void): void
//   - off<K extends keyof Events>(event: K, handler: (...args: Events[K]) => void): void
//   - emit<K extends keyof Events>(event: K, ...args: Events[K]): void
//
// Usage example:
//   type AppEvents = {
//     "user:login": [userId: string, timestamp: Date]
//     "user:logout": [userId: string]
//     "error": [message: string, code: number]
//   }
//   const emitter = new EventEmitter<AppEvents>()
//   emitter.on("user:login", (id, ts) => console.log(id, ts)) // id: string, ts: Date
//   emitter.emit("error", "oops", 500)
```

**Acceptance Criteria:**
- [ ] `on("user:login", (id, ts) => ...)` — id and ts have correct types without annotation
- [ ] TypeScript error if wrong args passed to emit
- [ ] Works with any Events shape (generic, not hardcoded)

---

## M09 — Generic Project

### Challenge: DoublyLinkedList

**File:** `practice/m09-linked-list.ts`

```typescript
// Implement DoublyLinkedList<T>:
//   - append(value: T): void
//   - prepend(value: T): void
//   - delete(value: T): void
//   - find(value: T): ListNode<T> | null
//   - toArray(): T[]
//   - [Symbol.iterator](): Iterator<T>  (bonus)
```

---

## M10 — Type Utilities

### Challenge: Advanced Custom Utilities

**File:** `practice/m10-utilities.ts`

```typescript
// Implement these 5 utility types from scratch (no copy from lib):

// 1. DeepPartial<T> — recursively makes all properties optional
type DeepPartial<T> = // your impl

// 2. DeepReadonly<T> — recursively makes all properties readonly
type DeepReadonly<T> = // your impl

// 3. PickByValue<T, V> — pick keys where value type matches V
//    PickByValue<{a: string; b: number; c: string}, string> → {a: string; c: string}
type PickByValue<T, V> = // your impl

// 4. RequireAtLeastOne<T, Keys extends keyof T = keyof T>
//    — at least one of the listed keys must be present
type RequireAtLeastOne<T, Keys extends keyof T = keyof T> = // your impl

// 5. FlattenObjectKeys<T> — union of dotted paths for nested objects
//    FlattenObjectKeys<{a: {b: {c: string}}; d: number}> → "a" | "a.b" | "a.b.c" | "d"
type FlattenObjectKeys<T, Prefix extends string = ""> = // your impl
```

**Acceptance Criteria:**
- [ ] Each type passes at least 3 test cases using `type Expect<T extends true> = T` pattern
- [ ] No `any` in implementations

---

## M13 — DnD Project

### Challenge: Build Without Looking

**Goal:** Complete the full drag-and-drop project using ONLY:
- The transcript for requirements/UI description
- The `resources/163-prj-00-initial-starting-setup/` starter files
- TypeScript docs for syntax questions

**Do NOT open `resources/179-prj-16-finished/` until your version is complete.**

**Phases:**
1. [ ] Project input form renders + captures input
2. [ ] Validation works (no empty fields, positive numbers)
3. [ ] Projects appear in "active" list after submit
4. [ ] Drag from active → finished works
5. [ ] Visual drag feedback (border highlight on drop targets)

---

## M17 — React + TypeScript

### Challenge: Typed useFetch Hook

**File:** `practice/m17-react/hooks/useFetch.ts`

```typescript
// Implement a generic useFetch hook:
//   function useFetch<T>(url: string, options?: RequestInit): {
//     data: T | null;
//     loading: boolean;
//     error: string | null;
//     refetch: () => void;
//   }
//
// Requirements:
// - T inferred from usage site, not hardcoded
// - Handles abort on unmount (cleanup)
// - refetch triggers a new request
// - Error is string message (not Error object)
```

---

## M18 — Node + Express

### Challenge: Typed REST Router

**File:** `practice/m18-node/src/users.router.ts`

```typescript
// Requirements:
// POST /users — body: { name: string; email: string; role: "admin" | "user" }
//   Returns: { id: string; name: string; email: string; role: string; createdAt: string }
//
// GET /users/:id
//   Returns: same as above, or 404 JSON { error: "Not found" }
//
// PUT /users/:id — body: Partial<{ name: string; role: string }>
//   Returns: updated user
//
// All request bodies validated (Zod or manual type guard)
// No req.body as any
```

---

## Cross-Module Capstone

### Challenge: TypeScript CLI Task Manager

**After completing M02, M06, M07, M08, M10.**

Build a CLI task manager with:
- `Task` type with subtasks, tags, priority, deadline
- Generic `Repository<T>` class with CRUD
- `EventEmitter<Events>` for task state changes
- Discriminated union for task actions
- `satisfies` for config objects
- Mapped type for form schemas from data types
- Export as proper ES module with `.d.ts`

No framework, no build tools — just `ts-node` + Node.js standard library.
