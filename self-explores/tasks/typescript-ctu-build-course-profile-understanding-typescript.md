---
date: 2026-05-05
type: task-worklog
task: typescript-ctu
title: "Build course profile: understanding-typescript"
status: open
detail_score: ready-for-dev
tags: [learning, asset-factory, understanding-typescript, common]
---

# Build course profile: understanding-typescript

## Objective
Tạo hồ sơ đầy đủ của course để AI sessions sau hiểu context khi tạo module guides và learning plans.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/_course-profile.md`
- **Đọc bởi:** `/viec hoc` — context initialization, A4 knowledge map
- **Format:** 7 sections

## Input Source
- `self-explores/learning/understanding-typescript/_inventory.md` (A1)
- `lectures.txt` — TOC
- Sample lectures từ `lectures/` để gauge style

## Steps

### Bước 1: Đọc inventory (~3 phút)
Đọc `_inventory.md` để có section map và lecture count.

### Bước 2: Sample lectures (~10 phút)
Đọc 3-5 lectures từ các modules khác nhau để đánh giá teaching style và depth.

### Bước 3: Write profile (~15 phút)
Viết 7 sections với user profile context: biết JS, mới học TS, goal = build project thực.

## Acceptance Criteria
- **Given** `_course-profile.md` tồn tại
- **When** A4 (knowledge map) đọc file
- **Then** User Fit Analysis có kết luận rõ ràng cho profile "biết JS, mới TS"

## Quality Rules
- [ ] Tất cả 7 sections có nội dung
- [ ] Recommended Mode có lý do (không chỉ "speed")
- [ ] Skip Strategy có lecture numbers cụ thể

## Dependencies
- typescript-7v3 — inventory phải có trước

## Worklog

### 2026-05-05 — COMPLETED

**Kết quả:** `self-explores/learning/understanding-typescript/_course-profile.md` — 7 sections đầy đủ.

**Key findings:**
- Teaching style: explicit, error-driven, explains WHY before HOW, code-first
- Fit verdict: Excellent cho profile "biết JS, mới TS, goal = build project"
- Recommended mode: Speed first → Practice second (Expert optional)
- Lecture L234 có link GitHub resource — nhưng actual code trong resources/233-starting-project/ local
- M15 Webpack (L204-L213): outdated — dùng Vite (M14) thay thế
- M17 Google Maps: optional (cần API key bên ngoài)
- Skip list confirmed: L1-L9, L50-L57, L110-L117, L204-L213, L224-L231, L258-L339

**Verify checks:**
- [x] 7 sections có nội dung
- [x] Section 3 (User Fit Analysis) có verdict rõ ràng cho profile "biết JS, mới TS"
- [x] Section 4 (Recommended Mode) có lý do cụ thể (không chỉ "speed")
- [x] Section 5 (Skip Strategy) có lecture numbers cụ thể
- [x] Section 6 (Outcomes) có ≥10 concrete capabilities
