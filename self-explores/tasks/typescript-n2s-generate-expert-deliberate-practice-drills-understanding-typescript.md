---
date: 2026-05-05
type: task-worklog
task: typescript-n2s
title: "Generate expert deliberate practice drills: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, expert]
---

# Generate expert deliberate practice drills: understanding-typescript

## Objective
Tạo danh sách structured deliberate practice drills cho expert mode — target TypeScript-specific expert skills. Mỗi drill có output requirement cụ thể và expert criteria để tự-judge.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/expert/expert-drills.md`
- **Mode:** expert
- **Đọc bởi:** /viec hoc (expert sessions), E1 quality gate

## Input Source
- `self-explores/learning/understanding-typescript/expert/module-guides/` (D2 — extract Deliberate Drill từ Section 7)

## Steps

### Bước 1: Extract drills từ D2 module guides (~15 phút)
Từ mỗi module guide, đọc `## 7. Deliberate Drill`. Tổng hợp, enrich với metadata.

### Bước 2: Build structured table (~20 phút)
Format: `| Drill ID | Skill Trained | Difficulty | Output Required | Expert Criteria |`
- Drill ID: D{module_N}-{a|b} (e.g., D02-a)
- Skill Trained: cụ thể (e.g., "conditional type inference" không phải "generics")
- Difficulty: intermediate/advanced/expert
- Output Required: cụ thể (e.g., "working TypeScript code that compiles with strict mode")
- Expert Criteria: measurable pass/fail

Expert skills phải cover (từ D2):
- conditional types + `infer`
- mapped types với modifiers
- discriminated unions + exhaustive check
- type narrowing với custom guards
- declaration file (.d.ts) authoring
- performance-aware typing (avoid large union expansions)

### Bước 3: Add grouping by skill category (~5 phút)
Group drills: Type System | Advanced Types | Runtime Safety | Architecture

### Verify
- [ ] ≥15 drills
- [ ] Cover ≥80% modules
- [ ] Expert Criteria column: measurable, không phải "good code"

## Acceptance Criteria
- **Given** `expert/expert-drills.md` tồn tại
- **When** E1 quality gate scan
- **Then** ≥15 drills, cover ≥80% modules

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/expert/expert-drills.md`
- [ ] ≥15 drills với metadata đầy đủ
- [ ] Expert Criteria: measurable (compiler output, specific behavior, not "elegant")
- [ ] Skill Trained: specific (not "generics" but "conditional type inference")

## Dependencies
- typescript-ioj — D2 expert module guides phải có trước (drill source)

## Worklog
*(Chưa bắt đầu)*
