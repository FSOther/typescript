---
date: 2026-05-05
type: task-worklog
task: typescript-13s
title: "Speed Mode — M07+M08 Advanced Types & Generics L89-109"
status: closed
detail_score: ready-for-dev
tags: [typescript, learning, advanced-types, generics, speed-mode]
---

# Speed Mode — M07+M08 Advanced Types & Generics L89-109

## Objective

Hoàn thành M07 (Advanced Types) và M08 (Generics) theo **speed mode**. Đây là 2 module phức tạp nhất của course — trọng tâm: hiểu mental model, không cần master mọi syntax ngay. Generics đặc biệt quan trọng vì xuất hiện trong mọi TypeScript codebase thực tế.

## Scope

**In scope (M07 — L89-L99):**
- L88: SKIP (intro)
- L89: Intersection types (`A & B`)
- L90: Type guards (`typeof`, `instanceof`, `in`) — MASTER
- L91: Discriminated unions (tagged unions) — MASTER
- L92-L93: `instanceof` & type predicates (`x is Type`)
- L94-L95: Function overloads
- L96: Index signatures (`[key: string]: number`)
- L97: `as const` — literal inference
- L98-L99: `satisfies` operator

**In scope (M08 — L100-L109):**
- L100: SKIP (intro)
- L101: Generic functions (`function identity<T>(x: T): T`)
- L102: Generic constraints (`<T extends object>`)
- L103: Generic types & interfaces
- L104-L105: Generic classes
- L106: Default type parameters
- L107: `keyof` with generics (`<T, K extends keyof T>`)
- L108: Conditional types (`T extends U ? X : Y`)
- L109: Infer keyword

**Out of scope:**
- Variadic tuple types (TS 4.0+) — expert mode
- Template literal types — M10
- Mapped types — M10

## Input / Output

**Input:** Transcripts `lectures/89.*.txt` → `lectures/109.*.txt` (skip L88, L100)

**Output:**
- `speed/study-progress.md` — L89-L109 tất cả done
- Can write: generic function, constrained generic, discriminated union
- Mental model: "Generics = type variables. Discriminated unions = exhaustive switch on `kind` field."

## Dependencies

- **Requires:** `typescript-wnw` (M06) done
  - Abstract classes, interfaces cần hiểu để understand generic constraints

## Flow

### Session 1: M07 Advanced Types (~2h)

#### Step 1: L89 — Intersection types (~10 phút)

```bash
cat lectures/89.*.txt
```

**Đọc và trả lời:**
- `A & B` vs `A | B` — semantic difference?
- Intersection của `{x: number}` & `{y: string}` = gì?

**Verify:** Biết `string & number` = `never` (không value nào satisfy cả 2)

---

#### Step 2: L90-L91 — Type guards & Discriminated unions (~30 phút)

```bash
cat lectures/90.*.txt lectures/91.*.txt
```

**MASTER level — đọc kỹ:**
- 3 loại type guard: `typeof`, `in`, `instanceof`
- Discriminated union: field chung (`kind: "circle" | "square"`) + exhaustive switch

**Mental model cho discriminated unions:**
```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; side: number };

function area(s: Shape): number {
  switch (s.kind) {
    case "circle": return Math.PI * s.radius ** 2;
    case "square": return s.side ** 2;
    // TS knows: if you add a new variant and miss it, `default: s` → never error
  }
}
```

**Verify:** Có thể viết basic discriminated union + switch từ memory

---

#### Step 3: L92-L95 — instanceof, Predicates, Overloads (~25 phút)

```bash
cat lectures/92.*.txt lectures/93.*.txt lectures/94.*.txt lectures/95.*.txt
```

**Đọc và trả lời:**
- Type predicate: `function isString(x: unknown): x is string` — TS làm gì với return type này?
- Function overloads — bao nhiêu signatures tối thiểu?
- Overload implementation signature có visible ở call site không?

**Verify:** Biết predicate syntax `x is Type` và khi nào dùng vs `typeof`

---

#### Step 4: L96-L99 — Index signatures, as const, satisfies (~20 phút)

```bash
cat lectures/96.*.txt lectures/97.*.txt lectures/98.*.txt lectures/99.*.txt
```

**Đọc:**
- Index signature `[key: string]: number` — limitation là gì?
- `as const` — widening prevention, `readonly` deep
- `satisfies` (TS 4.9) — giữ narrow type nhưng validate shape

**Verify:** Biết `const palette = { red: [255, 0, 0] } as const` → `palette.red` = `readonly [255, 0, 0]`

---

### Session 2: M08 Generics (~2h)

#### Step 5: L101-L103 — Generic functions & types (~30 phút)

```bash
cat lectures/101.*.txt lectures/102.*.txt lectures/103.*.txt
```

**Đọc và trả lời:**
- Generic function vs overload — khi nào dùng generic?
- `<T extends object>` — ý nghĩa constraint?
- Generic interface: `interface Repository<T> { findById(id: string): T }`

**Mental model:** "T is a placeholder that gets filled in at call site. Constraint = what T must be."

**Verify:** Có thể viết `function first<T>(arr: T[]): T | undefined`

---

#### Step 6: L104-L106 — Generic classes & defaults (~20 phút)

```bash
cat lectures/104.*.txt lectures/105.*.txt lectures/106.*.txt
```

**Đọc:**
- Generic class: `class Stack<T> { private items: T[] = [] }`
- Default type: `<T = string>` — khi dùng khi không cung cấp type arg
- Generic class instantiation: `new Stack<number>()` vs `new Stack()`

**Verify:** Biết default type param và khi nào TS infer vs khi nào cần cung cấp

---

#### Step 7: L107 — keyof + generics (~20 phút)

```bash
cat lectures/107.*.txt
```

**Đọc và trả lời (IMPORTANT):**
- `<T, K extends keyof T>` pattern — tại sao quan trọng?
- `function getProperty<T, K extends keyof T>(obj: T, key: K): T[K]` — tại sao return type `T[K]`?

**This pattern appears everywhere in real TS code (React props, Redux selectors, etc.)**

**Verify:** Có thể explain tại sao `K extends keyof T` làm cho `obj[key]` type-safe

---

#### Step 8: L108-L109 — Conditional types & infer (~25 phút)

```bash
cat lectures/108.*.txt lectures/109.*.txt
```

**Đọc:**
- `T extends U ? X : Y` — distributed vs non-distributed
- `infer R` trong conditional type: `type ReturnType<T> = T extends (...args: any) => infer R ? R : never`
- Mental model: "infer = declare a type variable to be inferred from the match"

**Note:** Don't try to master immediately — these need practice. Just understand the pattern.

**Verify:** Biết `infer` chỉ xuất hiện trong `extends` clause của conditional type

---

#### Step 9: Update progress (~5 phút)

Update `speed/study-progress.md`:
- L88, L100 = `skipped`
- L89-L99, L101-L109 = `done`

**Verify:** Đúng 21 lectures `done` trong range này

---

## Edge Cases

| Tình huống | Xử lý |
|---|---|
| Conditional types quá phức tạp (L108-L109) | OK để NOT fully understand — mark `done` và revisit in expert mode |
| `infer` không hiểu sau 2 lần đọc | Note: "infer = type variable in conditional" — đủ cho speed mode |
| Overloads dễ nhầm với generics | Both solve "multiple call signatures" but differently — note the tradeoff |
| Session 1 dài hơn 2h | Split: end session 1 sau step 4, start session 2 với step 5 |

## Acceptance Criteria

**AC1 — Happy path: Discriminated union**
- **Given** code `type Shape = {kind: "circle"; r: number} | {kind: "square"; s: number}`
- **When** hỏi cách compute area type-safely
- **Then** viết đúng `switch(shape.kind)` với `case "circle": return Math.PI * shape.r ** 2`

**AC2 — Happy path: Generic function**
- **Given** cần viết function lấy last element của array bất kỳ type
- **When** viết function
- **Then** output `function last<T>(arr: T[]): T | undefined { return arr[arr.length - 1] }` (hoặc tương đương)

**AC3 — Negative: infer only in conditional**
- **Given** code `type X<T> = infer U` (không trong conditional)
- **When** hỏi có compile không
- **Then** trả lời đúng: "No — infer only valid in extends clause of conditional type"

## Technical Notes

- Discriminated unions + exhaustive switch = most important pattern from M07
- `keyof T` + `K extends keyof T` = most important pattern from M08 (appears in TS stdlib)
- `infer` = advanced — OK to not master in speed mode, revisit in expert mode
- Estimated time: ~4h total (Session 1 ~2h M07, Session 2 ~2h M08)
- Can split across 2 separate study sessions

## Risks

| Risk | Probability | Mitigation |
|---|---|---|
| M07+M08 combined = too much per sitting | High | Split flow into Session 1 + Session 2 explicitly |
| Conditional types overwhelm → quit early | Medium | Step 8 is last step — arrive there only after basics mastered |
| Skip discriminated unions (feels complex) | Low | L90-91 marked MASTER — cannot skip |

## Worklog

### [2026-05-05] Hoàn thành — 21 lectures M07+M08 trong 1 session

**Kết quả:** 21 lectures hoàn thành (L89-L109, trừ L88 + L100 đã SKIP).

**Session log:** [2026-05-05-3.md](../../learning/understanding-typescript/speed/sessions/2026-05-05-3.md)

**Output mong đợi:**
- [x] L89-L99 (M07) tất cả status=done trong study-progress.md
- [x] L100 SKIP, L101-L109 (M08) status=done trong study-progress.md
- [x] Active recall 3 câu/lecture — đã trả lời trong session log
- [x] Type guards cheat sheet: `in` / `instanceof` / discriminated union / type predicate
- [x] Generics cheat sheet: type, function, constraint, class syntax

**Key findings:**
- M07 highlights: discriminated unions (thêm `type` property literal) + exhaustiveness check với `never` default case
- `satisfies` keyword: validates type shape nhưng giữ narrow inferred type (không widen như `:` annotation)
- M08 highlights: `<T extends object>` = constraint khác hoàn toàn với class `extends` inheritance
- Generics key insight: type "flows through" function/class → không mất type info như `any`
- `as const` → converts `string[]` thành `readonly ["a", "b"]` (literal tuple, immutable)

**Progress:** 37% (67 lectures done — skips counted)

**AC checks:**
- ✅ AC1: L89-L109 → done/skipped in study-progress.md
- ✅ AC2: Có thể giải thích `<T extends SomeType>` là constraint (không phải inheritance), và tại sao generic tốt hơn `any`
- ✅ AC3: L90-91 (type guards + discriminated unions) → status=done (không skip)
- ✅ AC4: Viết được generic function với constraint: `function mergeObj<T extends object, U extends object>(a: T, b: U): T & U`
