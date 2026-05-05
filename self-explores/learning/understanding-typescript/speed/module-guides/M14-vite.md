# M14 — Vite + Build Tools (L194–L201)

**Speed module** | Lectures: 194–201 | ~25 min

---

## 1. Key Concepts

| Concept | One-liner |
|---------|-----------|
| Vite | Modern build tool using ESBuild/Rollup under the hood; handles TS, CSS, assets |
| ESBuild | Ultra-fast bundler (Vite's dev server); does NOT type-check |
| `tsc --noEmit` | Type-check without emitting JS; combine with Vite build for CI |
| `vite-env.d.ts` | Auto-generated ambient declarations (`/// <reference types="vite/client" />`) |
| `isolatedModules: true` | Required by Vite; each file transpiled independently — no cross-file inference |
| `npm create vite@latest` | Scaffold a new Vite project interactively |
| `.d.ts` files | Type declarations without runnable code |

---

## 2. Code Patterns

### Scaffold a Vite + TypeScript project
```bash
npm create vite@latest my-app
# Choose: Vanilla → TypeScript
cd my-app && npm install
npm run dev      # dev server (hot reload)
npm run build    # production bundle
npm run preview  # serve built output
```

### Typical Vite tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "noEmit": true,
    "strict": true,
    "skipLibCheck": true
  }
}
```

### vite-env.d.ts (auto-generated, don't delete)
```typescript
/// <reference types="vite/client" />
// This gives you import.meta.env types, asset imports, etc.
```

### Importing static assets (Vite feature)
```typescript
import logoUrl from './logo.png'; // string URL
import styles from './module.css'; // CSS modules (if configured)
```

### Type-check in CI (separate from Vite build)
```bash
# package.json scripts
"build": "tsc && vite build"
# tsc --noEmit for type checking only, vite build for bundling
```

---

## 3. Common Gotchas

| Gotcha | Detail |
|--------|--------|
| Vite doesn't type-check | `npm run dev` succeeds even with type errors; run `tsc --noEmit` separately |
| `isolatedModules` breaks `const enum` | Use regular `enum` or `declare const enum` |
| `allowImportingTsExtensions` + `noEmit` | Required together; you can't emit when importing `.ts` directly |
| Import paths in Vite | Use `/src/...` or relative paths; no `baseUrl` needed by default |
| `.d.ts` not `.ts` for Webpack | Webpack legacy setup requires separate loader config; Vite handles it natively |

---

## 4. Active Recall

> **No answers here.** Close this file and answer from memory.

1. You run `npm run dev` and the app loads fine in the browser, but you know you added a type error. Why doesn't Vite catch it, and what command should you add to your CI pipeline?

2. A teammate deleted `vite-env.d.ts`. What breaks, and why?

3. You're migrating from Webpack to Vite. Your existing code uses `const enum Direction { Up, Down }` throughout. The Vite build fails. Why, and what are your two options to fix it?

---

## 5. Quick Reference

```bash
npm create vite@latest          # scaffold new project
npm run dev                     # dev server
npm run build                   # production: tsc + vite build
tsc --noEmit                    # type check only (no emit)

# Key tsconfig flags for Vite
"isolatedModules": true         # required by Vite
"noEmit": true                  # let Vite handle emit
"moduleResolution": "bundler"   # modern resolution for Vite
```

---

## 6. Done Criteria

- [ ] Know why Vite doesn't type-check and how to add type checking
- [ ] Understand `isolatedModules: true` implication for `const enum`
- [ ] Know what `vite-env.d.ts` does and why not to delete it
- [ ] Can scaffold a Vanilla TS project with Vite from memory
