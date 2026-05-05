# E08 — Modules & Import Types

**Expert module** | Covers: L182–L191 | ~55 min

---

## 1. Core Mental Model

> "`import type { X }` is a correctness requirement when X is a type-only symbol. It's not a style preference. Tools with `isolatedModules: true` (Vite, esbuild, Babel) transpile each file independently — they can't tell if `X` is a runtime value or a type-only construct. `import type` tells them explicitly."

Expert insight: the distinction between "JavaScript module graph" and "TypeScript type information" is fundamental. Types are erased. Everything in the JS output must be a real value. `import type` marks the erasure boundary.

---

## 2. Beginner Misunderstanding

> "I don't use `import type` — it's just extra syntax for something TypeScript already understands."

The problem shows up with `isolatedModules: true`. When a bundler transpiles a file without cross-file analysis, it sees `import { Request } from 'express'` and must decide: is `Request` a value or a type? It can't know — so it emits an import for `Request`. Now your bundle includes a runtime import that immediately throws because `Request` doesn't exist as an exported value from the `express` module.

---

## 3. Professional Concern

> "Our monorepo has a file that re-exports types from a shared package. The Vite build fails with 'Re-exporting a type requires using export type'."

This is `isolatedModules: true` in action. The fix: `export type { MyType }` in barrel files. This is a discipline issue — teams need to be consistent about `import type` for all type-only imports.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: Circular imports with shared types**
```typescript
// a.ts imports from b.ts
// b.ts imports from a.ts
// Runtime: works (JS handles circular deps)
// TypeScript: may work but causes subtle initialization order bugs
// What's the recommended fix for circular imports that only share types?
```

**Trap 2: Barrel file `index.ts` re-export breaks tree-shaking**
```typescript
// index.ts
export { ProjectItem } from './project-item';
export { ProjectList } from './project-list';
export { ProjectInput } from './project-input';
// Problem: importing one thing imports all three files into the bundle
// What's the tradeoff between barrel files (convenience) and tree-shaking (perf)?
```

**Trap 3: `module: "commonjs"` with `import` syntax**
```typescript
// tsconfig: "module": "commonjs"
import { readFile } from 'fs/promises'; // works
import type { PathLike } from 'fs'; // where does this go in the output?
// What does TypeScript emit for this when targeting CommonJS?
```

---

## 5. Expert Shortcut

**`import type` — all forms:**
```typescript
// Per-import type annotation
import { type DragTarget, ProjectStatus } from './models.js';
//              ^^^^                          ^^^^
//         type-only                     runtime value (enum has JS output)

// Full import is type-only
import type { Draggable, DragTarget } from './interfaces.js';

// Re-export types
export type { User, Project } from './models.js';
```

**Checking if `isolatedModules` applies to your project:**
```bash
# In tsconfig.json
"isolatedModules": true  # Vite sets this; Babel requires it; esbuild requires it
```

**Shared types pattern (breaks circular deps):**
```typescript
// types.ts — no imports from other app files
export interface Project { id: string; title: string; status: ProjectStatus }
export type Listener<T> = (items: T[]) => void;

// state.ts — imports only from types.ts
import type { Project, Listener } from './types.js';
```

---

## 6. Real-world Scenario

You're auditing a Vite + TypeScript codebase for `import type` compliance. You have 40 files. What's the fastest way to identify violations? (Think: what does the violation look like in source? How would you grep for it?)

After finding them, your colleague says: "Just disable `isolatedModules: true` in tsconfig — problem solved." What are the tradeoffs of this approach?

---

## 7. Deliberate Drill

**Constraint:** Audit these imports and label each one: CORRECT / NEEDS `type` / AMBIGUOUS:

```typescript
// In a Vite project (isolatedModules: true)
import { Component } from './base-component.js';        // used as base class
import { ProjectStatus } from './models.js';             // used in switch
import { Project } from './models.js';                   // used only as type annotation
import { validate, Validatable } from './validation.js'; // validate() called, Validatable used as type
import { autobind } from './decorators.js';              // decorator used on method
```

For each one that needs `type`, show the corrected import.

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can explain what `isolatedModules: true` does and why bundlers need it
- [ ] Know all three forms of `import type` syntax
- [ ] Understand why `enum` is a runtime value (needs JS output) but `interface` is not
- [ ] Can audit a file for `import type` violations

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. `namespace` vs ES modules: give one concrete reason why namespaces are dangerous for large codebases that ES modules don't have.

2. You have `export * from './models'` in a barrel file. A new developer imports `UserService` from the barrel. Later, `UserService` is moved to a separate package and `models.ts` is deleted. The barrel file now has a broken export. How could TypeScript have caught this earlier?

3. The `.js` extension in ESM imports (`import from './file.js'`) confuses people because the file is actually `.ts`. Why does TypeScript require `.js` even in source code, and what setting controls this behavior?
