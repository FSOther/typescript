---
date: 2026-05-05
type: task-worklog
task: typescript-iun
title: "Generate expert learning plan: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, expert]
---

# Generate expert learning plan: understanding-typescript

## Objective
Tạo learning plan expert mode với deliberate practice focus. Không phải session schedule — là curriculum với expert mindset framing cho mỗi module. D2 (module guides), D7 (study-progress) đọc file này.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/expert/learning-plan.md`
- **Mode:** expert
- **Đọc bởi:** /viec hoc (expert mode), D2 module guides, D7 study-progress

## Input Source
- `self-explores/learning/understanding-typescript/expert/_mode-rationale.md` (D0)
- `self-explores/learning/understanding-typescript/_learning-index.md` (A5)
- `self-explores/learning/understanding-typescript/_knowledge-map.md` (A4 — concept relationships)

## Steps

### Bước 1: Đọc D0 rationale + A4 knowledge map (~10 phút)
Extract: top mistake traps (per module), expert gaps, module grouping.

### Bước 2: Design expert curriculum (~25 phút)
Per module (M01-M13), xác định:
- Core mental model (expert nghĩ gì về module này)
- Top mistake (từ D0 traps)
- Expert drill (1 deliberate practice activity)
- Red flags (patterns người mới dùng sai)

### Bước 3: Write 5 sections (~15 phút)
```
## 1. Expert Mindset Overview
## 2. Module Plan (module N: mental model, top mistake, drill)
## 3. Expert Red Flags
## 4. Lecture Matrix (lecture N | importance | expert notes)
## 5. Deliberate Practice Schedule
```

### Verify
- [ ] Module Plan: tất cả ≥8 modules có entry
- [ ] Lecture Matrix: ≥core lectures (từ B0 80/20) có expert notes
- [ ] Expert Red Flags: ≥5 concrete anti-patterns

## Acceptance Criteria
- **Given** `expert/learning-plan.md` tồn tại
- **When** D2 (module guides) dùng plan để build content
- **Then** plan có expert red flags section, lecture matrix có expert notes cho core lectures

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/expert/learning-plan.md`
- [ ] Section 2: tất cả ≥8 modules có entry với 3 sub-sections
- [ ] Section 3: ≥5 expert red flags (production TS anti-patterns)
- [ ] Section 4: ≥core lectures có expert annotation

## Dependencies
- typescript-ep0 — D0 expert rationale phải có trước (mistake traps, red flags)
- typescript-yxl — A5 indexes phải initialized

## Worklog
*(Chưa bắt đầu)*
