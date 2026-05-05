---
date: 2026-05-05
type: task-worklog
task: typescript-bz0
title: "Generate speed module guides: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, speed]
---

# Generate speed module guides: understanding-typescript

## Objective
Tạo per-module learning assets cho speed mode — mỗi file là một study guide ngắn gọn mà /viec hoc đọc khi dạy session tương ứng. Active Recall KHÔNG được có đáp án — đó là nhiệm vụ của /viec hoc.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/speed/module-guides/M{N}-{name}.md`
- **Mode:** speed
- **Đọc bởi:** /viec hoc (speed mode sessions, 1 module guide per session block)
- **Expected count:** ~8-13 files (M01-Intro... M13-NodeTS)

## Input Source
- `self-explores/learning/understanding-typescript/speed/learning-plan.md` (B1 — module grouping + lecture ranges)
- `lectures/` folder — transcript files (đọc key lectures trong mỗi module)

## Steps

### Bước 1: Extract module list từ B1 (~5 phút)
Đọc `learning-plan.md`, extract module grouping. Expected:
M01-Intro, M02-TypeSystem, M03-Functions, M04-Classes, M05-Interfaces, M06-AdvancedTypes, M07-Generics, M08-TypeUtilities, M09-Decorators, M10-DnDProject, M11-Modules, M12-ReactTS, M13-NodeTS

### Bước 2: Per module — đọc transcripts (~15-20 phút/module)
Với mỗi module: đọc 3-5 key lectures (transcript files), extract concepts, code patterns, gotchas.

### Bước 3: Write module guide (6 sections) (~15 phút/module)
```markdown
## 1. Key Concepts
## 2. Code Patterns (max 10 lines/snippet)
## 3. Common Gotchas (3-5 TypeScript-specific)
## 4. Active Recall (5 câu hỏi — TUYỆT ĐỐI không có đáp án)
## 5. Quick Reference (bảng syntax/pattern)
## 6. Next Steps (link sang module tiếp)
```
**CRITICAL:** Section 4 Active Recall = ONLY questions, zero answers.

### Verify per file
- [ ] Tất cả 6 sections có nội dung
- [ ] Section 4: đếm số câu hỏi ≥5, kiểm tra không có dòng "Answer:" hay "Đáp án:"
- [ ] Section 2: code snippets valid TypeScript syntax

## Acceptance Criteria
- **Given** `speed/module-guides/` folder tồn tại với ≥8 files
- **When** /viec hoc (speed mode) đọc file cho module session
- **Then** ≥80% modules có guide (dựa số modules trong B1), active recall KHÔNG có đáp án

## Quality Rules
- [ ] ≥8 module guide files tồn tại
- [ ] Tất cả files có đủ 6 sections
- [ ] Section 4 không chứa từ "đáp án", "answer", "vì", "because" sau câu hỏi
- [ ] Code snippets compilable TypeScript syntax (không phải pseudocode)

## Dependencies
- typescript-6cd — B1 learning plan phải có trước (module grouping, lecture ranges)

## Worklog
*(Chưa bắt đầu)*
