---
date: 2026-05-05
type: task-worklog
task: typescript-7h6
title: "Generate practice learning plan: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, practice]
---

# Generate practice learning plan: understanding-typescript

## Objective
Tạo learning plan cho practice mode — Build→Break→Fix approach, challenge schedule theo module. Ai đọc file này biết ngay làm module nào, challenge nào, theo thứ tự gì.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/practice/learning-plan.md`
- **Mode:** practice
- **Đọc bởi:** /viec hoc (practice sessions), C2 (module guides cần biết module list), C5 (study-progress)

## Input Source
- `self-explores/learning/understanding-typescript/practice/_mode-rationale.md` (C0)
- `self-explores/learning/understanding-typescript/_learning-index.md` (A5)

## Steps

### Bước 1: Đọc C0 rationale (~5 phút)
Extract: module grouping approach, challenge design principles, key code paths.

### Bước 2: Build module breakdown (~20 phút)
Dựa theo same module grouping như speed mode (M01-M13):
- Module N: Build Task (from scratch) + Break→Fix cycle
- Mỗi module estimate: ~45-90 phút (tùy complexity)
- Resources folder liên quan cho mỗi module

### Bước 3: Write 5 sections (~15 phút)
```
## 1. Overview (approach, tổng thời gian: ~15-25h)
## 2. Module Breakdown (module N: challenges, time)
## 3. Build→Break→Fix Guide (step-by-step method)
## 4. Progression Rules (khi nào advance)
## 5. Resource Usage (cách dùng resources/)
```

### Verify
- [ ] Module Breakdown cover tất cả modules trong A5 section map
- [ ] Build→Break→Fix Guide đủ cụ thể để C2 follow khi tạo challenges
- [ ] Progression Rules rõ ràng (e.g., "phải pass 2/3 challenges trước khi advance")

## Acceptance Criteria
- **Given** `practice/learning-plan.md` tồn tại
- **When** /viec hoc (practice mode) đọc để plan session
- **Then** tất cả modules covered, progression rules rõ ràng, Build→Break→Fix Guide có đủ detail

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/practice/learning-plan.md`
- [ ] Module Breakdown: tất cả ≥8 modules có entry
- [ ] Section 3 (Build→Break→Fix Guide): ≥3 concrete steps per stage
- [ ] Section 4 (Progression Rules): có measurable criteria

## Dependencies
- typescript-6uv — C0 practice rationale phải có trước
- typescript-yxl — A5 indexes phải initialized

## Worklog
*(Chưa bắt đầu)*
