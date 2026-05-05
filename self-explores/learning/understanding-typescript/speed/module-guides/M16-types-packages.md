# M16 — Third-Party Types & Zod (L214–L223)

**Speed module** | Lectures: 214–223 | ~40 min

---

## 1. Key Concepts

| Concept | One-liner |
|---------|-----------|
| DefinitelyTyped | Community repo of `.d.ts` files for JS-only libraries; installed via `@types/xxx` |
| `.d.ts` file | Type declarations only — no runtime code; tells TypeScript what a module exports |
| `declare` keyword | Tells TS "this exists at runtime, I promise" — for globals not in type files |
| Library with built-in types | Modern libraries (Zod, Prisma, tRPC) ship their own `.d.ts` — no `@types/` needed |
| Zod | Runtime schema validation library; infers TypeScript types from schemas |
| `z.infer<typeof schema>` | Extracts the TypeScript type from a Zod schema |
| Runtime vs compile-time | TypeScript types are erased; Zod validates actual values at runtime |

---

## 2. Code Patterns

### Installing and using @types packages
```bash
# JavaScript-only library with no built-in types
npm install lodash
npm install --save-dev @types/lodash  # installs from DefinitelyTyped

# Now TypeScript understands lodash
import _ from 'lodash';
const chunks = _.chunk([1, 2, 3, 4], 2); // fully typed
```

### Reading a .d.ts file
```typescript
// node_modules/@types/lodash/index.d.ts (excerpt)
export function chunk<T>(array: ArrayLike<T> | null | undefined, size?: number): T[][];
// No implementation — just the shape
```

### Using `declare` for globals
```typescript
// Globals injected by a script tag (e.g., Google Maps)
declare var google: any;
declare function initMap(): void;
// Tells TS these exist; use sparingly — prefer @types
```

### Zod: define schema → get type
```bash
npm install zod  # no @types needed — Zod ships its own types
```
```typescript
import { z } from 'zod';

const UserSchema = z.object({
  id: z.number(),
  name: z.string().min(1),
  email: z.string().email(),
  roles: z.array(z.enum(['admin', 'viewer'])),
});

type User = z.infer<typeof UserSchema>; // TypeScript type from schema

// Validate at runtime (e.g., API response, JSON file)
const rawData = JSON.parse(response.body);
const user = UserSchema.parse(rawData); // throws ZodError if invalid
// user is now typed as User
```

### Zod safe parse (non-throwing)
```typescript
const result = UserSchema.safeParse(rawData);
if (result.success) {
  console.log(result.data.name); // typed
} else {
  console.error(result.error.flatten());
}
```

### Zod: runtime vs compile-time gap
```typescript
// TypeScript alone — only compile-time
function greet(user: User) { ... } // no guarantee user is valid at runtime

// With Zod — runtime validation bridges the gap
function processApiUser(raw: unknown): User {
  return UserSchema.parse(raw); // throws if shape is wrong
}
```

---

## 3. Common Gotchas

| Gotcha | Detail |
|--------|--------|
| Version mismatch | `@types/express` version must match `express` major version |
| No `@types` needed for modern libs | Zod, Prisma, tRPC include types — installing `@types/zod` installs wrong package |
| `declare` is a promise | If the global doesn't exist at runtime, TypeScript won't catch it |
| `.d.ts` in `src/` | Custom `.d.ts` files (e.g., for SVG imports in Vite) must be included in tsconfig `include` |
| `z.infer` returns the TS type | But Zod's schemas add overhead — only validate at boundaries, not deep in business logic |

---

## 4. Active Recall

> **No answers here.** Close this file and answer from memory.

1. You install `axios`. The import has no type errors. Then you install `lodash` and get: *"Could not find a declaration file for module 'lodash'"*. Why does axios work but lodash doesn't? What command fixes lodash?

2. What is the fundamental difference between TypeScript's type system and Zod? In what scenario does TypeScript fail but Zod saves you?

3. You define a Zod schema `const PostSchema = z.object({ title: z.string(), views: z.number() })`. Write the TypeScript type and a function that validates unknown API data against it.

---

## 5. Quick Reference

```bash
# Install types for JS library
npm install --save-dev @types/library-name

# Install Zod (no @types needed)
npm install zod
```

```typescript
// .d.ts minimal example
declare module 'some-lib' {
  export function doThing(x: string): number;
}

// Zod cheatsheet
z.string().min(1).max(100)
z.number().int().positive()
z.boolean()
z.array(z.string())
z.object({ key: z.string() })
z.union([z.string(), z.number()])
z.enum(['a', 'b', 'c'])
z.optional(z.string())       // string | undefined
z.nullable(z.string())       // string | null

type T = z.infer<typeof MySchema>  // extract TS type
schema.parse(data)                 // throws on failure
schema.safeParse(data)             // { success, data | error }
```

---

## 6. Done Criteria

- [ ] Know when to install `@types/xxx` vs when it's unnecessary
- [ ] Understand what a `.d.ts` file contains and how to write a minimal one
- [ ] Can define a Zod schema and extract its TypeScript type
- [ ] Can explain the runtime vs compile-time gap that Zod bridges
