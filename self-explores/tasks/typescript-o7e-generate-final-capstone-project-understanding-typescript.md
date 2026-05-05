---
date: 2026-05-05
type: task-worklog
task: typescript-o7e
title: "Generate final capstone project: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, practice]
---

# Generate final capstone project: understanding-typescript

## Objective
Tạo capstone project spec — end-to-end TypeScript project mà user phải build để chứng minh mastery. KHÔNG có solution hay hints — chỉ spec, requirements, và review criteria.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/practice/final-capstone.md`
- **Mode:** practice
- **Đọc bởi:** /viec hoc (khi user ready cho capstone), E1 quality gate

## Input Source
- `self-explores/learning/understanding-typescript/practice/_practice-mastery-path.md` (C6 — Final Capability, Completion Rule)

## Steps

### Bước 1: Đọc C6 mastery path (~5 phút)
Extract: Final Capability statement, L5 milestone description. Capstone phải target L5.

### Bước 2: Design project (~20 phút)
Project phải:
- Require ≥6 TypeScript features (interfaces, generics, decorators, modules, type guards, utility types)
- Buildable từ scratch trong 4-8 hours
- Verifiable (AI có thể chấm pass/fail)
- Real-world relevant (không phải "hello world")

Ví dụ scope: CLI task manager, REST API client với typing, typed event system.

### Bước 3: Write 7 sections (~20 phút)
```markdown
## Project Goal (1 câu)
## Scope (in/out)
## Technical Requirements (features phải dùng, với lý do)
## Steps (numbered, chi tiết — không có code)
## Acceptance Criteria (Given/When/Then cho ≥3 core features)
## Review Checklist (tự review)
## Expert Review Criteria (tiêu chuẩn AI chấm)
```
**CRITICAL:** Không có solution, không có code examples trong spec.

### Verify
- [ ] Technical Requirements: ≥6 TypeScript features listed
- [ ] AC: ≥3 Given/When/Then cho core features
- [ ] Steps: có steps cụ thể nhưng không có code
- [ ] Expert Review Criteria: measurable, không phải "looks good"

## Acceptance Criteria
- **Given** `practice/final-capstone.md` tồn tại
- **When** /viec hoc giới thiệu capstone cho user
- **Then** có đủ 7 sections, AC Given/When/Then cho ≥3 core features, expert review criteria rõ ràng

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/practice/final-capstone.md`
- [ ] 7 sections đều có nội dung
- [ ] Technical Requirements: ≥6 TypeScript features
- [ ] AC: ≥3 Given/When/Then entries
- [ ] KHÔNG có solution code trong file

## Dependencies
- typescript-bgz — C6 mastery path phải có trước (Final Capability, level definitions)

## Worklog
*(Chưa bắt đầu)*
