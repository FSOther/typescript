---
date: 2026-05-06
type: mastery-path
mode: practice
course_slug: understanding-typescript
generated_by: typescript-bgz
---

# Practice Mode Mastery Path — Understanding TypeScript

---

## 1. Final Capability Target

By completing Practice mode, you can:

1. **Write** a complete TypeScript class hierarchy from a written spec — without looking at examples
2. **Debug** TypeScript error messages to find root causes (not just fix the red squiggle)
3. **Design** type-safe APIs: interfaces that make illegal states unrepresentable
4. **Build** a full-stack TypeScript feature (React frontend + Node backend) from scratch
5. **Critique** TypeScript code in code review: identify `any` escapes, missing narrowing, over-constrained generics

**The bar:** After completing P01–P09, you should be able to pass the P08 (DnD full build) challenge without looking at `app.ts`.

---

## 2. Milestone Ladder (5 Levels)

| Level | Name | What It Means | Evidence |
|-------|------|--------------|----------|
| **L1** | Understand | Can explain what TypeScript is doing and why | Can answer Explain It questions in each module verbally |
| **L2** | Replicate | Can implement TypeScript from a written spec without peeking | Completes P01 Build Task from spec only |
| **L3** | Debug | Can identify the root cause of type errors from error messages alone | Completes Fix It sections using only hints |
| **L4** | Design | Can write type-safe interfaces for a new feature from a description | Completes Apply It sections designing novel types |
| **L5** | Apply | Can integrate TypeScript patterns in a real project under time pressure | Completes P08 DnD build + P09 capstone |

### How to use the ladder

Each practice module targets a specific level range:
- P01, P02: L2→L3 (replicate then debug)
- P03, P04, P05: L2→L4 (replicate then design)
- P06, P07: L2→L4 (harder replication)
- P08: L3→L5 (debug the full app, then extend it)
- P09: L3→L5 (novel integration)

**Rule:** Don't move to the next module until you can complete the Build Task without hints. If you need to look at the solution, go back and redo the Explain It section, then retry.

---

## 3. Coverage Matrix

Mapping of core TypeScript concepts to practice modules:

| Concept | Covered In | Level Achieved |
|---------|-----------|----------------|
| Union types | P01 | L3 (debug union narrowing) |
| Type narrowing (`typeof`, `in`) | P01, P05 | L4 (design discriminated unions) |
| `unknown` vs `any` | P01, P05 | L3 |
| `interface` vs `type` | P01, P03 | L3 |
| tsconfig flags | P02 | L2 |
| `strict` mode flags | P02 | L3 |
| Abstract classes | P03 | L2→L3 |
| Access modifiers | P03, P04 | L3 |
| Generic classes | P04 | L2→L3 |
| Generic type constraints | P04, P06 | L3→L4 |
| Observer pattern | P04 | L3 |
| Singleton pattern | P04 | L3 |
| Discriminated unions | P05 | L3→L4 |
| `assertNever` exhaustiveness | P05 | L3 |
| `Extract` / `Exclude` | P05, P06 | L4 |
| Mapped types | P06 | L4 |
| Conditional types | P06 | L4 |
| Utility types (implementation) | P06 | L3→L4 |
| Experimental decorators | P07 | L2→L3 |
| ECMAScript decorators | P07 | L2→L3 |
| All patterns simultaneously | P08 | L5 |
| React: props + state + refs | P09 | L3→L4 |
| Express: routes + types | P09 | L3→L4 |
| Runtime validation (Zod) | P09 | L3 |

**Coverage:** 22 of 22 core TypeScript concepts from Speed mode are exercised at L2+ in at least one practice module. ≥80% are exercised at L3+.

---

## 4. Completion Rules

**Module is complete** when:
- [ ] Build Task completed from spec without looking at course code
- [ ] At least 2 of 4 Break It bugs observed and understood (can explain the error)
- [ ] Fix It completed using hints only (not solutions)
- [ ] All Explain It questions answered in writing
- [ ] Apply It attempted (partial is OK; no completion required)

**Mode is complete** when:
- [ ] All P01–P07 modules complete (by module rules above)
- [ ] P08 Build Task completed without opening `app.ts` at start
- [ ] P09 Part A server compiles and handles all 4 routes
- [ ] P09 Part B React app compiles and renders todos

**Stretch goal (L5 certification):**
- [ ] P08 Apply It extension (delete + count badge) working
- [ ] P09 Apply It (optimistic updates + typed API client) working
- [ ] Can review a PR of TypeScript code and identify 3 type-safety issues

---

## 5. Module-by-Module Checkpoints

### P01: Type System in Action
- Can you write `validate()` without any type errors under `strict: true`?
- Can you explain why `.length` fails on `string | number` without narrowing?

### P02: tsconfig from Scratch
- Can you write a browser tsconfig AND a Node.js tsconfig from memory?
- Can you explain `target` vs `lib` in one sentence each?

### P03: Classes & Interfaces
- Can you rebuild `Component<T, U>` without referring to `app.ts`?
- Can you explain why `abstract class` prevents direct instantiation?

### P04: State Management
- Can you implement `State<T>` with `Listener<T>` and `ProjectState` from scratch?
- Can you explain why `.slice()` is used in `updateListeners`?

### P05: Advanced Types
- Can you write `assertNever` and explain why it catches missing cases at compile time?
- Can you build a discriminated union with `type` discriminant?

### P06: Generics Hands-On
- Can you implement `Partial<T>` as a mapped type without looking at lib.d.ts?
- Can you explain distributive conditional types?

### P07: Decorators in Practice
- Can you implement `@autobind` in both experimental and ECMAScript spec?
- Can you explain why a `get()` accessor is needed in the experimental version?

### P08: DnD Full Build
- Did you attempt the build before opening `app.ts`?
- After comparing to the solution: what did you get right? What did you miss?

### P09: React + Node Capstone
- Can you build a typed CRUD API with Express from scratch?
- Can you build a React component tree with typed props, state, and refs?

---

## 6. When You're Stuck

**Pattern for getting unstuck without cheating:**

1. Read the Hint carefully (Fix It has hints — use them)
2. Write down what TypeScript's exact error message says
3. Search the error message text (not the concept name)
4. Look at the corresponding Speed mode guide for the concept
5. Look at the corresponding Expert mode guide for the deeper reasoning
6. As a last resort: open `/viec hoc` and ask for a conceptual explanation (NOT the answer)

**Never:** Look at the solution code first. The struggle is the learning.
