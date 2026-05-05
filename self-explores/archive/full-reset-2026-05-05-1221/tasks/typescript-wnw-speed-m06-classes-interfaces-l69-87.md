---
date: 2026-05-05
type: task-worklog
task: typescript-wnw
title: "Speed Mode — M06 Classes & Interfaces L69-87"
status: open
detail_score: ready-for-dev
tags: [typescript, learning, classes, interfaces, speed-mode]
---

# Speed Mode — M06 Classes & Interfaces L69-87

## Objective

Hoàn thành M06 theo **speed mode**: nắm OOP trong TypeScript — classes, access modifiers, abstract classes, interfaces. Trọng tâm: hiểu `interface` vs `type`, `implements` vs `extends`, khi nào dùng abstract class.

## Scope

**In scope:**
- L67-L68: Classes intro — SKIP (basic JS class, không có TS-specific)
- L69: Constructor shorthand (`private name: string` trong constructor params)
- L70: Access modifiers (`public`, `private`, `protected`)
- L71: Readonly
- L72: Inheritance (extends, super)
- L73: Override & protected
- L74: Getters/setters
- L75: Static properties/methods
- L76-L77: Abstract classes
- L78: `private` vs TypeScript `private` vs `#` (true private)
- L79-L80: Interfaces intro, interface as contract
- L81-L82: Implementing interfaces (`implements`)
- L83: Interfaces vs type aliases
- L84: Extending interfaces
- L85: Optional interface properties
- L86-L87: Readonly interface, function type in interface

**Out of scope:**
- Runtime OOP patterns (design patterns — not in scope)
- Mixins (covered in M07 advanced types)
- Deep expert WHY (bivariance, structural typing implications)

## Input / Output

**Input:** Lecture transcripts `lectures/69.*.txt` → `lectures/87.*.txt`

**Output:**
- `speed/study-progress.md` — L69-L87 tất cả status=done
- Mental model: "Interface = shape contract. Abstract class = partial implementation with forced overrides."

## Dependencies

- **Requires:** `typescript-1tu` (M02) phải done trước
  - Cần hiểu type aliases, function types từ M02
- L67-L68 đã SKIP

## Flow

### Step 1: L69-L71 — Constructor shorthand & Access modifiers (~15 phút)

```bash
cat lectures/69.*.txt lectures/70.*.txt lectures/71.*.txt
```

**Đọc và trả lời:**
- `constructor(private name: string)` làm gì khác với `constructor(name: string) { this.name = name }`?
- `private` trong TS vs `#` trong JS — difference?
- `readonly` field: có thể thay đổi sau constructor không?

**Verify:** Biết 3 levels access modifier và khi nào dùng

---

### Step 2: L72-L75 — Inheritance, Override, Getters, Static (~20 phút)

```bash
cat lectures/72.*.txt lectures/73.*.txt lectures/74.*.txt lectures/75.*.txt
```

**Đọc và trả lời:**
- `override` keyword (TS 4.3+) — tại sao quan trọng?
- `protected` — accessible ở đâu? (class + subclasses, KHÔNG phải instances)
- Static property: accessed via class name hay instance?

**Verify:** Biết `this` trong static method refer đến class, không phải instance

---

### Step 3: L76-L78 — Abstract classes (~15 phút)

```bash
cat lectures/76.*.txt lectures/77.*.txt lectures/78.*.txt
```

**Đọc và trả lời:**
- Abstract class vs Interface — 2 differences chính?
- Khi nào dùng abstract class thay interface?
- Có thể instantiate abstract class không?

**Mental model:** "Abstract class = interface + default implementation. Use when subclasses share code."

**Verify:** Biết `new AbstractClass()` = compile error

---

### Step 4: L79-L83 — Interfaces & implements (~25 phút)

```bash
cat lectures/79.*.txt lectures/80.*.txt lectures/81.*.txt lectures/82.*.txt lectures/83.*.txt
```

**Đọc và trả lời:**
- Interface được compile thành gì? (Không có runtime representation)
- `implements` vs `extends` — syntactic + semantic difference?
- `interface` vs `type` — 3 differences (declaration merging, extends syntax, mapped types)

**Key fact:** Interface có declaration merging — `type` không có.

**Verify:** Biết `interface Foo { x: number } interface Foo { y: string }` = valid (merged)

---

### Step 5: L84-L87 — Extending, Optional, Readonly, Function types (~15 phút)

```bash
cat lectures/84.*.txt lectures/85.*.txt lectures/86.*.txt lectures/87.*.txt
```

**Đọc:**
- `interface Child extends Parent1, Parent2` — multiple inheritance possible?
- Optional `?` in interface vs optional chaining `?.`
- Function type trong interface: `greet(name: string): void`

**Verify:** Biết interface có thể extend multiple interfaces (không giống class)

---

### Step 6: Update progress (~5 phút)

Mở `self-explores/learning/understanding-typescript/speed/study-progress.md`, update L69-L87 → `done`.

**Verify:** L67-L68 vẫn là `skipped`, L69-L87 tất cả `done`

---

## Edge Cases

| Tình huống | Xử lý |
|---|---|
| Nhầm `interface` và `type` — bài nào dùng cũng được | Note rule: "Prefer interface for objects/APIs, type for unions/primitives/computed" |
| L78 `#private` field syntax mới | Note: TS `private` = compile-time only, `#` = runtime private |
| Không nhớ override keyword | Re-read L73, đây là TS 4.3 feature — important for team codebases |

## Acceptance Criteria

**AC1 — Happy path: Progress cập nhật**
- **Given** đọc xong L69-L87
- **When** check `speed/study-progress.md`
- **Then** L69-L87 = `done`, L67-L68 = `skipped`

**AC2 — Happy path: Hiểu interface vs type**
- **Given** câu hỏi "khi nào dùng interface vs type?"
- **When** trả lời
- **Then** mention: (1) interface for object shapes, (2) type for unions/intersections, (3) interface has declaration merging, (4) both work for implements

**AC3 — Negative: Abstract class không instantiable**
- **Given** code `abstract class Shape { abstract draw(): void }`
- **When** hỏi `new Shape()` có lỗi không
- **Then** trả lời đúng: "Yes, compile error — abstract class cannot be instantiated"

## Technical Notes

- `private` trong TS = compile-time check only; JS output vẫn accessible
- `#field` = ES2022 true private = WeakMap-backed, runtime inaccessible
- Declaration merging chỉ có ở `interface`, không có ở `type alias`
- Estimated time: ~1.75h (19 lectures × ~5.5 phút)

## Risks

| Risk | Probability | Mitigation |
|---|---|---|
| Confusion: interface vs abstract class | High | Step 3 + Step 4 đều có active recall questions về topic này |
| Skip L78 `#private` (cảm giác như JS) | Medium | L78 labeled DEEP_DIVE trong expert, NOTE trong speed — đọc nhẹ |
| Start M06 before M02 done | Low | beads dep chain enforces order |

## Worklog
*(Chưa bắt đầu — sẽ ghi log khi `/viec bat-dau`)*
