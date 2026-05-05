---
type: course-profile
course_slug: understanding-typescript
created: 2026-05-04
---

# Course Profile

## Course Summary

- Course title: Understanding TypeScript (Maximilian Schwarzmüller — Udemy)
- Main domain: Programming
- Main skill: TypeScript
- Target learner level: beginner → intermediate
- Estimated course difficulty: easy → medium (tăng dần)
- Total lectures: 339 (+ legacy L258-339 là duplicate)
- Unique content: L1-257 (nhưng L1-9 là intro/setup có thể skip)

## Detected Stack

| Layer | Technology | Confidence |
|---|---|---|
| Language | TypeScript | High |
| Runtime (browser) | Browser DOM | High |
| Runtime (node) | Node.js | High |
| Framework (section) | React (L232-245) | High |
| Framework (section) | Express (L246-257) | High |
| Build tool (section) | Webpack (L205-213) | High |
| Build tool (section) | Vite (L197-201) | High |
| Validation library | Zod (L221-223) | Medium |
| Compiler | tsc | High |

## Course Structure (Modules)

| Module | Lectures | Topic | Priority |
|---|---|---|---|
| M01 | L1-9 | Setup & intro | Skip (biết JS rồi) |
| M02 | L10-39 | TypeScript types & type system | CORE |
| M03 | L40-49 | tsconfig | STUDY |
| M04 | L50-57 | Mini project (types) | PRACTICE |
| M05 | L58-66 | Modern JS features với TS | SKIM (biết JS) |
| M06 | L67-87 | Classes & interfaces | CORE |
| M07 | L88-99 | Advanced types | CORE |
| M08 | L100-109 | Generics in depth | CORE |
| M09 | L110-117 | Generic linked list project | DEEP_DIVE |
| M10 | L118-132 | Type utilities (keyof, mapped, conditional, infer) | CORE |
| M11 | L133-145 | ECMAScript Decorators (mới) | STUDY |
| M12 | L146-161 | Experimental Decorators | STUDY |
| M13 | L162-181 | Full project: drag-and-drop | BUILD |
| M14 | L182-193 | Modules & namespaces | STUDY |
| M15 | L194-213 | Build tools (Vite/Webpack) | SKIM |
| M16 | L214-231 | Third-party types & Zod | STUDY |
| M17 | L232-245 | React + TypeScript | DEEP_DIVE |
| M18 | L246-257 | Node.js + Express + TypeScript | DEEP_DIVE |
| LEGACY | L258-339 | Duplicate content | SKIP |

## Key Concepts

| Concept | Importance | Notes |
|---|---|---|
| Type inference vs assignment | High | Foundation — must understand |
| Union & intersection types | High | Used everywhere |
| Generic types | High | Core TS superpower |
| Keyof / typeof | High | Advanced type manipulation |
| Mapped types | High | Custom utility types |
| Conditional types + infer | High | Expert-level |
| Discriminated unions | High | Pattern dùng nhiều trong real-world |
| Function overloads | Medium | Quan trọng cho library authoring |
| Decorators (ECMAScript) | Medium | Modern approach, L133+ |
| Type guards | High | Narrowing — critical pattern |
| Record type | Medium | Utility type hay dùng |
| Zod | Medium | Runtime validation |

## User Fit (Đã biết JS, muốn học TS bài bản)

- User likely already knows: JS syntax, arrow functions, destructuring, async/await, classes
- User should study deeply: type system, generics, advanced type utilities, real-world patterns
- User can safely skip: L1-9 (setup), L58-66 (modern JS), L258-339 (legacy)
- Expert-level gaps course doesn't fully cover: advanced infer patterns, variance, template literal types deep, conditional type recursion, declaration merging
