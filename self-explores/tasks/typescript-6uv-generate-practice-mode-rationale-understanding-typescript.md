---
date: 2026-05-05
type: task-worklog
task: typescript-6uv
title: "Generate practice mode rationale: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, practice]
---

# Generate practice mode rationale: understanding-typescript

## Objective
Lưu lý do thiết kế practice mode: tại sao Build→Break→Fix phù hợp với TypeScript learning, code paths nào quan trọng nhất, challenge design rationale. File này là "north star" cho C1-C8 — mọi decision phải nhất quán với file này.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/practice/_mode-rationale.md`
- **Mode:** practice
- **Đọc bởi:** /viec hoc (practice mode initialization), C1 learning plan

## Input Source
- `self-explores/learning/understanding-typescript/_learning-rationale.md` (A6)
- `self-explores/learning/understanding-typescript/_lecture-resource-map.md` (A3 — để identify key code paths)

## Steps

### Bước 1: Đọc A6 learning rationale (~5 phút)
Extract: recommended approach cho practice mode, expert gaps, skills người dùng cần build.

### Bước 2: Phân tích key code paths (~10 phút)
Từ resources/ folder, identify top 5 code paths quan trọng nhất:
- resources/179-prj-16-finished/ — DnD project (abstract classes, decorators, observer)
- resources/190-modules-03-finished-modules/
- resources/184-prj-16-finished/ (alternative)
- resources/210-webpack-03-finished-dev-setup/
Ghi lý do tại sao mỗi path là quan trọng.

### Bước 3: Write 5 sections (~20 phút)
```
## 1. Mode Objective
## 2. Build→Break→Fix Rationale
## 3. Key Code Paths (cụ thể resources/ paths)
## 4. Challenge Design Principles
## 5. Known Limitations
```

### Verify
- [ ] Section 3 có ít nhất 3 actual paths từ resources/ folder
- [ ] Section 2 giải thích tại sao 3-step method phù hợp với TypeScript (không phải generic learning theory)
- [ ] Section 4 có nguyên tắc đủ cụ thể để C2 design challenges nhất quán

## Acceptance Criteria
- **Given** `practice/_mode-rationale.md` tồn tại
- **When** C1 (learning plan) dùng file để design challenge schedule
- **Then** tất cả 5 sections có nội dung, key code paths có actual resources/ paths

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/practice/_mode-rationale.md`
- [ ] Section 3 có ≥3 actual resources/ paths
- [ ] Section 4 có ≥3 challenge design principles cụ thể
- [ ] Section 5 (Known Limitations) trung thực về gì practice mode không cover được

## Dependencies
- typescript-41s — A6 learning rationale phải có trước

## Worklog
*(Chưa bắt đầu)*
