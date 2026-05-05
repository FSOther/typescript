---
date: 2026-05-05
type: task-worklog
task: typescript-1tu
title: "Speed Mode — M02 Type System L13-39"
status: open
detail_score: ready-for-dev
tags: [typescript, learning, type-system, speed-mode]
---

# Speed Mode — M02 Type System L13-39

## Objective

Hoàn thành module M02 theo **speed mode**: đọc transcript, nắm syntax + mental model cốt lõi, trả lời active recall questions, đánh dấu progress. Không cần code along — chỉ cần hiểu đủ để dùng được trong 2-3 tuần.

## Scope

**In scope:**
- L13-L14: Types overview, TypeScript purpose
- L15: Type inference
- L16: Function parameter types
- L17: `any` type — tại sao tránh
- L18: Union types (`string | number`)
- L19-L20: Array types (`string[]`, `Array<string>`)
- L21: Generics preview (chỉ NOTE — xem lại ở M08)
- L22: Tuples
- L23: Object types (inline shape)
- L24: Non-null assertion `!`
- L25: Record type (`Record<K, V>`)
- L26: Enums — tại sao CRITICAL (reverse mapping pitfall)
- L27: Literal types (`"left" | "right"`)
- L28: Type aliases (`type Point = {x: number; y: number}`)
- L29-L32: Function types (`(x: number) => string`)
- L33: `null` & `undefined` handling
- L34-L35: Type narrowing (`typeof`, `in`)
- L36: Type casting (`as`, `<T>`)
- L37: `unknown` type
- L38-L39: Optional chaining `?.` & nullish coalescing `??`

**Out of scope:**
- Code-along exercises (practice mode)
- Deep WHY analysis (expert mode)
- L40+ (tsconfig — M03)

## Input / Output

**Input:** Lecture transcripts `lectures/13.*.txt` → `lectures/39.*.txt`

**Output:**
- `speed/study-progress.md` — L13-L39 tất cả status=done
- Active recall answers (mental, không cần viết ra)
- Mental model: "TypeScript = JavaScript với type labels — compiler strips them at runtime"

## Dependencies

- L1-L12 đã SKIP (setup, không cần)
- Không có task dependency trong beads

## Flow

### Step 1: L13-L14 — Types overview (~10 phút)

```bash
cat lectures/13.*.txt lectures/14.*.txt
```

**Đọc và trả lời:**
- TypeScript giải quyết vấn đề gì mà JavaScript không giải quyết được?
- Difference giữa `string` (TS type) và `String` (JS constructor)?

**Verify:** Có thể giải thích: "TS type annotations compile away — runtime không thấy chúng"

---

### Step 2: L15-L16 — Inference & Function types (~15 phút)

```bash
cat lectures/15.*.txt lectures/16.*.txt
```

**Đọc và trả lời:**
- Khi nào inference đủ, khi nào cần annotate tường minh?
- Bivariance bug trong function parameters là gì? (expert note)

**Mental model:** "Annotate function signatures, let TS infer the rest"

**Verify:** Biết khi nào cần viết `: string` vs để TS tự infer

---

### Step 3: L17-L18 — any & Union types (~15 phút)

```bash
cat lectures/17.*.txt lectures/18.*.txt
```

**Đọc và trả lời:**
- `any` làm mất gì? Khi nào dùng `unknown` thay thế?
- `string | number` — narrowing cần làm gì trước khi dùng?

**Verify:** Có thể viết union type và narrow đúng cách

---

### Step 4: L19-L21 — Arrays & Generics preview (~10 phút)

```bash
cat lectures/19.*.txt lectures/20.*.txt lectures/21.*.txt
```

**Đọc:** `string[]` = `Array<string>`. L21 chỉ NOTE — generics chi tiết ở M08.

**Verify:** Biết 2 cú pháp array type, biết L21 liên quan M08

---

### Step 5: L22-L25 — Tuples, Objects, Non-null, Record (~20 phút)

```bash
cat lectures/22.*.txt lectures/23.*.txt lectures/24.*.txt lectures/25.*.txt
```

**Đọc và trả lời:**
- Tuple vs array — difference là gì?
- `!` non-null assertion — khi nào an toàn? Khi nào nguy hiểm?
- `Record<string, number>` = gì? Có thể viết bằng index signature không?

**Verify:** Biết `[string, number]` là tuple, `string[]` là array

---

### Step 6: L26-L28 — Enums, Literals, Type Aliases (~20 phút)

```bash
cat lectures/26.*.txt lectures/27.*.txt lectures/28.*.txt
```

**Đọc (CRITICAL cho L26):**
- Enum compiles thành gì? Reverse mapping pitfall là gì?
- Tại sao `"left" | "right"` thường tốt hơn enum?
- `type` vs `interface` — khi nào dùng cái nào?

**Verify:** Biết `enum Direction { Up = 1 }` — `Direction[1]` = `"Up"` (reverse mapping)

---

### Step 7: L29-L32 — Function types (~15 phút)

```bash
cat lectures/29.*.txt lectures/30.*.txt lectures/31.*.txt lectures/32.*.txt
```

**Đọc:**
- Function type syntax: `(x: number, y: number) => number`
- Void vs never — difference?
- Optional params `?`, default params

**Verify:** Có thể annotate callback type trong higher-order function

---

### Step 8: L33-L39 — Null, Narrowing, Casting, unknown, Optional (~30 phút)

```bash
cat lectures/33.*.txt lectures/34.*.txt lectures/35.*.txt lectures/36.*.txt lectures/37.*.txt lectures/38.*.txt lectures/39.*.txt
```

**Đọc và trả lời:**
- `null` vs `undefined` trong TS — strict null checks?
- 3 cách narrow type: `typeof`, `in`, `instanceof`
- `as Type` vs `<Type>` — khi nào dùng cái nào?
- `unknown` vs `any` — practical difference?

**Verify:** Có thể narrow `string | null` và access safely

---

### Step 9: Update progress (~5 phút)

Mở `self-explores/learning/understanding-typescript/speed/study-progress.md`, update tất cả L13-L39 từ `todo` → `done`.

**Verify:** `grep -c "done" speed/study-progress.md` tăng ít nhất 20 dòng

---

## Edge Cases

| Tình huống | Xử lý |
|---|---|
| Transcript file bị thiếu | `find lectures/ -name "*.txt" \| sort -n` để check |
| Lecture đề cập syntax TS 5.x mới | Note lại, continue — chỉ cần mental model |
| Không hiểu một khái niệm sau 2 lần đọc | Đánh dấu `in_progress` thay `done`, skip và quay lại |
| Mất tập trung sau 45 phút | Nghỉ 10 phút, tiếp tục từ step chưa xong |

## Acceptance Criteria

**AC1 — Happy path: Hoàn thành module**
- **Given** tất cả transcript L13-L39 đã được đọc
- **When** check study-progress.md
- **Then** tất cả L13-L39 có status=done, progress_percent tăng lên ~10%

**AC2 — Happy path: Hiểu đủ để dùng**
- **Given** đã học xong M02
- **When** gặp code `const x: string | null = getUserName(); if (x !== null) { x.toUpperCase() }`
- **Then** có thể giải thích tại sao code đúng và `x.toUpperCase()` an toàn trong branch đó

**AC3 — Negative: Không skip critical lectures**
- **Given** L26 (Enums) được đánh dấu CRITICAL
- **When** hoàn thành M02
- **Then** L26 phải có status=done, KHÔNG được có status=skipped

## Technical Notes

- File naming: `lectures/13.Working_With_Types.txt` — dùng glob `lectures/13.*.txt` để tránh type nhầm tên
- Speed mode: chỉ cần đọc + active recall, KHÔNG cần mở editor viết code
- Estimated time: ~2.5h (20 lectures × 7.5 phút trung bình)
- Session split: có thể dừng sau step 5 (L25) và tiếp tục session sau

## Risks

| Risk | Probability | Mitigation |
|---|---|---|
| Đọc quá nhanh, không nhớ | Medium | Trả lời active recall questions trước khi qua step tiếp theo |
| Transcript có lỗi chính tả code | Low | Đọc ý, không copy code trong speed mode |
| L26 enum pitfall bị miss | Medium | Đánh dấu CRITICAL — đọc kỹ hơn step 6 |

## Worklog

### [2026-05-05] Bắt đầu — Claimed task, đọc toàn bộ L13-L39

**Kết quả:** 27 lectures hoàn thành trong 1 session.

**Session log:** [2026-05-05-1.md](../../learning/understanding-typescript/speed/sessions/2026-05-05-1.md)

**Output mong đợi:**
- [x] L13-L39 tất cả status=done trong study-progress.md
- [x] Active recall 3 câu/lecture — đã trả lời trong session log
- [x] Mental model: TypeScript = JS với type labels, stripped at compile time

**Key findings:**
- Core types: `string`, `number`, `boolean`, `string[]`, `[T, T]` (tuple), `{ key: T }`, `Record<K, V>`
- Special types: `any` (avoid), `unknown` (safe any), `void` (no return), `never` (no completion), `{}` (non-null)
- Operators: `|` union, `?` optional, `!` non-null assertion, `?.` optional chaining, `??` nullish coalescing, `as` cast
- Critical: Enum pitfall (compiles to numbers), `{}` ≠ empty object
- Best practice: Trust inference, explicit only when no initial value

**Progress:** 16% (27/172 lectures done — skips counted)

**AC checks:**
- ✅ AC1: L13-L39 → done in study-progress.md
- ✅ AC2: Code `const x: string | null; if (x !== null) { x.toUpperCase() }` — x narrowed to `string` after null check → safe
- ✅ AC3: L26 Enums → status=done (không skip)
