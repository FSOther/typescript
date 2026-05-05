---
date: 2026-05-05
type: task-worklog
task: typescript-3b1
title: "Generate practice coverage matrix: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, practice]
---

# Generate practice coverage matrix: understanding-typescript

## Objective
Tạo matrix tracking concept × skill coverage trong practice mode. Dùng để identify gaps trước khi mark practice complete và để /viec hoc tìm challenges theo concept nhanh.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/practice/practice-coverage-matrix.md`
- **Mode:** practice
- **Đọc bởi:** /viec hoc (finding challenges by concept), E1 quality gate

## Input Source
- `self-explores/learning/understanding-typescript/practice/practice-challenges.md` (C3 — challenge list)
- `self-explores/learning/understanding-typescript/_knowledge-map.md` (A4 — concept list)

## Steps

### Bước 1: Extract concepts từ A4 knowledge map (~5 phút)
Lấy concept list (≥80 concepts). Nhóm theo category (Type System, Classes, Generics, etc.).

### Bước 2: Map challenges từ C3 → concepts (~15 phút)
Với mỗi challenge trong practice-challenges.md:
- Identify concepts nó cover
- Map vào skills: Build / Break / Fix / Explain / Apply

### Bước 3: Write matrix (~15 phút)
Format:
```markdown
| Concept | Build | Break | Fix | Explain | Apply | Covered By |
|---------|-------|-------|-----|---------|-------|------------|
| Type inference | yes | yes | - | - | partial | C02-a, C02-b |
| Union types | yes | - | yes | - | - | C02-a |
...
```
- yes = có challenge cover
- partial = có challenge nhưng superficial
- `-` = gap

### Bước 4: Add Gap Analysis section (~5 phút)
List tất cả concepts có nhiều "-" entries → đây là gaps cần thêm challenges.

### Verify
- [ ] ≥80% concepts từ A4 có entry trong matrix
- [ ] Gap Analysis section có ít nhất 5 gaps identified
- [ ] "Covered By" column có challenge IDs thực tế từ C3

## Acceptance Criteria
- **Given** `practice/practice-coverage-matrix.md` tồn tại
- **When** E1 quality gate scan coverage
- **Then** ≥80% concepts từ knowledge map có entry, coverage gaps ghi rõ

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/practice/practice-coverage-matrix.md`
- [ ] ≥80% concepts có entry (dựa theo A4 concept count)
- [ ] "Covered By" column không để trống nếu ô là "yes" hoặc "partial"
- [ ] Gap Analysis section tồn tại và honest

## Dependencies
- typescript-a1v — C3 practice challenges phải có trước (challenge IDs và skills)

## Worklog
*(Chưa bắt đầu)*
