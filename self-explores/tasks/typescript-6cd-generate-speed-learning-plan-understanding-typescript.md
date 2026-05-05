---
date: 2026-05-05
type: task-worklog
task: typescript-6cd
title: "Generate speed learning plan: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, speed]
---

# Generate speed learning plan: understanding-typescript

## Objective
Tạo session-by-session learning plan cho speed mode — ai đọc file này biết ngay học lecture nào trong session nào, mất bao lâu, focus vào gì. File này là backbone của speed mode execution.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/speed/learning-plan.md`
- **Mode:** speed
- **Đọc bởi:** /viec hoc (speed sessions), B3 (module guides cần biết module grouping), B6 (study-progress cần session list)

## Input Source
- `self-explores/learning/understanding-typescript/speed/_mode-rationale.md` (B0 — 80/20 analysis, skip list)
- `self-explores/learning/understanding-typescript/_learning-index.md` (A5)
- `lectures.txt` — TOC cho lecture titles

## Steps

### Bước 1: Đọc B0 rationale và A5 indexes (~5 phút)
Extract: core lecture ranges, skip list, pacing strategy (minutes/session).

### Bước 2: Tạo session breakdown (~20 phút)
Với pacing từ B0 (e.g., 20-25 lectures/session):
- Session 1: L13-L39 (~27 lectures, TypeSystem) ~2h
- Session 2: L50-L68 (~19 lectures, Modern JS+TS) ~1.5h
- ... continue until L257 (skip L258-L339)
Mỗi session: lecture range, focus area, ước tính thời gian.

### Bước 3: Build lecture matrix (~20 phút)
Bảng `| Lecture N | Title | Priority | Notes |`
- Priority: core / optional / skip
- Ghi rõ lecture numbers từ skip list (B0)

### Bước 4: Write milestones + active recall strategy (~10 phút)
- Milestones: sau session N = có thể làm được gì
- Active recall: method cụ thể (e.g., "5 câu từ active-recall-bank.md")

### Verify
- [ ] Session breakdown có ≥4 sessions, mỗi session có time estimate
- [ ] Lecture matrix cover 100% lectures L13-L257 (kể cả skip entries)
- [ ] Skip list nhất quán với B0 rationale

## Acceptance Criteria
- **Given** `speed/learning-plan.md` tồn tại
- **When** /viec hoc (speed mode) đọc file để plan session
- **Then** session breakdown cover 100% lectures (kể cả skip), lecture matrix có priority cho ≥80% lectures L13-L257

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/speed/learning-plan.md`
- [ ] Session breakdown: ≥4 sessions, mỗi có lecture range + time estimate
- [ ] Lecture matrix: ≥80% lectures có priority (core/optional/skip)
- [ ] Skip entries phải consistent với B0 rationale

## Dependencies
- typescript-cms — B0 speed rationale phải có trước (pacing, 80/20)
- typescript-yxl — A5 indexes phải initialized

## Worklog
*(Chưa bắt đầu)*
