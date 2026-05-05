---
type: module-guide
mode: speed
module: M02
course_slug: understanding-typescript
lectures: L41-L49
sessions: S03
created: 2026-05-06
---

# M02 — tsconfig Essentials (L41–L49)

## 1. Key Concepts

`tsconfig.json` controls how TypeScript compiles your code. Without understanding tsconfig, you can't use TypeScript in a real project.

**The 5 flags you MUST know:**
- `"strict": true` — enables all strict type checks (non-negotiable for new projects)
- `"target": "ES2020"` — what JavaScript version to compile to (use ES2020+ for modern environments)
- `"module": "ESNext"` or `"CommonJS"` — module system for output (ESNext for Vite/bundlers, CommonJS for Node.js)
- `"outDir": "./dist"` — where to put compiled JS files
- `"rootDir": "./src"` — where your TypeScript source files are

**What `strict` actually enables:**
`"strict": true` turns on a family of checks:
- `strictNullChecks` — `null` and `undefined` are not assignable to non-nullable types
- `strictFunctionTypes` — stricter function type variance
- `noImplicitAny` — disallows implicit `any` (must annotate all parameters)
- `strictPropertyInitialization` — class properties must be initialized in constructor
- `useUnknownInCatchVariables` — catch variables are `unknown` not `any`

**A minimal working tsconfig:**
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "lib": ["ES2020", "DOM"]
  },
  "include": ["src"]
}
```

## 2. Code Patterns

**For Vite project:**
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

**For Node.js project:**
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "moduleResolution": "node",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "esModuleInterop": true
  }
}
```

**Checking types without emitting:**
```bash
tsc --noEmit    # type-check only, no output files (used in CI)
tsc --watch     # re-check on file changes
```

## 3. Common Gotchas

**Gotcha 1: `strict: true` is NOT the same as `strictNullChecks: true`**
`strict` is a shorthand for enabling a family of checks. Setting only `strictNullChecks: true` is less strict than `strict: true`. Always use `strict: true`.

**Gotcha 2: `target` vs `lib`**
- `target` controls the output syntax (arrow functions vs `function`)
- `lib` controls which type definitions are available (DOM APIs, ES2020 methods)
- They are independent: `"target": "ES5"` doesn't prevent you from using `"lib": ["ES2020"]`

**Gotcha 3: `module: "ESNext"` requires a bundler**
If you're using Vite or webpack, use `"module": "ESNext"`. If running Node.js directly, use `"module": "CommonJS"`. Mixing these causes runtime errors.

**Gotcha 4: Missing `include` → TypeScript checks all .ts files**
Without `"include"`, TypeScript processes every `.ts` file it finds including `node_modules`. Always specify `"include": ["src"]`.

**Gotcha 5: `esModuleInterop: true` for Node.js**
Without this, importing CommonJS modules with default imports (`import express from 'express'`) fails. Always include it for Node.js projects.

## 4. Active Recall Questions

*(No answers here — answer from memory.)*

**Q1:** What does `"strict": true` enable? Name at least 3 of the sub-checks it activates.

**Q2:** You are setting up a new Vite + TypeScript project. What should `"module"` and `"moduleResolution"` be set to, and why?

**Q3:** A teammate runs `tsc` and gets hundreds of errors about null not being assignable to string. They suggest disabling `strictNullChecks`. What is the correct response?

## 5. Quick Reference

| Flag | Values | Purpose |
|------|--------|---------|
| `strict` | `true/false` | Enable all strict checks |
| `target` | `ES2020`, `ESNext` | Output syntax version |
| `module` | `ESNext`, `CommonJS` | Output module system |
| `moduleResolution` | `bundler`, `node` | How imports are resolved |
| `outDir` | `"./dist"` | Compiled output location |
| `rootDir` | `"./src"` | Source files location |
| `lib` | `["ES2020", "DOM"]` | Available type definitions |
| `noEmit` | `true/false` | Type-check only, no output |
| `esModuleInterop` | `true` | Default imports for CJS modules |

## 6. Done Criteria

Session complete when you can write a working tsconfig.json from memory for a Vite project AND a Node.js project. Both files must have the correct `module` and `moduleResolution` for their context.
