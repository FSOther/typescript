---
type: learning-plan
course_slug: understanding-typescript
mode: speed
status: ready
created: 2026-05-04
---

# Learning Plan — Speed (Học tốc độ cao nhất)

## Mode Objective

Đã biết JS → nắm TS đủ dùng trong 2–3 tuần (2h/ngày). Skip setup, skip legacy, skip modern-JS section. Chỉ học những gì TS khác JS, và những concept không thể thiếu khi code TS thực tế.

## Learning Policy

| Rule | Description |
|---|---|
| Skip policy | Bỏ setup, bỏ modern-JS (biết rồi), bỏ toàn bộ legacy L258-339 |
| Depth per concept | Hiểu WHY + WHEN TO USE, không cần nhớ hết syntax |
| Practice depth | Chỉ đọc transcript, không cần chạy code trừ M13 project |
| Output | Cheatsheet + mental model per module |

## Lecture Action Matrix

### M01 — Setup (L1-9): SKIP tất cả
| Lecture | Action | Reason |
|---|---|---|
| L1-9 | SKIP | Intro/setup — biết JS + đọc CLAUDE.md là đủ |

### M02 — Type System (L10-39): STUDY core, SKIM detail
| Lecture | Title | Action | Key Point |
|---|---|---|---|
| L10 | Module Introduction | SKIP | |
| L11 | Node.js To Run JS | SKIP | biết rồi |
| L12 | Project Setup | SKIP | biết rồi |
| L13 | Working with Types | STUDY | TS types overview |
| L14 | Vanilla JS Has Types Too | SKIM | context |
| L15 | Type Inference vs Assignment | STUDY | Core concept #1 |
| L16 | Assigning Types To Function Params | STUDY | function typing |
| L17 | The "any" Type | STUDY | avoid it |
| L18 | Understanding Union Types | STUDY | Core concept #2 |
| L19 | Arrays & Types | STUDY | |
| L20 | Advanced Array Types | STUDY | |
| L21 | First Glimpse at Generic Types | SKIM | preview |
| L22 | Making Sense of Tuples | STUDY | |
| L23 | Object Types | STUDY | |
| L24 | "Must Not Be Null" Type | STUDY | ! operator |
| L25 | Record Type | STUDY | useful utility |
| L26 | Working with Enums | STUDY | |
| L27 | Literal Types | STUDY | Core concept #3 |
| L28 | Type Aliases & Custom Types | STUDY | fundamental |
| L29 | Function Return Value Types | STUDY | |
| L30 | The "void" Type | STUDY | |
| L31 | The "never" Type | STUDY | important |
| L32 | Functions As Types | STUDY | |
| L33 | null & undefined | STUDY | |
| L34 | Inferred null & Type Narrowing | STUDY | narrowing preview |
| L35 | Forced "Not Null" & Optional Chaining | STUDY | |
| L36 | Type Casting | STUDY | |
| L37 | The "unknown" Type | STUDY | vs any |
| L38 | Optional Values | STUDY | |
| L39 | Nullish Coalescing | SKIM | biết JS rồi |

### M03 — tsconfig (L40-49): SKIM
| Lecture | Action | Reason |
|---|---|---|
| L40 | Module Intro | SKIP | |
| L41-L48 | tsconfig options | SKIM | đọc nhanh, không cần nhớ hết |
| L49 | Type Packages | STUDY | @types — quan trọng |

### M04 — Mini Project (L50-57): SKIM
| Lecture | Action | Reason |
|---|---|---|
| L50-57 | Mini project | SKIM | xem cách apply, không cần làm |

### M05 — Modern JS (L58-66): SKIP
| Lecture | Action | Reason |
|---|---|---|
| L58-66 | Arrow fn, spread, destructuring | SKIP | biết JS rồi |

### M06 — Classes & Interfaces (L67-87): STUDY/DEEP_DIVE
| Lecture | Title | Action |
|---|---|---|
| L67 | Module Intro | SKIP | |
| L68 | What are Classes? | SKIM | biết OOP |
| L69 | Creating a First Class | STUDY | TS class syntax |
| L70 | Useful Shortcut & Compiling | STUDY | |
| L71 | public and private | STUDY | |
| L72 | readonly | STUDY | |
| L73 | Getters | STUDY | |
| L74 | Setters | STUDY | |
| L75 | Static Properties & Methods | STUDY | |
| L76 | Understanding Inheritance | STUDY | |
| L77 | protected Modifier | STUDY | |
| L78 | Abstract Classes | DEEP_DIVE | important pattern |
| L79 | Introducing Interfaces | DEEP_DIVE | Core concept #4 |
| L80-L81 | Interface basics | STUDY | |
| L82 | Interfaces vs Type Aliases | DEEP_DIVE | confusion point |
| L83 | Interfaces for Function Types | STUDY | |
| L84-L87 | Implementing, extending interfaces | DEEP_DIVE | |

### M07 — Advanced Types (L88-99): DEEP_DIVE
| Lecture | Title | Action |
|---|---|---|
| L88 | Module Intro | SKIP | |
| L89 | Intersection Types | DEEP_DIVE | |
| L90 | More on Type Guards | DEEP_DIVE | Core concept #5 |
| L91 | Discriminated Unions | DEEP_DIVE | Core concept #6 |
| L92 | Type Guards via instanceof | DEEP_DIVE | |
| L93 | Type Predicates | DEEP_DIVE | |
| L94-L95 | Function Overloads | STUDY | |
| L96 | Index Types | STUDY | |
| L97 | Constant Types "as const" | STUDY | |
| L98 | Record Type revisited | STUDY | |
| L99 | satisfies Keyword | STUDY | new TS feature |

### M08 — Generics (L100-109): DEEP_DIVE
| Lecture | Title | Action |
|---|---|---|
| L100 | Module Intro | SKIP | |
| L101 | Generic Type We Already Know | STUDY | Array<T> |
| L102 | Understanding Generic Types | DEEP_DIVE | Core concept #7 |
| L103 | Creating & Using Generic Type | DEEP_DIVE | |
| L104 | Generic Functions & Inference | DEEP_DIVE | |
| L105 | Multiple Generic Parameters | STUDY | |
| L106-L107 | Generics & Constraints | DEEP_DIVE | |
| L108 | Generic Classes & Interfaces | DEEP_DIVE | |
| L109 | Summary | SKIM | |

### M09 — Generic Project (L110-117): STUDY
| Lecture | Action |
|---|---|
| L110 | SKIP |
| L111-L117 | STUDY — đọc code, hiểu pattern |

### M10 — Type Utilities (L118-132): DEEP_DIVE
| Lecture | Title | Action |
|---|---|---|
| L118 | Module Intro | SKIP | |
| L119-L121 | typeof | DEEP_DIVE | Core concept #8 |
| L122-L123 | keyof | DEEP_DIVE | Core concept #9 |
| L124-L125 | Indexed Access Types | DEEP_DIVE | |
| L126-L127 | Mapped Types | DEEP_DIVE | Core concept #10 |
| L128 | Template Literal Types | STUDY | |
| L129-L130 | Conditional Types | DEEP_DIVE | |
| L131 | infer Keyword | DEEP_DIVE | advanced |
| L132 | Built-in Utility Types | STUDY | Pick, Partial, etc. |

### M11 — ECMAScript Decorators (L133-145): STUDY
| Lecture | Action |
|---|---|
| L133-L134 | STUDY — what & why |
| L135-L145 | STUDY — class, method, field decorators |

### M12 — Experimental Decorators (L146-161): SKIM
| Lecture | Action |
|---|---|
| L146 | SKIP |
| L147-L161 | SKIM — biết đây là legacy approach |

### M13 — DnD Project (L162-181): BUILD (quan trọng nhất!)
| Lecture | Action |
|---|---|
| L162 | SKIP |
| L163-L181 | BUILD — đọc kỹ + chạy code trong resources/ |

### M14 — Modules (L182-193): STUDY
| Lecture | Action |
|---|---|
| L182-L191 | STUDY |
| L192-L193 | SKIP |

### M15 — Build Tools (L194-213): SKIM
| Lecture | Action |
|---|---|
| L194-L199 | SKIM |
| L200-L201 | STUDY — .d.ts files quan trọng |
| L202-L213 | SKIM |

### M16 — 3rd Party (L214-231): STUDY
| Lecture | Action |
|---|---|
| L214-L220 | STUDY — @types packages |
| L221-L223 | STUDY — Zod |
| L224-L231 | SKIM — specific project |

### M17 — React + TS (L232-245): DEEP_DIVE
| Lecture | Action |
|---|---|
| L232 | SKIP |
| L233-L245 | DEEP_DIVE — props, state, refs typing |

### M18 — Node + Express (L246-257): DEEP_DIVE
| Lecture | Action |
|---|---|
| L246 | SKIP |
| L247-L257 | DEEP_DIVE |

### LEGACY (L258-339): SKIP tất cả
| Lectures | Action | Reason |
|---|---|---|
| L258-339 | SKIP | Duplicate content của M02-M08 |

## Module Plan

| Module | Lectures | Action | Est. Time | Priority |
|---|---|---|---|---|
| M02 | L10-39 | STUDY | ~3h | P0 |
| M06 | L67-87 | DEEP_DIVE | ~3h | P0 |
| M07 | L88-99 | DEEP_DIVE | ~2.5h | P0 |
| M08 | L100-109 | DEEP_DIVE | ~2.5h | P0 |
| M10 | L118-132 | DEEP_DIVE | ~3h | P0 |
| M13 | L162-181 | BUILD | ~4h | P1 |
| M17 | L232-245 | DEEP_DIVE | ~3h | P1 |
| M18 | L246-257 | DEEP_DIVE | ~2.5h | P1 |
| M11 | L133-145 | STUDY | ~2h | P2 |
| M03 | L40-49 | SKIM | ~1h | P2 |
| M09 | L110-117 | STUDY | ~1.5h | P3 |
| M14 | L182-193 | STUDY | ~2h | P3 |
| M16 | L214-231 | STUDY | ~2h | P3 |
| M12 | L146-161 | SKIM | ~1h | P4 |
| M15 | L194-213 | SKIM | ~1h | P4 |

**Tổng ước tính (speed mode):** ~33h → ~17 ngày × 2h
