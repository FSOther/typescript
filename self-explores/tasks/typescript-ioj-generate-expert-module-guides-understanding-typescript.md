---
date: 2026-05-05
type: task-worklog
task: typescript-ioj
title: "Generate expert module guides: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, expert]
---

# Generate expert module guides: understanding-typescript

## Objective
Tạo per-module expert guides với deliberate practice material. KHÔNG phải summary — là material buộc user hiểu bản chất. Self-review Questions KHÔNG có đáp án.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/expert/module-guides/M{N}-{name}.md`
- **Mode:** expert
- **Đọc bởi:** /viec hoc (expert sessions), D3/D4/D6 extract từ đây
- **Expected count:** ~8-13 files

## Input Source
- `self-explores/learning/understanding-typescript/expert/learning-plan.md` (D1 — module plan with mental models)
- `lectures/` folder — key transcripts per module

## Steps

### Bước 1: Extract module list từ D1 (~3 phút)
Lấy module names, lecture ranges, mental models, top mistakes.

### Bước 2: Per module — đọc key transcripts (~15 phút/module)
Tập trung vào lectures có complexity cao (advanced types, generics, decorators). Tìm: expert gotchas, professional concerns, real-world patterns.

### Bước 3: Write 9 sections per module (~20 phút/module)
```markdown
## 1. Core Mental Model
## 2. Beginner Misunderstanding
## 3. Professional Concern
## 4. Mistake Traps (KHÔNG có solution ngay)
## 5. Expert Shortcut
## 6. Real-world Scenario
## 7. Deliberate Drill
## 8. Expert Acceptance Criteria
## 9. Self-review Questions (KHÔNG có đáp án)
```
**CRITICAL:** Section 9 = only 3 questions, zero answers.

### Verify per file
- [ ] 9 sections đều có nội dung
- [ ] Section 4: describe trap nhưng không có "solution: use X instead"
- [ ] Section 9: 3 câu hỏi, không có dòng "Answer:" hay "Trả lời:"
- [ ] Section 7: drill có output requirement cụ thể

## Acceptance Criteria
- **Given** `expert/module-guides/` folder tồn tại với ≥8 files
- **When** /viec hoc (expert mode) đọc guide
- **Then** ≥80% modules có guide, self-review questions KHÔNG có đáp án

## Quality Rules
- [ ] ≥8 module guide files tồn tại
- [ ] Tất cả files có đủ 9 sections
- [ ] Section 9: không có đáp án
- [ ] Section 4: describe trap chứ không giải quyết nó (tạo cognitive dissonance)

## Dependencies
- typescript-iun — D1 expert learning plan phải có trước

## Worklog
*(Chưa bắt đầu)*
