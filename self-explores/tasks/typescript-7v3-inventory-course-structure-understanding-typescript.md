---
date: 2026-05-05
type: task-worklog
task: typescript-7v3
title: "Inventory course structure: understanding-typescript"
status: open
detail_score: ready-for-dev
tags: [learning, asset-factory, understanding-typescript, common]
---

# Inventory course structure: understanding-typescript

## Objective
Quét toàn bộ cấu trúc course để tạo inventory — nền tảng cho tất cả assets sau. Không có file này thì không task nào khác có thể chạy đúng.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/_inventory.md`
- **Đọc bởi:** A2, A3, A4, A5 và tất cả mode tasks
- **Format:** 5 sections: Lecture list, Section map, Resource list, Code repos, Duration estimate

## Input Source
- `lectures.txt` — TOC, 339 lectures
- `lectures/` — transcript files
- `resources/` — 276 resource folders

## Steps

### Bước 1: Đọc TOC (~5 phút)
Đọc `lectures.txt` (339 lines). Parse section breaks và lecture groupings.

### Bước 2: Scan resources/ (~5 phút)
`find resources/ -maxdepth 1 -type d | sort` — liệt kê tất cả resource folders với naming pattern.

### Bước 3: Build section map (~10 phút)
Map lecture ranges → section names (dựa theo TOC groupings và naming patterns).

### Bước 4: Write _inventory.md (~10 phút)
Viết 5 sections với data thu thập được.

## Acceptance Criteria
- **Given** `_inventory.md` tồn tại tại đúng path
- **When** các tasks sau (A2, A3) đọc file
- **Then** tất cả 5 sections có nội dung, lecture count = 339

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/_inventory.md`
- [ ] Lecture list đầy đủ 339 lectures
- [ ] Section map có ≥10 sections
- [ ] Resource list có 276 entries

## Dependencies
Không có dependency — task đầu tiên.

## Worklog

### 2026-05-05 — COMPLETED

**Kết quả:** `self-explores/learning/understanding-typescript/_inventory.md` — 5 sections đầy đủ.

**Phát hiện quan trọng:**
- `ls resources/ | wc -l` = 276 items (92 folders + ~92 .zip + ~92 Zone.Identifier) — khớp với spec
- Actual code folders: 92 (không phải 276)
- Resources range: L3–L322, không có cho L1–L57 (ngoại trừ L3 demo-app và L58–L66)
- Section count: 20 modules (M00–M19) + LEGACY block
- LEGACY = L258–L339 (82 lectures): re-recordings của M01, M02, M05, M07 — SKIP hoàn toàn
- Key finished projects: `179-prj-16-finished/`, `190-modules-03-finished-modules/`, `210-webpack-03-finished-dev-setup/`, `229-prj-libs-04-finished/`

**Verify checks:**
- [x] File path đúng
- [x] Lecture count = 339
- [x] Section map có 20 modules + LEGACY
- [x] Resource list: 92 folders listed, 276 items count explained
- [x] Duration estimate có 5 scenarios
