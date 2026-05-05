---
date: 2026-05-06
type: retrieval-index
course_slug: understanding-typescript
created: 2026-05-06
generated_by: typescript-76l
---

# Retrieval Index — Understanding TypeScript

Quick lookup for concepts, Q&A, decisions, and patterns encountered during learning. Populated as learning sessions progress — auto-appended by `/viec hoc`.

---

## Key Questions Answered

Pre-populated from course analysis. More entries added as user studies and asks questions.

| Question | Answer Summary | Source File | Tags |
|----------|---------------|-------------|------|
| When should I annotate a type vs. rely on inference? | Annotate: function params, return types when complex, variables without initializer. Infer: variable with initializer, simple return types. | `_course-profile.md` Section 2 | inference, annotation |
| What's the difference between interface and type alias? | Interfaces: extensible via declaration merging, implements on classes. Type aliases: unions/intersections possible, more flexible, but no merging. For objects: either works; prefer interface for classes. | `_knowledge-map.md` Layer 3 | interface, type-alias |
| When to use `unknown` vs `any`? | `unknown` requires type narrowing before use — safer than `any`. Use `unknown` for untrusted external data (API responses). Use `any` only as last resort escape hatch. | `_knowledge-map.md` Layer 1 | unknown, any, type-safety |
| What is a discriminated union? | A union type where all members share a common literal property (the discriminant, e.g., `kind: "A" \| "B"`). TypeScript narrows based on that property. | `_knowledge-map.md` Layer 4 | discriminated-union, narrowing |
| What does `satisfies` do that `as` doesn't? | `satisfies` validates a value matches a type WITHOUT widening — keeps the narrower inferred type. `as` forces a type assertion. Use `satisfies` when you want both type-checking and type narrowing preserved. | `_learning-rationale.md` Section 5 (L99) | satisfies, type-assertion |
| What's the difference between `T extends U` in generics vs conditionals? | In generics: constraint — T must be assignable to U. In conditional types: check — if T extends U, yield X else Y. Same syntax, different contexts. | `_knowledge-map.md` Layer 2 | generics, conditional-types |
| Why use abstract classes instead of interfaces? | Abstract classes can have implementation (default method bodies, constructor logic). Interfaces = pure contract. Use abstract when sharing default behavior; interface when only defining contract. | `_knowledge-map.md` Layer 3 | abstract-class, interface |
| How does the DnD project singleton pattern work? | `private constructor()` prevents `new`. `static getInstance()` creates instance lazily on first call, stores in `private static instance`. Guarantees one instance. | `_lecture-resource-map.md` + `179-prj-16-finished/` | singleton, pattern |
| What is `infer` used for? | Extract a type from inside a conditional type. Example: `type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never`. | `_knowledge-map.md` Layer 2 | infer, conditional-types |
| When should I use `Partial<T>` vs `Required<T>`? | `Partial<T>`: all fields optional — useful for update/patch operations. `Required<T>`: all fields required — reverse Partial. | `_knowledge-map.md` Layer 2 | partial, required, utility-types |
| What does `as const` do? | Marks all properties as readonly and preserves literal types (not widened to string/number). Use for fixed configuration objects, tuples, enum-like constants. | `_knowledge-map.md` Layer 4 | as-const, literal-types |
| What's a mapped type? | A type that creates a new type by iterating over keys of another type. `{ [K in keyof T]: V }`. Can add/remove `readonly`/`?` modifiers with `+`/`-`. | `_knowledge-map.md` Layer 2 | mapped-types, keyof |
| What's the @autobind decorator pattern? | A method decorator that replaces the method with a getter. The getter returns a bound version of the original function. Solves `this` context loss in event handlers. | `_lecture-resource-map.md` + `156-decorators-09-example-autobind/` | autobind, decorator, this-binding |

---

## Decisions

Key decisions made during learning system design. More entries added as learning progresses.

| Decision | Reason | Source |
|----------|--------|--------|
| Speed mode first, then Practice, Expert optional | User has JS background → 80/20 covers critical TS fast; Practice needs mental model first; Expert not required for "build project" goal | `_learning-rationale.md` Section 4 |
| Skip L258–L339 entirely | Re-recordings of M01/M02/M05/M07 — zero new content, lower quality | `_course-profile.md` Section 5 |
| Skip L204–L213 (Webpack) | Outdated — Vite (M14) is the modern equivalent and simpler | `_learning-rationale.md` Section 5 |
| L110–L117 = skip in Speed, optional in Practice | M07 already covers generics; linked list is CS exercise not TS skill | `_learning-rationale.md` Section 5 |
| Use Vite (L192–L198) not Webpack (L204–L213) | Course itself shows Vite in newer resources (233-starting-project/); Webpack section is older | `_course-profile.md` + `_lecture-resource-map.md` |
| DnD project (M12) = centerpiece of Practice mode | Only project applying 6+ patterns simultaneously; abstract base class pattern transfers to Angular/NestJS | `_learning-rationale.md` Section 4 |
| Practice module guides must NOT show solution | If solution shown, user doesn't struggle → no learning. Only show spec and hints | Task typescript-ygy (worklog) |

---

## Concepts

Core TypeScript concepts with best learning sources. Populated from knowledge map; expanded as user studies.

| Concept | Best Source | Related Files | Tags |
|---------|------------|---------------|------|
| Type inference | `speed/module-guides/M01*.md` | `_knowledge-map.md` Layer 1 | L15, inference, foundation |
| Union types | `speed/module-guides/M01*.md` | `_knowledge-map.md` Layer 1 | L18, union, narrowing |
| Tuple types | `speed/module-guides/M01*.md` | `_knowledge-map.md` Layer 1 | L22, tuple, fixed-length |
| tsconfig strict mode | `speed/module-guides/M02*.md` | `_knowledge-map.md` Layer 6 | L44, tsconfig, strict |
| Class access modifiers | `speed/module-guides/M05*.md` | `_knowledge-map.md` Layer 3 | L71, private, protected, readonly |
| Abstract classes | `speed/module-guides/M05*.md` | `_knowledge-map.md` Layer 3 | L78, abstract, inheritance |
| Interfaces | `speed/module-guides/M05*.md` | `_knowledge-map.md` Layer 3 | L79-L87, interface, implements |
| Type guards | `speed/module-guides/M06*.md` | `_knowledge-map.md` Layer 4 | L90-L93, instanceof, typeof, in |
| Discriminated unions | `speed/module-guides/M06*.md` | `_knowledge-map.md` Layer 4 | L91, kind, narrowing |
| Generics | `speed/module-guides/M07*.md` | `_knowledge-map.md` Layer 5 | L101-L109, T, constraints |
| Generic constraints | `speed/module-guides/M07*.md` | `_knowledge-map.md` Layer 5 | L106-L107, extends, keyof |
| Mapped types | `speed/module-guides/M09*.md` | `_knowledge-map.md` Layer 2 | L126-L127, keyof, readonly |
| Conditional types | `speed/module-guides/M09*.md` | `_knowledge-map.md` Layer 2 | L129-L131, infer, extends |
| Built-in utility types | `speed/module-guides/M09*.md` | `_knowledge-map.md` Layer 2 | L132, Partial, Pick, Omit |
| ECMAScript decorators | `speed/module-guides/M10*.md` | `_knowledge-map.md` Layer 7 | L133-L145, decorator, class |
| @autobind pattern | `practice/code-walkthrough.md` | `_lecture-resource-map.md` 156-autobind | L156, getter, descriptor |
| Singleton pattern | `practice/code-walkthrough.md` | `_lecture-resource-map.md` 179-prj | L170, private-constructor, getInstance |
| Observer/Listener | `practice/code-walkthrough.md` | `_lecture-resource-map.md` 179-prj | L170, Listener<T>, state-management |
| ES modules + type imports | `speed/module-guides/M13*.md` | `_knowledge-map.md` Layer 6 | L187-L191, import-type, barrel |
| Zod integration | `speed/module-guides/M16*.md` | `_knowledge-map.md` Layer 6 | L214-L223, runtime-validation |
| React + TypeScript | `speed/module-guides/M18*.md` | `_knowledge-map.md` Layer 6 | L232-L245, FC, useState, useRef |

---

## Problems Solved

Specific problems encountered and their TypeScript solutions. Populated as user studies.

| Problem | Solution | Source |
|---------|----------|--------|
| `this` is undefined in class event handler | Use `@autobind` decorator or `this.method = this.method.bind(this)` in constructor | `_lecture-resource-map.md` 156-autobind |
| Function receiving string but value could be null | Use `string \| null` union, then narrow with `if (value !== null)` before using | `_knowledge-map.md` Layer 1 |
| Want to ensure object matches type without losing literal info | Use `satisfies` instead of `: MyType` annotation | `_knowledge-map.md` Layer 4 |
| Generic function needs to access property that may not exist | Add constraint: `<T extends { id: string }>` to ensure `id` is accessible | `_knowledge-map.md` Layer 5 |
| Want to make all fields of a type optional for update payload | Use `Partial<T>` utility type | `_knowledge-map.md` Layer 2 |

---

## Open Threads

Questions still unanswered — to be resolved during learning sessions.

| Topic | Status | Next Action |
|-------|--------|-------------|
| Context API typing in M18 (useContext<T>) | open | Check transcripts L238–L240 |
| Async error handling in M19 | open | Check transcripts L250–L255 |
| NestJS decorator compatibility in M10 | open | Check L133–L145 for @Injectable analogue |
| tsconfig `paths` coverage in M02 | open | Check L41–L49 transcripts |
| `satisfies` depth (L99) — intro or deep dive? | open | Read L99 transcript |
