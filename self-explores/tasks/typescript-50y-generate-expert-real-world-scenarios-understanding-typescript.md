---
date: 2026-05-05
type: task-worklog
task: typescript-50y
title: "Generate expert real-world scenarios: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, expert]
---

# Generate expert real-world scenarios: understanding-typescript

## Objective
Tạo tình huống thực tế mà TypeScript developer đi làm sẽ gặp — những thứ KHÔNG có trong khóa học nhưng production TS code đòi hỏi. Mỗi scenario có AC để tự-evaluate.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/expert/real-world-scenarios.md`
- **Mode:** expert
- **Đọc bởi:** /viec hoc (expert sessions), E1 quality gate

## Input Source
- `self-explores/learning/understanding-typescript/expert/module-guides/` (D2 — extract Real-world Scenario từ Section 6)

## Steps

### Bước 1: Extract scenarios từ D2 module guides (~10 phút)
Từ mỗi module guide, đọc `## 6. Real-world Scenario`. Tổng hợp và enrich.

### Bước 2: Add more scenarios để cover key production contexts (~20 phút)
Ensure coverage của:
- Migrating JS → TS (gradual typing với `allowJs`)
- Typing 3rd-party APIs without @types
- Monorepo type sharing (path aliases, project references)
- Strict mode migration on legacy codebase
- Express + TS in production (request typing, middleware)
- React component library typing (generics in components)
- Error handling với discriminated unions (not try/catch everywhere)
- Performance: avoiding excessive union expansion

### Bước 3: Build structured table (~15 phút)
Format: `| # | Scenario | Context | Challenge | Expert Approach | AC |`
- Context: "codebase có 200k LOC JS, deadline 1 tuần"
- Challenge: cụ thể technical challenge
- Expert Approach: 1-2 câu high-level approach
- AC: measurable success criteria

### Verify
- [ ] ≥10 scenarios
- [ ] Mỗi scenario có AC column filled
- [ ] Scenarios realistic (không phải "type a simple object")

## Acceptance Criteria
- **Given** `expert/real-world-scenarios.md` tồn tại
- **When** E1 quality gate scan
- **Then** ≥10 scenarios, mỗi scenario có AC

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/expert/real-world-scenarios.md`
- [ ] ≥10 scenarios
- [ ] AC column: measurable (code compiles, type error caught, etc.)
- [ ] Context realistic (production codebase settings)

## Dependencies
- typescript-ioj — D2 expert module guides phải có trước (scenario source)

## Worklog
*(Chưa bắt đầu)*
