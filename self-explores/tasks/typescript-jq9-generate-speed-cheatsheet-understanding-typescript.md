---
date: 2026-05-05
type: task-worklog
task: typescript-jq9
title: "Generate speed cheatsheet: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, speed]
---

# Generate speed cheatsheet: understanding-typescript

## Objective
Tạo cheatsheet 1 trang reference cho toàn bộ course — compact, printable, syntax + short examples. Không phải summary dài — là lookup reference khi code.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/speed/cheatsheet.md`
- **Mode:** speed
- **Đọc bởi:** /viec hoc (speed mode quick-reference), user khi code

## Input Source
- `self-explores/learning/understanding-typescript/speed/module-guides/` (B3 — extract Quick Reference sections)
- `lectures/` folder — verify syntax accuracy nếu cần

## Steps

### Bước 1: Collect Quick Reference sections từ module guides (~10 phút)
Đọc `## 5. Quick Reference` của mỗi module guide. Tổng hợp theo category.

### Bước 2: Organize 6 sections (~15 phút)
```markdown
## Type System (types, inference, generics)
## Classes & Interfaces
## Advanced Types & Utilities
## Decorators
## Module System
## Common Patterns
```
Mỗi section: compact, syntax-focused, ví dụ ngắn (≤5 lines code).

### Bước 3: Verify syntax accuracy (~10 phút)
Cross-check các patterns quan trọng với transcript (e.g., conditional types, mapped types). Không copy-paste từ module guides mà không verify.

### Verify
- [ ] 6 sections đều có nội dung
- [ ] Code snippets valid TypeScript (không phải pseudocode)
- [ ] Mỗi section compact — không có đoạn text giải thích dài

## Acceptance Criteria
- **Given** `speed/cheatsheet.md` tồn tại
- **When** user cần tra syntax TypeScript nhanh
- **Then** tất cả 6 sections có nội dung, có code snippets cho ≥5 patterns quan trọng nhất

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/speed/cheatsheet.md`
- [ ] 6 sections có nội dung
- [ ] Code snippets compilable TypeScript (không phải pseudocode)
- [ ] Không có đoạn giải thích >3 dòng (cheatsheet phải compact)

## Dependencies
- typescript-bz0 — B3 module guides phải có trước (source cho Quick Reference sections)

## Worklog
*(Chưa bắt đầu)*
