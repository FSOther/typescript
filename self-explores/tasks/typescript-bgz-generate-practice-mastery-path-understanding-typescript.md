---
date: 2026-05-05
type: task-worklog
task: typescript-bgz
title: "Generate practice mastery path: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, practice]
---

# Generate practice mastery path: understanding-typescript

## Objective
Định nghĩa đích cuối của practice mode — user sau khi hoàn thành có thể làm được gì. Milestone ladder, completion rule, và coverage matrix framework. C7 (capstone) và E1 (quality gate) đều phụ thuộc vào file này.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/practice/_practice-mastery-path.md`
- **Mode:** practice
- **Đọc bởi:** /viec hoc (context về mastery goal), C7 (capstone design), E1 quality gate

## Input Source
- `self-explores/learning/understanding-typescript/practice/module-guides/` (C2 — module list, concepts per module)

## Steps

### Bước 1: List concepts từ C2 module guides (~10 phút)
Đọc `## 1. Key Concepts` từ mỗi module guide. Tổng hợp list ≥80 concepts.

### Bước 2: Define Final Capability (~10 phút)
Viết 3-5 câu: user sau khi xong practice mode có thể build/debug/refactor/explain/apply TypeScript như thế nào.

### Bước 3: Design Milestone Ladder (~15 phút)
```
L1 → hiểu TypeScript concepts khi đọc code
L2 → làm theo examples, modify code
L3 → tự debug và fix TypeScript errors
L4 → tự design types, interfaces, generics
L5 → áp dụng vào real project từ đầu
```
Mỗi level: concrete capability + example task.

### Bước 4: Write Coverage Matrix framework (~10 phút)
Table header: `| Concept | Build | Break | Fix | Explain | Apply | Covered By |`
List tất cả concepts (từ Bước 1), để các cells trống (C3/C8 sẽ fill).

### Bước 5: Define Completion Rule (~5 phút)
Criteria cụ thể: ≥80% challenges done + ≥1 capstone + Feynman pass + final checklist.

### Verify
- [ ] Milestone Ladder có đủ 5 levels, mỗi level có concrete example
- [ ] Coverage matrix có ≥80% concepts từ knowledge map
- [ ] Completion Rule có measurable criteria (percentages, counts)

## Acceptance Criteria
- **Given** `practice/_practice-mastery-path.md` tồn tại
- **When** C7 (capstone) dùng file để design final project spec
- **Then** tất cả 5 sections có nội dung, milestone ladder có 5 levels, coverage matrix có ≥80% concepts

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/practice/_practice-mastery-path.md`
- [ ] Milestone ladder: 5 levels với concrete examples
- [ ] Coverage matrix: ≥80% concepts từ A4 knowledge map
- [ ] Completion Rule: measurable (percentages, counts, không phải "feel ready")

## Dependencies
- typescript-ygy — C2 module guides phải có trước (concepts và modules list)

## Worklog
*(Chưa bắt đầu)*
