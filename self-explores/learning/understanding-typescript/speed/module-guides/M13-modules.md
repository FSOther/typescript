# M13 — Modules (L182–L191)

**Speed module** | Lectures: 182–191 | ~35 min

---

## 1. Key Concepts

| Concept | One-liner |
|---------|-----------|
| ES modules | Native browser/Node syntax; `export`/`import` per file |
| Namespaces | TypeScript-only feature; avoid in modern code |
| Named export | `export const fn = ...` — imported by exact name |
| Default export | `export default fn` — imported with any name |
| `import type` | Marks import as type-only; erased at compile time |
| Barrel file | `index.ts` that re-exports from multiple files |
| `module` tsconfig | Controls output format: `commonjs`, `es2022`, `nodenext` |

**Why ES modules beat namespaces:** Namespaces lack explicit import graphs — you can delete a file and the code still compiles if another file happens to import it. ES modules fail loudly on missing imports.

---

## 2. Code Patterns

### Basic export/import
```typescript
// drag-drop.ts
export interface Draggable {
  dragStartHandler(event: DragEvent): void;
  dragEndHandler(event: DragEvent): void;
}

// project-list.ts
import { Draggable } from './drag-drop.js'; // .js extension in ESM output
```

### Wildcard (namespace) import
```typescript
import * as Validation from './validation.js';
// Use as: Validation.validate(input)
// Avoids name collisions; groups related utilities
```

### Alias import
```typescript
import { autobind as Autobind } from './decorators.js';
// Rename to avoid clash with local autobind variable
```

### import type (type-only)
```typescript
import type { DragTarget } from './drag-drop.js';
// DragTarget is an interface — erased at runtime
// Required by some bundlers (Vite with isolatedModules: true)
// Also supported as per-name: import { type DragTarget, someValue } from '...'
```

### Default export
```typescript
// base-component.ts
export default class Component<T extends HTMLElement, U extends HTMLElement> { ... }

// importer.ts
import Component from './base-component.js'; // any name, no braces
```

### tsconfig for ES modules (browser)
```json
{
  "compilerOptions": {
    "module": "es2022",
    "moduleResolution": "bundler",
    "target": "es2022"
  }
}
```

---

## 3. Common Gotchas

| Gotcha | Detail |
|--------|--------|
| `.js` extension in imports | Even in `.ts` files, ES module imports need `.js` (the compiled output extension) — TypeScript resolves `.ts` but emits `.js` |
| Namespaces still compile | Deleting a file referenced only by namespace won't produce a build error |
| `isolatedModules: true` | Bundlers like Vite require this; forces `import type` for type-only imports |
| Circular imports | ES modules handle them but can cause `undefined` values at init time; split shared types to a separate file |
| Browser needs `<script type="module">` | Without this, ES imports fail at runtime |

---

## 4. Active Recall

> **No answers here.** Close this file and answer from memory.

1. You need to import `ProjectStatus` (an enum) and `Project` (a class) from the same file. You later discover `ProjectStatus` is only used for type annotations. How should you write the import?

2. Your teammate deleted `validators.ts` but the build still passes with zero errors. What module system is the project using, and what's the fix?

3. In a Vite project with `isolatedModules: true`, you try to `import { Listener } from './state.ts'` but get an error: *"Re-exporting a type when the '--isolatedModules' flag is provided requires using 'export type'."* What caused this and how do you fix it?

---

## 5. Quick Reference

```bash
# tsconfig snippet for ES modules (Vite/browser)
"module": "es2022"
"moduleResolution": "bundler"

# tsconfig snippet for Node.js CJS
"module": "commonjs"
"moduleResolution": "node"

# Syntax summary
import { Named } from './file.js'           // named
import DefaultThing from './file.js'         // default
import * as All from './file.js'             // wildcard
import type { OnlyType } from './file.js'    // type-only
export default function foo() { ... }        // default export
export { foo, bar }                          // re-export names
export type { MyType }                       // re-export type only
```

---

## 6. Done Criteria

- [ ] Can explain why namespaces are problematic for large codebases
- [ ] Know when to use `import type` vs regular import
- [ ] Understand `.js` extension rule in TypeScript ESM output
- [ ] Can configure tsconfig for browser (ESM) vs Node.js (CJS)
