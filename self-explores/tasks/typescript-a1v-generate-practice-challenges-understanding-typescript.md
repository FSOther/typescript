---
date: 2026-05-05
type: task-worklog
task: typescript-a1v
title: "Generate practice challenges: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, practice]
---

# Generate practice challenges: understanding-typescript

## Objective
Tạo danh sách structured practice challenges với AC Given/When/Then. File này là inventory của tất cả challenges — C8 (coverage matrix) đọc để check gap.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/practice/practice-challenges.md`
- **Mode:** practice
- **Đọc bởi:** C8 (coverage matrix), /viec hoc (listing available challenges)

## Input Source
- `self-explores/learning/understanding-typescript/practice/module-guides/` (C2 — extract Build Task specs từ Section 1)

## Steps

### Bước 1: List tất cả module guide files (~2 phút)
```bash
ls self-explores/learning/understanding-typescript/practice/module-guides/*.md
```

### Bước 2: Per module — extract 2+ challenges (~10 phút/module)
Từ mỗi module guide:
- Challenge A: lấy Build Task (Section 1) → đặt thành Build challenge
- Challenge B: thiết kế Fix challenge (có bug TypeScript-specific)
- (Optional) Challenge C: Refactor challenge nếu module phức tạp

### Bước 3: Assign challenge metadata (~10 phút)
Format: `| Challenge ID | Module | Title | Difficulty | Given/When/Then AC | Skills Tested |`
- Challenge ID: C{module_number}-{a|b|c} (e.g., C02-a, C02-b)
- Difficulty: easy/medium/hard
- Skills Tested: link `M{N}-{name}.md#section`

### Bước 4: Verify completeness (~5 phút)
Count: ≥2 challenges/module, mỗi row có AC Given/When/Then, không có solution

### Verify
- [ ] ≥2 challenges per module (1 Build + 1 Fix minimum)
- [ ] Mỗi challenge có AC với Given/When/Then format
- [ ] Không có solution hoặc hint dẫn đến solution trong file này

## Acceptance Criteria
- **Given** `practice/practice-challenges.md` tồn tại
- **When** C8 (coverage matrix) đọc để map challenges → concepts
- **Then** ≥2 challenges/module, mỗi challenge có AC Given/When/Then đầy đủ, difficulty rating, KHÔNG có solution

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/practice/practice-challenges.md`
- [ ] ≥2 challenges per module (expect ≥16 total for 8+ modules)
- [ ] Mỗi challenge có difficulty label
- [ ] AC column có Given/When/Then, không phải chỉ "pass/fail"
- [ ] Skills Tested column điền đủ

## Dependencies
- typescript-ygy — C2 module guides phải có trước (source cho Build Task specs)

## Worklog
*(Chưa bắt đầu)*
