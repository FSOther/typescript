---
date: 2026-05-05
type: task-worklog
task: typescript-479
title: "Speed Mode — M10 Type Utilities L119-132"
status: open
detail_score: ready-for-dev
tags: [typescript, learning, type-utilities, mapped-types, speed-mode]
---

# Speed Mode — M10 Type Utilities L119-132

## Objective

Hoàn thành M10 theo **speed mode**: nắm TypeScript built-in utility types + cơ chế tạo ra chúng (mapped types, conditional types, template literals). Đây là module cung cấp "vocabulary" để đọc production TypeScript code — `Partial<T>`, `Required<T>`, `Pick<T, K>`, `Omit<T, K>`, etc.

## Scope

**In scope:**
- L118: SKIP (intro)
- L119: `typeof` in type position, `ReturnType<T>`, `Parameters<T>`
- L120: `Readonly<T>`, `Partial<T>`, `Required<T>`
- L121: `Record<K, V>` — deeper dive
- L122: `Pick<T, K>`, `Omit<T, K>`
- L123: `Exclude<T, U>`, `Extract<T, U>`
- L124: `NonNullable<T>`, `Awaited<T>`
- L125: Mapped types (`{ [K in keyof T]: ... }`)
- L126: Mapped type modifiers (`+?`, `-?`, `+readonly`, `-readonly`)
- L127: Template literal types (`type EventName = \`on${string}\``)
- L128: String manipulation utilities (`Uppercase<S>`, `Capitalize<S>`, etc.)
- L129: Conditional types — distributive behavior
- L130: `infer` advanced usage
- L131: `NoInfer<T>` (TS 5.4)
- L132: Built-in utilities overview + which to use when

**Out of scope:**
- Implementing all utilities from scratch (practice/expert mode)
- Recursive mapped types
- Variance annotations (expert mode)

## Input / Output

**Input:** Transcripts `lectures/119.*.txt` → `lectures/132.*.txt` (skip L118)

**Output:**
- `speed/study-progress.md` — L119-L132 done
- Mental model: "Utility types = generic transformers that map one shape to another"
- Can recognize and use: `Partial`, `Required`, `Readonly`, `Pick`, `Omit`, `Exclude`, `Extract`, `NonNullable`, `ReturnType`, `Parameters`

## Dependencies

- **Requires:** `typescript-13s` (M07+M08) done
  - Mapped types và conditional types build directly on M07+M08 foundations
  - Generic constraints (`K extends keyof T`) critical for understanding `Pick<T, K>`

## Flow

### Step 1: L119 — typeof & ReturnType (~15 phút)

```bash
cat lectures/119.*.txt
```

**Đọc và trả lời:**
- `typeof` in type position vs value position — difference?
- `ReturnType<typeof someFunction>` — why useful in practice?
- `Parameters<T>` — returns what type?

**Mental model:** `typeof` bridges values and types. `ReturnType<typeof fn>` = "give me the type of whatever fn returns."

**Verify:** Biết `typeof` trong `type X = typeof myVar` ≠ `typeof myVar === 'string'`

---

### Step 2: L120-L122 — Partial, Required, Readonly, Record (~20 phút)

```bash
cat lectures/120.*.txt lectures/121.*.txt lectures/122.*.txt
```

**Đọc:**
- `Partial<User>` = all fields optional
- `Required<Config>` = all fields required (opposite of Partial)
- `Readonly<T>` = all fields readonly
- `Record<string, User[]>` = object with string keys, User[] values

**Real-world use:** `Partial<T>` appears in every React update handler and Redux reducer.

**Verify:** Know all 4 transformations and when to reach for them

---

### Step 3: L122 cont + L123-L124 — Pick, Omit, Exclude, Extract, NonNullable (~25 phút)

```bash
cat lectures/123.*.txt lectures/124.*.txt
```

**Đọc và trả lời:**
- `Pick<User, "name" | "email">` — which fields remain?
- `Omit<User, "password">` — which fields remain?
- `Exclude<"a" | "b" | "c", "b">` — result?
- `Extract<string | number | boolean, string | number>` — result?
- `NonNullable<string | null | undefined>` — result?

**Key mental model:**
- `Pick`/`Omit` work on **object types** (select/remove keys)
- `Exclude`/`Extract` work on **union types** (filter union members)

**Verify:** Can tell apart Pick vs Exclude without looking them up

---

### Step 4: L125-L126 — Mapped types & modifiers (~25 phút)

```bash
cat lectures/125.*.txt lectures/126.*.txt
```

**Đọc (IMPORTANT):**
- Mapped type syntax: `{ [K in keyof T]: SomeTransform<T[K]> }`
- `Partial<T>` is implemented as: `{ [K in keyof T]?: T[K] }`
- `-?` removes optional, `+?` adds optional (default)
- `-readonly` removes readonly modifier

**Key insight:** All utility types above are implemented via mapped types. Understanding mapped types = understanding HOW they work.

**Verify:** Can write `Partial<T>` from scratch: `type Partial<T> = { [K in keyof T]?: T[K] }`

---

### Step 5: L127-L128 — Template literal types (~20 phút)

```bash
cat lectures/127.*.txt lectures/128.*.txt
```

**Đọc:**
- Template literal type: `` type EventName = `on${Capitalize<string>}` ``
- String manipulation types: `Uppercase<S>`, `Lowercase<S>`, `Capitalize<S>`, `Uncapitalize<S>`
- Distribution over unions: `` type Keys = `on${"Click" | "Hover"}` `` = `"onClick" | "onHover"`

**Real-world:** CSS-in-JS, typed event names, API endpoint generators all use this.

**Verify:** Know that template literal types distribute over unions

---

### Step 6: L129-L131 — Distributive conditionals & infer & NoInfer (~25 phút)

```bash
cat lectures/129.*.txt lectures/130.*.txt lectures/131.*.txt
```

**Đọc:**
- Distributive: `type Nulls<T> = T extends null | undefined ? true : false` distributes over union `T`
- Non-distributive: `[T] extends [null | undefined]` — bracket prevents distribution
- `NoInfer<T>` (TS 5.4): prevents inference from a parameter position

**Note:** Distributive conditionals are advanced. Speed mode: understand the concept, don't master.

**Verify:** Know difference between `T extends U` (distributes over union T) vs `[T] extends [U]` (doesn't)

---

### Step 7: L132 — Review built-in utilities (~10 phút)

```bash
cat lectures/132.*.txt
```

**Read the overview** — mentally verify you recognize all utility types.

Quick reference cheatsheet (already in `speed/high-speed-cheatsheet.md`).

---

### Step 8: Update progress (~5 phút)

Update `speed/study-progress.md`:
- L118 = `skipped`
- L119-L132 = `done`

**Verify:** 14 done entries in L118-L132 range

---

## Edge Cases

| Tình huống | Xử lý |
|---|---|
| Pick vs Omit confusion | Mnemonic: "Pick = choose which to KEEP. Omit = choose which to REMOVE." |
| Mapped type syntax intimidating | Focus on `Partial<T>` implementation — it's the simplest, understand that first |
| Distributive conditional types confusing | OK for speed mode — just know the concept, not the mechanism |
| NoInfer<T> not in older TS | It's TS 5.4 — note the version requirement |

## Acceptance Criteria

**AC1 — Happy path: Utility types vocabulary**
- **Given** code `type UpdateUser = Partial<Pick<User, "name" | "email">>`
- **When** asked what it means
- **Then** explain: "Object with optional `name` and `email` fields (both from User type, both optional)"

**AC2 — Happy path: Mapped type from scratch**
- **Given** asked to implement `Readonly<T>` using mapped types
- **When** write the implementation
- **Then** output: `type Readonly<T> = { readonly [K in keyof T]: T[K] }`

**AC3 — Negative: Pick vs Exclude distinction**
- **Given** `Pick<User, "name">` vs `Exclude<"name" | "email", "email">`
- **When** asked what each produces
- **Then** correctly identify: `Pick` produces `{name: string}` (object), `Exclude` produces `"name"` (string literal)

## Technical Notes

- M10 is the "vocabulary module" — you'll reference these types daily in TS code
- `Partial`, `Required`, `Readonly`, `Pick`, `Omit` = must know cold
- `ReturnType`, `Parameters`, `Awaited` = know when to reach for them
- Mapped types + conditional types = the MECHANISM behind all utilities
- Estimated time: ~2.5h (14 lectures × ~10 min each — slightly harder)

## Risks

| Risk | Probability | Mitigation |
|---|---|---|
| Pick/Exclude/Omit/Extract confusion | High | Step 3 has explicit compare-and-contrast questions |
| Mapped types feel abstract | Medium | Step 4 connects mapped types to familiar `Partial<T>` |
| Trying to master conditional types deeply | Medium | Speed mode: concept only. Expert mode handles deep dive |

## Worklog

### [2026-05-05] Hoàn thành — 14 lectures M10 trong 1 session

**Kết quả:** 14 lectures hoàn thành (L119-L132).

**Session log:** [2026-05-05-4.md](../../learning/understanding-typescript/speed/sessions/2026-05-05-4.md)

**Output mong đợi:**
- [x] L119-L132 tất cả status=done trong study-progress.md
- [x] Active recall 3 câu/lecture — đã trả lời trong session log
- [x] `typeof`/`keyof` mental model: derive types from values/keys
- [x] Mapped type pattern: `{ [K in keyof T]: NewType }`
- [x] `infer` mental model: extract sub-type from conditional check
- [x] Built-in utilities cheat sheet

**Key findings:**
- `typeof` most useful for: deriving types from complex objects + function types without re-typing
- `keyof T` + `K extends keyof T` = the canonical "safe key access" pattern (getProp function)
- Mapped types modifiers: `?` (optional), `-?` (required), `readonly`, `-readonly` — minus prefix removes flags
- `infer R` = "capture this sub-type as R" — only usable inside conditional type, only on true branch
- Built-in utilities cover 90% of use cases — check docs before building custom ones
- These features primarily matter for library/framework code, not typical app code

**Progress:** 45% (81 lectures done — skips counted)

**AC checks:**
- ✅ AC1: L119-L132 → done in study-progress.md
- ✅ AC2: Có thể viết `Readonly<T>` từ đầu: `type Readonly<T> = { readonly [K in keyof T]: T[K] }`
- ✅ AC3: Có thể viết `Partial<T>` từ đầu: `type Partial<T> = { [K in keyof T]?: T[K] }`
- ✅ AC4: Hiểu `infer` mental model — nắm ví dụ `ReturnType<T>` từ L131
