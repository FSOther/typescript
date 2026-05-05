---
type: learning-plan
mode: speed
course_slug: understanding-typescript
created: 2026-05-06
generated_by: typescript-6cd
---

# Speed Learning Plan — Understanding TypeScript

**Course:** Understanding TypeScript (Maximilian Schwarzmüller)
**Mode:** Speed (80/20 — read + recall, no code building)
**Estimated time:** 15–18 hours over 14 sessions
**Prerequisite:** JavaScript proficiency (ES6+: arrow functions, classes, async/await, destructuring)

---

## 1. Overview

Speed mode covers **165 of 339 lectures** — those that deliver TS type-system knowledge needed to read and write TypeScript in React and Node.js projects. 174 lectures are explicitly skipped (admin, legacy, outdated tools, CS exercises).

**Mode output:** After completing all sessions, user can:
1. Annotate TypeScript functions, classes, and interfaces without IDE prompts
2. Read generic type signatures in React and Express source code
3. Configure `tsconfig.json` with `strict: true` from memory
4. Explain TypeScript's type narrowing, discriminated unions, and decorator patterns
5. Debug type errors from compiler messages alone

**Progress tracking:** `study-progress.md` tracks each lecture with status (todo/awaiting_user/done/skip). Progress % = done lectures / study lectures (not counting skip).

---

## 2. Session Breakdown

14 sessions × ~55 minutes average. Each session = 40 min active study + 15 min Active Recall.

| # | Session | Lectures | Module | Study Type | Est. Minutes |
|---|---------|----------|--------|------------|-------------|
| S01 | Types Foundation (Part 1) | L13–L26 | M01 | DEEP_DIVE | 55 |
| S02 | Types Foundation (Part 2) | L27–L39 | M01 | DEEP_DIVE | 55 |
| S03 | tsconfig Essentials | L41–L49 | M02 | STUDY | 45 |
| S04 | Classes & Interfaces (Part 1) | L67–L76 | M05 | DEEP_DIVE | 55 |
| S05 | Classes & Interfaces (Part 2) | L77–L87 | M05 | DEEP_DIVE | 50 |
| S06 | Advanced Types | L88–L99 | M06 | STUDY | 55 |
| S07 | Generics | L100–L109 | M07 | STUDY | 50 |
| S08 | Type Utilities & Mapped Types | L118–L132 | M09 | STUDY | 60 |
| S09 | ECMAScript Decorators | L133–L145 | M10 | STUDY | 55 |
| S10 | Experimental Decorators | L146–L160 | M11 | STUDY | 55 |
| S11 | Modules & Build Tools | L182–L198 | M13+M14 | STUDY | 60 |
| S12 | @types, .d.ts, Zod | L214–L223 | M16 | STUDY | 45 |
| S13 | React + TypeScript | L232–L245 | M18 | STUDY | 65 |
| S14 | Node.js + TypeScript | L246–L257 | M19 | STUDY | 55 |

**Total: ~770 min active time + ~210 min Active Recall = ~980 min (~16 hrs)**

---

## 3. Lecture Matrix

Full coverage of L1–L257 with speed mode action. All L258–L339 are SKIP (legacy).

| Lectures | Count | Module | Action | Reason |
|----------|-------|--------|--------|--------|
| L1–L9 | 9 | M00 Admin | SKIP | No TypeScript content. Welcome, Discord, IDE setup. |
| L10–L12 | 3 | M01 intro | SKIP | Node.js setup intros — JS dev doesn't need this. |
| **L13–L39** | **27** | **M01 Types** | **DEEP_DIVE** | Foundation. Union, tuple, enum, object types, type guards, unknown, never. |
| L40 | 1 | M02 intro | SKIP | 1-minute module intro. |
| **L41–L49** | **9** | **M02 tsconfig** | **STUDY** | Required for any project. strict, target, lib, module, outDir, rootDir. |
| L50–L57 | 8 | M03 Node | SKIP | Duplicates M19. Node workflow covered better with full TS typing in M19. |
| L58–L65 | 8 | M04 Modern JS | SKIP | User knows ES6+. Arrow functions, destructuring, spread are not TS content. |
| L66 | 1 | M04 links | SKIP | Resource-only lecture. |
| **L67–L87** | **21** | **M05 Classes** | **DEEP_DIVE** | Daily TS usage. Abstract, implements, readonly, access modifiers, interface. |
| **L88–L99** | **12** | **M06 Adv Types** | **STUDY** | Modern patterns. Discriminated unions, type guards, satisfies, unknown vs any. |
| **L100–L109** | **10** | **M07 Generics** | **STUDY** | Library fluency. Generic constraints, extends, utility generics. |
| L110–L117 | 8 | M08 Generic LL | SKIP | CS exercise. Generic linked list — patterns already covered in M07. |
| **L118–L132** | **15** | **M09 Utilities** | **STUDY** | Library reading. keyof, typeof, mapped types, conditional types, infer. |
| **L133–L145** | **13** | **M10 ECMAScript decorators** | **STUDY** | Modern spec (2023+). Angular/NestJS patterns. |
| **L146–L160** | **15** | **M11 Experimental decorators** | **STUDY** | Legacy codebases. experimentalDecorators=true patterns. |
| L161 | 1 | — | SKIP | Resource-only lecture. |
| L162–L179 | 18 | M12 DnD | SKIP (save for Practice) | Project requires Build mode. Speed: read module guide only. Practice: build it. |
| L181 | 1 | — | SKIP | Resource-only lecture. |
| **L182–L191** | **10** | **M13 Modules** | **STUDY** | import type, namespaces, ES modules. Required for multi-file projects. |
| **L192–L198** | **7** | **M14 Vite** | **STUDY** | Modern bundler. Vite + ESBuild setup for new projects. |
| L199–L213 | 15 | M15 Webpack | SKIP | Outdated (2020-era). Replaced by Vite. |
| **L214–L223** | **10** | **M16 @types** | **STUDY** | Daily integration. @types/*, .d.ts files, declare, Zod validation. |
| L224–L231 | 8 | M17 Google Maps | SKIP | Requires external API key. Less relevant than M18/M19 for stated goal. |
| **L232–L245** | **14** | **M18 React+TS** | **STUDY** | Core goal. Props, events, useRef, component generic types. |
| **L246–L257** | **12** | **M19 Node+TS** | **STUDY** | Backend completion. Express typed routes, middleware, @types/node. |
| L258–L339 | 82 | LEGACY | SKIP | Re-recordings of M01/M02/M05/M07. Zero new content. |

**Coverage:** 165 study lectures + 174 skip lectures = 339 total. Skip accounts for 51% of the course.

**Lecture priority classification (for ≥80% of L13–L257):**

| Priority | Lectures | Description |
|----------|----------|-------------|
| P0 Core | L13–L39, L67–L87 | Foundation. Must know before anything else. |
| P1 Essential | L41–L49, L88–L109, L118–L132, L232–L257 | Daily usage. High transfer to real projects. |
| P2 Important | L133–L160, L182–L198, L214–L223 | Context-dependent. Needed for specific codebases. |
| SKIP | Everything else | No TS content, legacy, outdated, or covered better elsewhere. |

---

## 4. Milestones

| Milestone | After Session | Evidence |
|-----------|--------------|---------|
| M1: Type System Foundation | S02 | User can annotate basic TypeScript functions. Active Recall: 3/3 on M01 questions. |
| M2: Project Setup | S03 | User can configure tsconfig.json from memory with strict=true, target=ES2020, correct module/outDir. |
| M3: OOP Fluency | S05 | User can write abstract base class with generic parameters. Implements interface. Uses access modifiers. |
| M4: Type Safety Mastery | S07 | User can read generic type signatures. Writes type guards. Explains discriminated unions. |
| M5: Library Reading | S08 | User can read Partial<T>, ReturnType<F>, infer keyword in @types/* declaration files. |
| M6: Decorator Literacy | S10 | User explains difference between ECMAScript and experimental decorators. Can trace @autobind logic. |
| M7: Modern Build Setup | S11 | User configures Vite + TypeScript project from scratch. Understands import type syntax. |
| M8: Full-Stack TS Literate | S14 | User reads React component prop types and Express middleware types without confusion. |

---

## 5. Skip List

Complete list of lectures excluded from speed mode with rationale.

**Category: Admin (L1–L9, 9 lectures)**
Course welcome, Discord invite, IDE setup instructions. No TypeScript content.

**Category: Module Intros (L10, L12, L40, L66, L161, L181, 6 lectures)**
1–2 minute overview videos. No technical content. Safely skipped.

**Category: Already Know (L50–L65, 14 lectures)**
Node.js project setup (M03) and Modern JavaScript (M04). A JS developer already knows:
arrow functions, default params, rest/spread, destructuring, for..of, template literals. These are ES6 syntax, not TypeScript.

**Category: CS Exercise (L110–L117, 8 lectures)**
Generic linked list project (M08). Demonstrates generic constraints that M07 already covers. The linked list data structure itself is not a TypeScript production pattern — it's a CS fundamentals exercise.

**Category: DnD Project — saved for Practice mode (L162–L179, 18 lectures)**
The DnD project applies 6+ TS patterns simultaneously. In Speed mode, building it from scratch before mastering individual patterns causes frustration. In Practice mode (where it belongs), user has the individual patterns mastered and can attempt the full build.

**Category: Outdated (L199–L213, 15 lectures)**
Webpack + TypeScript configuration (circa 2020). Covered tools: ts-loader, fork-ts-checker-webpack-plugin, webpack-merge. These are superseded by Vite + ESBuild. New projects should use Vite (L192–L198).

**Category: Out of Scope (L224–L231, 8 lectures)**
Google Maps SDK integration project. Requires external API key. TS skills demonstrated (external .d.ts, typing SDK objects) are covered in M16. React/Node (M18/M19) is higher priority for user's goal.

**Category: Legacy (L258–L339, 82 lectures)**
Re-recordings of M01 (L258–L274), M05 (L275–L300), M07 (L301–L339). Confirmed via transcript review: same examples, same code, lower recording quality. Zero new content.

---

## 6. Active Recall Strategy

Each session ends with Active Recall — NOT re-reading. Active Recall is the primary learning mechanism, not the review at the end.

**The rule:** If you can't recall it, you don't know it. Re-reading creates the illusion of knowing. Active Recall exposes gaps.

**Session Active Recall protocol:**
1. Close all tabs and notes (no references)
2. Answer the 3 module guide questions from memory
3. Grade yourself: ≥2/3 correct = session passes. <2/3 = re-read the module guide section you missed and test again tomorrow (do not advance)
4. If stuck for >5 minutes on a question: write "I don't know X" and continue. Note X for next session start.

**Active Recall question format:**
Each module guide contains exactly 3 Active Recall questions in this structure:
- Q1: Definition/concept ("What is a discriminated union?")
- Q2: Application ("Given this interface, what type guard would you write?")
- Q3: Gotcha/nuance ("Why is `as any` different from `unknown` — and which is safer?")

**Spaced repetition (optional):**
For lectures marked DEEP_DIVE (M01 + M05), repeat Active Recall after 3 days and after 7 days. This is optional but reduces forgetting by 60–80% (Ebbinghaus, 1885). The `study-progress.md` tracks `review_due` dates for this.

**The "Feynman test":**
Before marking a session complete, user should be able to explain the core concept in one plain-English sentence to an imaginary non-technical listener. Example: "TypeScript generics let you write functions that work correctly for different types without losing type information — like a function that can accept and return both strings and numbers while keeping track of which is which." If you can't articulate it simply, you don't understand it yet.
