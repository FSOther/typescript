# P02 — tsconfig from Scratch

**Practice module** | Mastery target: L2→L3 | ~60 min

---

## 1. Build Task

> **No solution provided.** Build from memory.

**Objective:** Write a `tsconfig.json` for each scenario from memory, then verify with `tsc --noEmit`.

**Scenario A — Browser app (Vite/ESBuild):**
Requirements:
- Output ECMAScript 2020 (modern browsers)
- ES module output (`import`/`export` syntax in output)
- Include DOM types
- Strict mode enabled
- No file emission (Vite handles bundling)
- Source maps enabled
- Root files in `src/`, exclude `node_modules`

**Scenario B — Node.js Express API:**
Requirements:
- Output CommonJS modules (`require`/`module.exports`)
- Target Node.js 18 (ES2022 features)
- No DOM types (server-side only)
- Strict mode
- Emit to `dist/` from `src/`
- Enable decorator support (`experimentalDecorators`)

**Scenario C — Library (published to npm):**
Requirements:
- Emit `.d.ts` declaration files
- Emit source maps for debugging
- Target ES2018 (wide compatibility)
- ES module output
- `rootDir: src`, `outDir: dist`
- `declaration: true`, `declarationMap: true`

Write each tsconfig.json without looking at docs. Then run `tsc --noEmit` on a sample `.ts` file and fix any errors.

---

## 2. Break It

Take Scenario B's tsconfig and introduce each of these:

1. Set `"module": "es2022"` — then run `node dist/app.js`. What happens at runtime?

2. Set `"target": "es5"` while keeping `"lib": ["ES2022", "dom"]` — does `Array.prototype.at(-1)` still compile? Does it run on Node.js 10?

3. Remove `"outDir"` — where does `tsc` emit files? What's the problem?

4. Set `"strict": false, "noImplicitAny": true` — what still gets checked? What doesn't?

---

## 3. Fix It

> **Hints only — no solution shown.**

This tsconfig has 4 bugs. Fix them using error messages:

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "commonjs",
    "lib": ["ES2022"],
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": "yes",
    "skipLibCheck": "false"
  },
  "include": "src/**/*.ts"
}
```

**Hint 1:** Look for fields where strings are used where booleans are required.

**Hint 2:** `include` expects an array.

**Hint 3:** Check the valid values for `lib` — is `"ES2022"` alone correct for a Node.js project, or are you missing something?

---

## 4. Explain It

1. What does `"target"` control vs `"lib"`? Give an example where they'd be set differently and why.

2. What is `"noEmit"` for? In what project setup would you always want it?

3. Explain `"skipLibCheck"`. What types of errors does it skip? What hidden bugs might this hide?

---

## 5. Apply It

For a new project: a CLI tool using Node.js 20, TypeScript 5, with one dependency (`chalk`). Write the complete `tsconfig.json`. Then:
1. Create `src/index.ts` with one function
2. Run `tsc`
3. Verify the `dist/` output has `.js` and `.d.ts` files

---

## 6. Acceptance Criteria

```
Given: Scenario A tsconfig
When: tsc --noEmit runs on a file using document.getElementById
Then: no type error (DOM lib is included)

Given: Scenario B tsconfig
When: tsc runs
Then: output files appear in dist/ as CommonJS (require syntax)

Given: the broken tsconfig
When: tsc runs
Then: at least 2 error messages appear; fixing all of them makes tsc pass
```

---

## 7. Resources

- Speed guide: [`M02-tsconfig.md`](../../../speed/module-guides/M02-tsconfig.md)
- Reference lectures: L41–L49
