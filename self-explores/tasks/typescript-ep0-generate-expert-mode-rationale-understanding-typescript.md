---
date: 2026-05-05
type: task-worklog
task: typescript-ep0
title: "Generate expert mode rationale: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, expert]
---

# Generate expert mode rationale: understanding-typescript

## Objective
Lưu lý do thiết kế expert mode: tại sao deliberate practice phù hợp, top mistake traps, gap analysis so với industry standard. File này là "north star" cho D1-D7.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/expert/_mode-rationale.md`
- **Mode:** expert
- **Đọc bởi:** /viec hoc (expert mode initialization), D1 learning plan

## Input Source
- `self-explores/learning/understanding-typescript/_learning-rationale.md` (A6 — expert gap analysis section)
- Sample lectures từ `lectures/` — để verify mistake traps với evidence

## Steps

### Bước 1: Đọc A6 learning rationale (~5 phút)
Extract: expert gaps, top-level concerns về course quality, recommended approach.

### Bước 2: Research top mistake traps (~20 phút)
Đọc 5-10 key lectures (decorators, advanced types, generics). Identify 5+ bẫy kỹ thuật nguy hiểm nhất:
- type assertion misuse (as any)
- structural typing gotchas (duck typing surprises)
- decorator pitfalls (order, experimental)
- module resolution issues
- any escape hatches trong codebase

Với mỗi trap: ghi evidence từ transcript (lecture number).

### Bước 3: Write 5 sections (~20 phút)
```
## 1. Mode Objective
## 2. Top Mistake Traps (≥5, với evidence từ lectures)
## 3. Expert Gap Analysis (gì course không dạy)
## 4. Deliberate Practice Design
## 5. Red Flags in Course
```

### Verify
- [ ] Section 2: ≥5 traps với lecture number evidence
- [ ] Section 3: ít nhất 5 gaps (real production TS skills not in course)
- [ ] Section 5: specific examples từ course content

## Acceptance Criteria
- **Given** `expert/_mode-rationale.md` tồn tại
- **When** D1 (learning plan) build expert curriculum
- **Then** tất cả 5 sections có nội dung, top mistake traps có ≥5 entries với evidence

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/expert/_mode-rationale.md`
- [ ] Section 2: ≥5 traps, mỗi trap có lecture evidence (số lecture)
- [ ] Section 3: ≥5 expert gaps với cụ thể (không phải "advanced topics")
- [ ] Section 5: ít nhất 3 red flags cụ thể từ course

## Dependencies
- typescript-41s — A6 learning rationale phải có trước (expert gap analysis)

## Worklog
*(Chưa bắt đầu)*
