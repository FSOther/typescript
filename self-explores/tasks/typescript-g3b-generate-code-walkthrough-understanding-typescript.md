---
date: 2026-05-05
type: task-worklog
task: typescript-g3b
title: "Generate code walkthrough: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, practice]
---

# Generate code walkthrough: understanding-typescript

## Objective
Tạo walkthrough qua code examples trong resources/ — giải thích patterns, anti-patterns cho TypeScript. Focus vào finished project code (không phải starting setups). Giúp user hiểu "tại sao" code viết như vậy, không chỉ "như thế nào".

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/practice/code-walkthrough.md`
- **Mode:** practice
- **Đọc bởi:** /viec hoc (practice sessions khi giải thích patterns), expert mode (D5 architecture critique reference)

## Input Source
- `self-explores/learning/understanding-typescript/_lecture-resource-map.md` (A3 — code paths)
- `resources/179-prj-16-finished/` — DnD project finished
- `resources/184-prj-16-finished/` — alternative DnD
- `resources/190-modules-03-finished-modules/` — ES modules
- `resources/210-webpack-03-finished-dev-setup/` — Webpack + TS

## Steps

### Bước 1: List key .ts files (~10 phút)
```bash
find resources/179-prj-16-finished/ resources/184-prj-16-finished/ resources/190-modules-03-finished-modules/ resources/210-webpack-03-finished-dev-setup/ -name "*.ts" | head -20
```
Chọn 5-7 files chứa patterns quan trọng nhất.

### Bước 2: Per pattern — read và analyze (~15 phút/pattern)
Đọc file source, identify:
- Tên pattern
- File path thực (resources/{N}-{slug}/{file.ts})
- TypeScript feature demonstrated
- Common mistake / anti-pattern version (viết code ví dụ)
- Tại sao correct version tốt hơn

### Bước 3: Write walkthrough (~20 phút)
≥5 patterns bắt buộc cover:
1. Abstract generic base class (resources/179-prj-16-finished/)
2. Singleton pattern với `private constructor()`
3. Observer/Listener types
4. @autobind decorator
5. HTML5 DnD event typing
(+2 more từ modules/webpack nếu có)

Format per pattern: 5 sub-sections như mô tả ở trên.

### Verify
- [ ] ≥5 patterns covered
- [ ] Mỗi pattern có actual file path từ resources/ (không phải generic)
- [ ] Anti-pattern section có code example (không chỉ text mô tả)

## Acceptance Criteria
- **Given** `practice/code-walkthrough.md` tồn tại
- **When** /viec hoc (practice mode) giải thích pattern cho user
- **Then** file cover ≥5 key TypeScript patterns, mỗi pattern có actual file path, anti-pattern có code example

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/practice/code-walkthrough.md`
- [ ] ≥5 patterns với actual resources/ file paths
- [ ] Mỗi pattern có code example cho anti-pattern (không chỉ text)
- [ ] Code examples valid TypeScript syntax

## Dependencies
- typescript-avw — A3 lecture-resource map phải có trước (để identify đúng code paths)

## Worklog
*(Chưa bắt đầu)*
