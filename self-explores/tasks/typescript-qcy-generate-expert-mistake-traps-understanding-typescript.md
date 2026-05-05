---
date: 2026-05-05
type: task-worklog
task: typescript-qcy
title: "Generate expert mistake traps: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, expert]
---

# Generate expert mistake traps: understanding-typescript

## Objective
Tổng hợp tất cả TypeScript bẫy kỹ thuật thường gặp vào 1 file có cấu trúc. Mỗi trap có drill. Focus vào TypeScript-specific — không phải JavaScript mistakes.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/expert/mistake-traps.md`
- **Mode:** expert
- **Đọc bởi:** /viec hoc (expert sessions), E1 quality gate

## Input Source
- `self-explores/learning/understanding-typescript/expert/module-guides/` (D2 — extract Mistake Traps từ Section 4)

## Steps

### Bước 1: List all expert module guide files (~2 phút)
```bash
ls self-explores/learning/understanding-typescript/expert/module-guides/*.md
```

### Bước 2: Extract all traps từ Section 4 (~15 phút)
Từ mỗi module guide, đọc `## 4. Mistake Traps`. Tổng hợp danh sách.

### Bước 3: Build table + add drills (~20 phút)
Format: `| Trap | Who Makes It | Why | Expert Avoidance | Drill |`
- Trap: tên ngắn gọn
- Who Makes It: "beginners", "JS devs new to TS", "anyone under pressure"
- Why: tại sao dễ mắc
- Expert Avoidance: 1 câu ngắn
- Drill: bài tập buộc nhận ra trap

TypeScript-specific traps phải cover:
- type assertion misuse (as any / as SomeType)
- structural typing gotchas (wrong object shape accepted)
- optional chaining → type narrowing bypassed
- enum vs const enum runtime behavior
- decorator execution order

### Bước 4: Add summary section (~5 phút)
"Top 3 traps người mới nhất hay mắc" — highlight với explanation tại sao nguy hiểm.

### Verify
- [ ] ≥10 traps trong table
- [ ] Mỗi trap có drill column filled
- [ ] Tất cả traps là TypeScript-specific (không phải JS gotchas)

## Acceptance Criteria
- **Given** `expert/mistake-traps.md` tồn tại
- **When** E1 quality gate scan
- **Then** ≥10 traps, mỗi trap có drill

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/expert/mistake-traps.md`
- [ ] ≥10 traps, tất cả TypeScript-specific
- [ ] Drill column không để trống
- [ ] Summary section tồn tại

## Dependencies
- typescript-ioj — D2 expert module guides phải có trước (source của traps)

## Worklog
*(Chưa bắt đầu)*
