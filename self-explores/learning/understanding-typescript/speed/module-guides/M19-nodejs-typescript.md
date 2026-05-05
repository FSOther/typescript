# M19 — Node.js + TypeScript (L246–L257)

**Speed module** | Lectures: 246–257 | ~40 min

---

## 1. Key Concepts

| Concept | One-liner |
|---------|-----------|
| `@types/node` | Type declarations for Node.js built-ins (fs, path, http, etc.) |
| `@types/express` | Types for Express.js; match major version to installed express |
| Compile-then-run | Classic workflow: `tsc` → `node dist/app.js` |
| Native TS support | Node.js ≥22.6 can run `.ts` directly via `--experimental-strip-types` |
| Type stripping | Removes TS annotations at runtime (no enums, no decorators) |
| Type transformation | Full compilation to JS (needed for enums, decorators) |
| Bun | Alternative runtime with native TypeScript support (no Node.js required) |
| `import type` | Required for Node.js native TS mode when importing interfaces/types |

---

## 2. Code Patterns

### Classic Express + TypeScript setup
```bash
npm install express
npm install --save-dev typescript @types/node @types/express ts-node
npx tsc --init   # generates tsconfig.json
```

```json
// tsconfig.json for Node.js
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "moduleResolution": "node",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true
  }
}
```

```typescript
// src/app.ts
import express, { Request, Response, NextFunction } from 'express';

const app = express();
app.use(express.json());

app.get('/', (req: Request, res: Response) => {
  res.json({ message: 'Hello World' });
});

app.listen(3000, () => console.log('Running on :3000'));
```

```bash
# package.json scripts
"build": "tsc"
"start": "node dist/app.js"
"dev": "ts-node src/app.ts"   # or nodemon with ts-node
```

### Typed route handlers
```typescript
interface Todo {
  id: string;
  text: string;
  completed: boolean;
}

const todos: Todo[] = [];

app.post('/todos', (req: Request, res: Response) => {
  const { text } = req.body as { text: string }; // body is 'any' by default
  const todo: Todo = { id: Date.now().toString(), text, completed: false };
  todos.push(todo);
  res.status(201).json(todo);
});
```

### Native Node.js TS (v22.6+) — no compilation step
```bash
# No build step needed
node --experimental-strip-types src/app.ts
```

```typescript
// IMPORTANT: use 'import type' for type-only imports
import express from 'express';
import type { Request, Response } from 'express'; // 'type' required in native mode

// Enums NOT supported in native mode (need transformation, not just stripping)
// Use const objects instead:
const HttpStatus = { OK: 200, NOT_FOUND: 404 } as const;
type HttpStatus = typeof HttpStatus[keyof typeof HttpStatus];
```

### Bun runtime
```bash
bun init           # creates project with tsconfig
bun run app.ts     # runs TS directly, no config needed
bun build app.ts --outdir dist  # bundle
```

---

## 3. Common Gotchas

| Gotcha | Detail |
|--------|--------|
| `req.body` is `any` | Express doesn't know your request shape; cast or validate with Zod |
| `@types/express` version | `express@4` → `@types/express@4`; mismatched versions break types |
| Native mode rejects `enum` | Enums require transformation (JS equivalent); use `as const` objects instead |
| `import type` in native mode | Mandatory for interfaces/types; Node.js can't strip what it doesn't recognize as type-only |
| `ts-node` vs native | `ts-node` is dev convenience; for production, always compile first |
| CommonJS vs ESM in Node | `"module": "commonjs"` → `require`; `"module": "nodenext"` → `import` with `.js` extensions |

---

## 4. Active Recall

> **No answers here.** Close this file and answer from memory.

1. You install `express` in a new TypeScript project but get *"Could not find a declaration file for module 'express'"*. What two packages must you install, and how?

2. Your team wants to use Node.js 23 to run TypeScript files directly without a build step. You have `enum Status { Active, Finished }` in your code. What error will you get, and what's the fix?

3. A route handler receives a POST request. `req.body.email` is typed as `any`. You want to ensure `email` is a valid string at runtime. Which two approaches can you use?

---

## 5. Quick Reference

```bash
# Install Node.js types
npm install --save-dev @types/node @types/express

# Compile and run (classic)
npx tsc && node dist/app.js

# Native TS (Node.js 22.6+)
node --experimental-strip-types app.ts

# Bun
bun run app.ts

# ts-node (dev only)
npx ts-node src/app.ts
```

```typescript
// Request body typing (Express)
req.body as { field: string }       // simple cast
const data = RequestSchema.parse(req.body) // Zod validation (safer)

// Enum alternative for native Node.js
const Status = { Active: 0, Finished: 1 } as const;
type Status = typeof Status[keyof typeof Status];
```

---

## 6. Done Criteria

- [ ] Can set up a typed Express app from scratch (deps, tsconfig, basic route)
- [ ] Know when to use `import type` and why it matters for native Node.js TS
- [ ] Understand the difference between type stripping and type transformation
- [ ] Know at least two alternatives to `const enum` when compilation isn't available
