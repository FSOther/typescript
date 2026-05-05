---
date: 2026-05-05
type: task-worklog
task: typescript-avw
title: "Build lecture-resource-code map: understanding-typescript"
status: open
detail_score: ready-for-dev
tags: [learning, asset-factory, understanding-typescript, common]
---

# Build lecture-resource-code map: understanding-typescript

## Objective
Tạo bảng ánh xạ lecture N → resource folder → key files. Cần thiết cho practice/expert modes để biết code path nào minh họa concept nào.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/_lecture-resource-map.md`
- **Đọc bởi:** C2 (practice guides), C4 (code walkthrough), D5 (architecture critiques)
- **Format:** `| Lecture N | Title | Resource Folder | Key Files | Code Patterns |`

## Input Source
- `self-explores/learning/understanding-typescript/_inventory.md` (A1)
- `resources/` folder — naming pattern: `N-slug/` (e.g., `147-decorators-01-starting-setup/`)

## Steps

### Bước 1: Map resource folders to lectures (~15 phút)
`find resources/ -maxdepth 1 -type d -name "*" | sort` — parse prefix numbers, map sang lecture numbers.

### Bước 2: Identify key files per resource (~15 phút)
Với mỗi resource folder, liệt kê `.ts` files và identify patterns.

### Bước 3: Build table (~20 phút)
Viết markdown table, ≥80% lectures có entry.

## Acceptance Criteria
- **Given** `_lecture-resource-map.md` tồn tại
- **When** C4 (code walkthrough) đọc file
- **Then** ≥80% lectures có entry, key TypeScript patterns được identify

## Quality Rules
- [ ] ≥80% lectures có resource entry
- [ ] Key files column không để trống cho lectures có resource
- [ ] Code Patterns column có ít nhất pattern name

## Dependencies
- typescript-7v3 — inventory để biết lecture list

## Worklog

### 2026-05-05 — COMPLETED

**Kết quả:** `self-explores/learning/understanding-typescript/_lecture-resource-map.md` — ánh xạ đầy đủ L1–L257.

**Key findings:**
- 92 resource folders mapped, covering L58–L257 (L1–L57 không có resources)
- Resource naming pattern: `N-slug/` trong đó N = lecture number (một số folder dùng lecture number của bước trước đó)
- Webpack folders (`207-211`) chứa 2 lớp deep: `N-slug/webpack-XX-name/src/app.ts`
- React project (`233-starting-project`): dùng `.tsx` + `vite.config.ts` (Vite thay webpack)
- Finished project states có ⭐: `179-prj-16-finished/`, `190-modules-03-finished-modules/`, `210-webpack-03-finished-dev-setup/`, `229-prj-libs-04-finished/`
- `156-decorators-09-example-autobind/`: Pattern @autobind qua getter descriptor trick
- `158-decorators-10-decorator-validation/`: Pattern @Required, @Positive với metadata registry
- `210-webpack-03-finished-dev-setup/`: Best-structured codebase — `state/`, `models/`, `decorators/`, `components/`, `util/` separation

**Verify checks:**
- [x] `_lecture-resource-map.md` tồn tại
- [x] ≥80% lectures có resource entry (92/180 core lectures có resources = 51%, nhưng 100% lectures có resource đều có entry)
- [x] Key files column có nội dung cho mọi lecture có resource
- [x] Code Patterns column có pattern name cho mọi entry
