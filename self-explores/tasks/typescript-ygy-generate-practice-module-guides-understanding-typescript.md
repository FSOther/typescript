---
date: 2026-05-05
type: task-worklog
task: typescript-ygy
title: "Generate practice module guides: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, practice]
---

# Generate practice module guides: understanding-typescript

## Objective
Tạo per-module Build→Break→Fix guides cho practice mode. KHÔNG viết solution — chỉ spec, hints, và AC. /viec hoc đọc file này khi dạy practice session cho module tương ứng.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/practice/module-guides/M{N}-{name}.md`
- **Mode:** practice
- **Đọc bởi:** /viec hoc (practice sessions), C3 (practice challenges extract từ đây)
- **Expected count:** ~8-13 files (same module grouping as speed)

## Input Source
- `self-explores/learning/understanding-typescript/practice/learning-plan.md` (C1 — module list, approach)
- `self-explores/learning/understanding-typescript/_lecture-resource-map.md` (A3 — code paths per module)

## Steps

### Bước 1: Extract module list từ C1 (~3 phút)
Lấy module names và lecture ranges từ learning-plan.md.

### Bước 2: Per module — identify code path (~5 phút)
Từ A3 map, tìm resources/ folder liên quan đến module này.

### Bước 3: Write 7 sections per module (~20 phút/module)
```markdown
## 1. Build Task (spec only — KHÔNG có solution)
## 2. Break It (cách phá code, liệt kê)
## 3. Fix It (hints only — KHÔNG có solution)
## 4. Explain It (Feynman: giải thích cho người mới)
## 5. Apply It (đưa vào project thực)
## 6. Acceptance Criteria (Given/When/Then)
## 7. Resources (links resources/ folder)
```
**CRITICAL:** Section 1 và 3 = zero solutions, only spec and hints.

### Verify per file
- [ ] 7 sections đều có nội dung
- [ ] Section 1: chỉ có spec/requirements, không có code solution
- [ ] Section 3: chỉ có hints/questions, không có "fix bằng cách..."
- [ ] Section 6: có Given/When/Then cho ít nhất 1 core feature

## Acceptance Criteria
- **Given** `practice/module-guides/` folder tồn tại với ≥8 files
- **When** /viec hoc (practice mode) đọc file cho module session
- **Then** ≥80% modules có guide, Build Task và Fix It KHÔNG có solution

## Quality Rules
- [ ] ≥8 module guide files tồn tại
- [ ] Tất cả files có đủ 7 sections
- [ ] Section 1 (Build Task): không có code solution, chỉ spec
- [ ] Section 3 (Fix It): không có full fix, chỉ hints
- [ ] Section 7 (Resources): có actual resources/ path từ A3 map

## Dependencies
- typescript-7h6 — C1 practice plan phải có trước (module list, approach)
- typescript-avw — A3 lecture-resource map phải có trước (code paths)

## Worklog
*(Chưa bắt đầu)*
