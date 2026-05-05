---
date: 2026-05-05
type: task-worklog
task: typescript-cue
title: "Generate architecture critiques: understanding-typescript"
status: open
detail_score: ready-for-dev
detailed_at: 2026-05-05
tags: [learning, asset-factory, understanding-typescript, expert]
---

# Generate architecture critiques: understanding-typescript

## Objective
Phân tích code examples trong resources/ theo tiêu chuẩn production-grade TypeScript. 3 tables: Strengths (đáng học theo), Weaknesses (cần tránh), Missing (pros làm khác). Giúp user thấy gap giữa tutorial code và production code.

## Asset Output
- **File path:** `self-explores/learning/understanding-typescript/expert/architecture-critiques.md`
- **Mode:** expert
- **Đọc bởi:** /viec hoc (expert sessions), E1 quality gate

## Input Source
- `self-explores/learning/understanding-typescript/_lecture-resource-map.md` (A3 — code paths)
- `resources/179-prj-16-finished/` — DnD project
- `resources/184-prj-16-finished/` — alternative DnD
- `self-explores/learning/understanding-typescript/expert/learning-plan.md` (D1 — expert red flags)

## Steps

### Bước 1: List key .ts files (~5 phút)
```bash
find resources/179-prj-16-finished/ resources/184-prj-16-finished/ -name "*.ts" | head -20
cat resources/179-prj-16-finished/src/app.ts | head -50
```

### Bước 2: Read và annotate code (~30 phút)
Đọc kỹ app.ts và các files chính. Với mỗi pattern, đánh giá:
- Production quality? Expert would do differently?
- Any `any`, type assertions, missing error handling?
- Architectural patterns: inheritance vs composition, single responsibility?

### Bước 3: Write 3 tables (~20 phút)
```markdown
## Strengths (pattern tốt, đáng học)
| Pattern | File | Why Good |

## Weaknesses/Anti-patterns
| Pattern | File | Why Problematic | Production Alternative |

## Missing (gaps vs production TS)
| Missing Concern | Why Pros Care | How to Address |
```
≥3 entries per table.

### Verify
- [ ] Mỗi table ≥3 entries với actual file paths
- [ ] Weaknesses có "Production Alternative" column filled
- [ ] Missing table explains "Why Pros Care" (not just "add error handling")

## Acceptance Criteria
- **Given** `expert/architecture-critiques.md` tồn tại
- **When** expert mode session review code patterns
- **Then** cả 3 tables có nội dung, ≥3 strengths và ≥3 weaknesses với file references

## Quality Rules
- [ ] File path đúng: `self-explores/learning/understanding-typescript/expert/architecture-critiques.md`
- [ ] 3 tables đều có ≥3 entries
- [ ] Mỗi entry có actual resources/ file path
- [ ] Weaknesses: có actionable alternative (không chỉ "bad")

## Dependencies
- typescript-avw — A3 lecture-resource map phải có trước (code paths)
- typescript-iun — D1 expert learning plan phải có trước (red flags context)

## Worklog
*(Chưa bắt đầu)*
