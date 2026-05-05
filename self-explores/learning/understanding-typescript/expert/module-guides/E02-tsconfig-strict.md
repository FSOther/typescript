# E02 — tsconfig & Strict Mode

**Expert module** | Covers: L41–L49 | ~60 min

---

## 1. Core Mental Model

> "`strict: true` is not a style preference — it's the minimum viable type safety. Each flag under `strict` catches a distinct class of bugs. Disabling any of them is accepting a category of runtime errors as untraceable."

The expert reads tsconfig not as "compilation settings" but as "the type system's runtime assumptions." `strictNullChecks: false` means "assume nothing is null" — a lie the compiler believes and stops protecting you from.

---

## 2. Beginner Misunderstanding

> "I disabled `strictNullChecks` because the errors were too many. Now TypeScript is less annoying."

What was lost: every function return type now silently includes `| undefined`. Every property access on a potentially-null value compiles. The "annoyance" was TypeScript finding real bugs — now those bugs are invisible runtime errors.

---

## 3. Professional Concern

> "We have a mixed codebase — some files were written with `strict: true`, others without. TypeScript behaves inconsistently across the project."

This happens when teams add `strict` to an existing project and suppress errors with `// @ts-ignore` rather than fixing them. The result: false confidence. Professional approach: migrate incrementally by enabling one strict flag at a time and fixing all its errors before enabling the next.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: Non-null assertion as lazy fix**
```typescript
const el = document.getElementById('root')!;
el.innerHTML = 'hello';
// The ! suppresses the null check. What happens when this runs in a unit test
// environment where document is mocked without that element?
```

**Trap 2: `noImplicitAny` disabled breaks inference**
```typescript
function process(items) {  // items: any — silently
  return items.map(i => i.name);
}
// Why is this dangerous at call sites? What would strict mode require you to write?
```

**Trap 3: `target` vs `lib` confusion**
```json
{ "target": "es5", "lib": ["es2022"] }
// You're using Array.prototype.at(). It compiles. Does it run in IE11?
// What is 'target' for and what is 'lib' for?
```

---

## 5. Expert Shortcut

**Incremental strict migration order** (safest to apply first):
1. `"noImplicitAny": true` — forces all untyped parameters to be explicit
2. `"strictNullChecks": true` — this one causes most errors, fix carefully
3. `"strictFunctionTypes": true` — catches function parameter variance issues
4. `"strict": true` — enables everything; last step

**`paths` alias to avoid `../../../` hell:**
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@app/*": ["src/app/*"],
      "@types/*": ["src/types/*"]
    }
  }
}
```

---

## 6. Real-world Scenario

You join a codebase with `strict: false`. It has 2000 `.ts` files. Your task is to enable `strict: true` by end of quarter. The team is actively shipping features. How do you approach this without blocking development or creating a one-way door?

*(Think through your approach before checking against an expert answer)*

---

## 7. Deliberate Drill

**Constraint:** Given this tsconfig:
```json
{ "strict": false, "noImplicitAny": false, "strictNullChecks": false }
```

Find all the places in this code that would fail under `strict: true`, and fix them **without using `!` or `as any`**:
```typescript
function getUserName(userId) {
  const users = [{ id: 1, name: "Alice" }, { id: 2, name: "Bob" }];
  const user = users.find(u => u.id === userId);
  return user.name.toUpperCase();
}
```

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can name 5 flags enabled by `strict: true` and what each catches
- [ ] Know the difference between `target` and `lib`
- [ ] Can explain why `!` is dangerous (two scenarios where it fails at runtime)
- [ ] Have a strategy for migrating a non-strict codebase

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. `compilerOptions.module = "commonjs"` vs `"es2022"` — what does this control and why does it matter for Node.js vs browser projects?

2. You set `"noEmit": true` in your tsconfig but run `tsc` and nothing is emitted. Your colleague says this means TypeScript is broken. Explain what `noEmit` does and when you'd want it.

3. A library you use has `"skipLibCheck": true` recommended in its docs. What does this flag skip, and what class of errors might you miss by enabling it?
