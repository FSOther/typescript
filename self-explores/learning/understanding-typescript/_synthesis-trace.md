---
date: 2026-05-06
type: synthesis-trace
course_slug: understanding-typescript
created: 2026-05-06
generated_by: typescript-1ti
---

# Synthesis Trace — Understanding TypeScript

This file tracks where each learning asset came from — which task created it, which inputs were used, and the key reasoning decision. Used by `/viec hoc why` and `/viec hoc trace` commands.

---

## Transformation Pipeline

The full asset production sequence for this course:

```
/viec samples 8
└── Group A (Common Assets)
    ├── A1: typescript-7v3  → _inventory.md
    │   Input: lectures.txt, resources/ folder structure
    ├── A2: typescript-ctu  → _course-profile.md
    │   Input: _inventory.md + sampled lecture transcripts
    ├── A3: typescript-avw  → _lecture-resource-map.md
    │   Input: _inventory.md + resources/ .ts file scan
    ├── A4: typescript-o05  → _knowledge-map.md
    │   Input: _course-profile.md + _inventory.md
    ├── A5: typescript-yxl  → _learning-index.md + _mode-index.md
    │   Input: all A1-A4 assets
    ├── A6: typescript-41s  → _learning-rationale.md
    │   Input: all A1-A5 assets (synthesis of all prior)
    ├── A7: typescript-1ti  → _synthesis-trace.md (this file)
    │   Input: all A1-A6 assets
    └── A8: typescript-76l  → _retrieval-index.md
        Input: all A1-A7 assets

└── Group B (Speed Mode)
    ├── B0: typescript-cms  → speed/_mode-rationale.md
    │   Input: _learning-rationale.md + _knowledge-map.md
    ├── B1: typescript-6cd  → speed/learning-plan.md
    │   Input: _course-profile.md + speed/_mode-rationale.md
    ├── B2: typescript-bz0  → speed/module-guides/*.md
    │   Input: speed/learning-plan.md + lecture transcripts + _lecture-resource-map.md
    ├── B3: typescript-lqz  → speed/active-recall-bank.md
    │   Input: speed/module-guides/
    ├── B4: typescript-jq9  → speed/cheatsheet.md (high-speed-cheatsheet.md)
    │   Input: speed/module-guides/ + _knowledge-map.md
    └── B5: typescript-3fs  → speed/study-progress.md
        Input: speed/learning-plan.md (extract sessions table)

└── Group C (Practice Mode)
    ├── C0: typescript-6uv  → practice/_mode-rationale.md
    ├── C1: typescript-7h6  → practice/learning-plan.md
    ├── C2: typescript-ygy  → practice/module-guides/*.md
    ├── C3: typescript-a1v  → practice/practice-challenges.md
    ├── C4: typescript-g3b  → practice/code-walkthrough.md
    ├── C5: typescript-tro  → practice/study-progress.md
    ├── C6: typescript-bgz  → practice/_practice-mastery-path.md
    ├── C7: typescript-o7e  → practice/final-capstone.md
    └── C8: typescript-3b1  → practice/practice-coverage-matrix.md

└── Group D (Expert Mode)
    ├── D0: typescript-ep0  → expert/_mode-rationale.md
    ├── D1: typescript-iun  → expert/learning-plan.md
    ├── D2: typescript-ioj  → expert/module-guides/*.md
    ├── D3: typescript-qcy  → expert/mistake-traps.md
    ├── D4: typescript-n2s  → expert/expert-drills.md
    ├── D5: typescript-cue  → expert/architecture-critiques.md
    ├── D6: typescript-50y  → expert/real-world-scenarios.md
    └── D7: typescript-pb7  → expert/study-progress.md

└── Group E (Quality Gate)
    └── E1: typescript-fqc  → speed/_generation-report.md
                             + practice/_generation-report.md
                             + expert/_generation-report.md

└── Ready for /viec hoc
```

---

## Final Assets

Assets created by tasks, with inputs and key reasoning. Auto-appended by each task when it closes (via `/viec xong` Learning Asset Trace Update protocol).

| Asset | Created By Task | Inputs Used | Key Reasoning | Quality Status |
|-------|----------------|-------------|---------------|----------------|
| `_inventory.md` | typescript-7v3 | `lectures.txt`, `resources/` dir scan | 276 items = 92 folders + ~92 .zip + ~92 Zone.Identifier; LEGACY = L258–L339 | ready |
| `_course-profile.md` | typescript-ctu | `_inventory.md`, sampled lecture transcripts (L15, L68, L101, L136, L163) | Teaching style = error-driven + WHY before HOW; User Fit = Excellent; M15 Webpack outdated → Vite | ready |
| `_lecture-resource-map.md` | typescript-avw | `_inventory.md`, `resources/` .ts file scan | 92 resource folders mapped; webpack folders 2-deep; React uses .tsx + Vite; ⭐ finished project states | ready |
| `_knowledge-map.md` | typescript-o05 | `_course-profile.md`, `_inventory.md` | 7 concept layers, dependency graph (L1→L2→L3→L4→L5→L7, L6 parallel), 5 milestones, 25 code patterns | ready |
| `_learning-index.md` | typescript-yxl | all A1–A4 assets | active_course=understanding-typescript; A1–A5 checked; 3 modes = missing | ready |
| `_mode-index.md` | typescript-yxl | `_knowledge-map.md`, mode structure from skill spec | Mode status table with missing/draft/ready/stale/regenerating legend | ready |
| `_learning-rationale.md` | typescript-41s | all A1–A5 assets | 7 sections: Course Understanding, User Fit, Learning Order, Mode Design, Skip table (34 rows), Expert Gaps (13), Open Questions (5) | ready |
| `_synthesis-trace.md` | typescript-1ti | all A1–A6 assets | This file — audit trail for asset provenance | ready |

*Rows below will be auto-appended as Group B/C/D/E tasks complete:*

---

## Evidence Map

Key claims made during asset synthesis, with their sources and confidence levels.

| Claim / Decision | Evidence Source | Confidence |
|-----------------|----------------|-----------|
| Teaching style = error-driven, explains WHY before HOW | Sampled transcripts: L15 (type inference), L68 (class intro), L101 (generics intro) | High |
| User fit = Excellent for "biết JS, mới TS, goal = build project" | Course profile analysis: no JS review needed, starts TS at L13, DnD project = production patterns | High |
| L258–L339 = LEGACY, skip entirely | Inventory scan: section headers show re-recordings of M01, M02, M05, M07 | High |
| M15 Webpack (L204–L213) is outdated | Course profile + resource scan: `resources/195-starting-project/` uses Vite; webpack resources exist but are older | High |
| Speed mode should be first (before Practice/Expert) | Learning rationale Section 2: user has JS background → 80/20 covers critical TS in 15-18h without waste | High |
| DnD project (M12) = centerpiece of Practice mode | 6+ patterns applied simultaneously: abstract base class, singleton, observer, @autobind, generics, HTML5 DnD | High |
| 13 expert gaps identified | Course content analysis: testing, monorepo, module augmentation, declaration merging not in any lecture | High |
| L50–L57 skip — duplicates M19 | Course profile Section 5: Node.js workflow covered better in M19 with full TS typing context | Medium |
| L110–L117 skip in Speed mode | Learning rationale: Generic linked list is CS exercise, M07 already covers generics | Medium |
| `satisfies` keyword = modern TS pattern | L99 title: "The satisfies Keyword" — added 2022, widely used in modern TS | High |
| Vite preferred over Webpack | Resource scan: `233-starting-project/` uses `vite.config.ts`; course profile recommends M14 over M15 | High |

---

## Gaps

Known uncertainties remaining after asset synthesis. Used by `/viec hoc why` to surface limitations.

| Topic | Status | What's Uncertain | Next Action |
|-------|--------|-----------------|-------------|
| Context API typing in M18 | Open | L238–L240 range not sampled — may or may not cover `useContext<T>` | Read transcripts during learning |
| Async error handling in M19 | Open | `Result<T, E>` pattern not found in sampled lectures | Check L250–L255 transcripts |
| NestJS decorator patterns in M10 | Open | ECMAScript decorators covered, but NestJS-specific @Injectable/@Controller analogues unknown | Check L133–L145 |
| tsconfig `paths` coverage | Open | M02 covers 9 lectures but path alias config not confirmed | Check L41–L49 |
| `satisfies` depth (L99) | Open | 1 lecture may be just intro vs deep dive | Read L99 transcript |

---

## Reset Events

*(Empty — will be populated if user resets learning progress)*

| Time | Mode | Archive | Reason |
|------|------|---------|--------|

---

## Overwrite Events

*(Empty — will be populated if user overwrites mode assets)*

| Time | Mode | Level | Archive | Reason |
|------|------|-------|---------|--------|

| speed/_mode-rationale.md | typescript-cms | _learning-rationale.md sections 4,5 | sections 4+5 directly mapped to 5 required sections | ready |

| expert/_mode-rationale.md | typescript-ep0 | _learning-rationale.md sections 2,3,4,6 | 8 mistake traps + deliberate practice design + red flags synthesis | ready |
| practice/_mode-rationale.md | typescript-6uv | _learning-rationale.md section 4 + resources/179-prj-16-finished/ code analysis | Build→Break→Fix rationale + 8 actual resource paths + 5 design principles | ready |
