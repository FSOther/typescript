---
type: knowledge-map
course_slug: understanding-typescript
created: 2026-05-04
---

# Knowledge Map

## Main Modules

| Module | Lectures | Concepts | Prerequisites | Output |
|---|---|---|---|---|
| M02-Types | L10-39 | union, intersection, literal, record, enum, type alias, never, unknown, type narrowing | JS basics | Hiểu type system TS |
| M03-Config | L40-49 | tsconfig, target, lib, strict, paths | M02 | Cấu hình project đúng cách |
| M04-Mini-Project | L50-57 | Áp dụng types vào project thực | M02 | Code TS chạy được |
| M06-OOP | L67-87 | class, interface, abstract, readonly, static, implements, extends, declaration merging | M02 | OOP với TS |
| M07-AdvTypes | L88-99 | type guard, discriminated union, instanceof, predicate, overloads, index type, satisfies | M06 | Narrowing & pattern nâng cao |
| M08-Generics | L100-109 | generic functions, generic classes, constraints, multiple params | M07 | Reusable type-safe code |
| M09-GenProject | L110-117 | Linked list với generic | M08 | Generic class pattern |
| M10-Utility | L118-132 | typeof, keyof, indexed access, mapped types, template literal, conditional types, infer, built-ins | M08 | Type manipulation |
| M11-Decorators | L133-145 | ECMAScript decorators, class/method/field decorator | M06 | Modern decorator pattern |
| M12-ExpDecorators | L146-161 | Experimental decorators, factories, validation | M11 | Legacy decorator pattern |
| M13-DnD | L162-181 | Full project với classes, interfaces, state, drag & drop | M07, M11 | Real project |
| M14-Modules | L182-193 | namespace, ES modules, import/export, type imports | M06 | Module system |
| M15-Build | L194-213 | Vite, Webpack, .d.ts files | M14 | Build pipeline |
| M16-Libraries | L214-231 | @types, .d.ts, declare, Zod, runtime validation | M10 | Third-party integration |
| M17-React | L232-245 | React + TS, props typing, state, refs, hooks | M08 | React với TS |
| M18-Node | L246-257 | Node + Express + TS, route typing, native TS support | M08 | Backend với TS |

## Concept Dependency Graph

| Concept | Depends On | Used In |
|---|---|---|
| Union types | Basic types | Discriminated unions, type guards |
| Type inference | Basic types | Everywhere |
| Generic types | Types, interfaces | Generics, utility types |
| Mapped types | keyof, typeof | Custom utility types |
| Conditional types | Generic types | infer, built-in utilities |
| Type guards | Union types | Narrowing |
| Discriminated unions | Union types, interface | Real-world patterns |
| Decorators | Classes | Framework patterns |

## Suggested Learning Order (cho user đã biết JS)

1. **M02** — Type system (STUDY mọi thứ trừ setup)
2. **M06** — Classes & interfaces (DEEP_DIVE)
3. **M07** — Advanced types (DEEP_DIVE)
4. **M08** — Generics (DEEP_DIVE)
5. **M10** — Type utilities (DEEP_DIVE)
6. **M13** — Full project drag-and-drop (BUILD)
7. **M09** — Generic linked list (BUILD)
8. **M17/M18** — React & Node với TS (DEEP_DIVE)
9. **M11/M12** — Decorators (STUDY)
10. **M03** — tsconfig (SKIM)
11. **M14** — Modules (SKIM)
12. **M16** — Third-party types (STUDY)

## Parallelizable Modules

| Module | Can Learn In Parallel With | Reason |
|---|---|---|
| M03-Config | M02-Types | Config không phụ thuộc type concepts |
| M09-GenProject | M10-Utility | Sau khi xong M08 |
| M17-React | M18-Node | Độc lập nhau |
