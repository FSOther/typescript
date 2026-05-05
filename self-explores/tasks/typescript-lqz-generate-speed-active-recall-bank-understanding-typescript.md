---
date: 2026-05-05
type: task-worklog
task: typescript-lqz
title: "Generate speed active recall bank: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, speed]
---

# Generate speed active recall bank: understanding-typescript

## Objective
Tổng hợp tất cả active recall questions từ module guides thành 1 bank có cấu trúc. Dùng để spaced repetition review và giúp /viec hoc điều phối review sessions.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/speed/active-recall-bank.md`
- **Mode:** speed
- **Đọc bởi:** /viec hoc (review sessions), E1 quality gate

## Input Source
- `self-explores/learning/understanding-typescript/speed/module-guides/` (B3 — extract questions từ Section 4 của mỗi file)

## Steps

### Bước 1: List all module guide files (~2 phút)
```bash
ls self-explores/learning/understanding-typescript/speed/module-guides/*.md
```
Verify: ≥8 files tồn tại.

### Bước 2: Extract Active Recall questions (~15 phút)
Với mỗi module guide: đọc `## 4. Active Recall` section, copy tất cả questions. Tag với module name.

### Bước 3: Build table (~15 phút)
Format: `| Q# | Question | Module | Difficulty | Tag |`
- Difficulty: easy/medium/hard (dựa theo concept depth)
- Tag: loại câu hỏi (syntax/concept/application/gotcha)
- Số thứ tự Q liên tục

### Bước 4: Add spaced repetition guide (~5 phút)
Section cuối: hướng dẫn dùng bank (review after session 1: Q1-Q10, after session 2: Q1-Q20 + new...)

### Verify
- [ ] Đếm rows: ≥30 questions
- [ ] Mỗi module có ≥2 entries trong table
- [ ] Không có answers trong column "Question"

## Acceptance Criteria
- **Given** `speed/active-recall-bank.md` tồn tại
- **When** /viec hoc review session đọc file
- **Then** ≥30 questions, phủ ≥80% modules (≥80% của số module guides đã tạo), có difficulty rating

## Quality Rules
- [ ] ≥30 questions trong table
- [ ] Tất cả questions có Module column filled
- [ ] Tất cả questions có Difficulty (easy/medium/hard)
- [ ] Không có duplicate questions (cùng nội dung)

## Dependencies
- typescript-bz0 — B3 module guides phải có trước (source của questions)

## Worklog
*(Chưa bắt đầu)*
