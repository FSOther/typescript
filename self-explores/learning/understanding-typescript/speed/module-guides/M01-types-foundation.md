---
type: module-guide
mode: speed
module: M01
course_slug: understanding-typescript
lectures: L13-L39
sessions: S01-S02
created: 2026-05-06
---

# M01 — Types Foundation (L13–L39)

## 1. Key Concepts

The TypeScript type system operates **at compile time only** — all type annotations are erased when compiled to JavaScript. Types are assertions to the compiler, not runtime behavior.

**Core types you must know:**
- `string`, `number`, `boolean` — primitives (lowercase, not `String`, `Number`, `Boolean`)
- `string | number` — union type (value can be either)
- `string[]` or `Array<string>` — typed array
- `[string, number]` — tuple (fixed-length, typed by position)
- `enum Direction { Up, Down }` — named constants (compiles to JS object)
- `any` — escape hatch (avoid), removes all type checking
- `unknown` — type-safe unknown value (must narrow before using)
- `never` — type with no values (appears in exhaustive checks and `throw`)
- `null` and `undefined` — with `strictNullChecks: true`, these are distinct types
- `Record<string, T>` — object where keys are strings and values are type T

**Type inference:** TypeScript infers the type from the initial value. `const x = 'hello'` → `x` is type `string`. You only need to annotate when inference is impossible (function parameters, empty variables).

**Type assignment vs inference:**
```typescript
let name: string = 'Alice';  // annotation — redundant (TS infers 'string')
let name = 'Alice';           // inferred — preferred
function greet(name: string) {} // annotation required — TS can't infer param types
```

**Union types and narrowing:**
```typescript
function process(value: string | number) {
  if (typeof value === 'string') {
    // narrowed to string here
    return value.toUpperCase();
  }
  // narrowed to number here
  return value.toFixed(2);
}
```

**unknown vs any:**
```typescript
let userInput: unknown;
userInput = 5;
userInput = 'hello';
// userInput.length; // ERROR — must narrow first
if (typeof userInput === 'string') {
  userInput.length; // OK after narrowing
}

let anything: any;
anything.noSuchMethod(); // No error — any bypasses ALL checks
```

## 2. Code Patterns

**Defining types:**
```typescript
type ID = string | number;
type Point = { x: number; y: number };
type Status = 'active' | 'inactive' | 'pending'; // literal union
```

**Tuple:**
```typescript
type RGB = [number, number, number];
const red: RGB = [255, 0, 0];
const [r, g, b] = red; // destructuring works
```

**Enum vs const object:**
```typescript
// Course teaches enum:
enum Direction { Up = 'UP', Down = 'DOWN' }

// Production preference:
const Direction = { Up: 'UP', Down: 'DOWN' } as const;
type Direction = typeof Direction[keyof typeof Direction];
```

**Nullable patterns:**
```typescript
function findUser(id: string): User | null {
  return db.get(id) ?? null;
}

const user = findUser('123');
if (user !== null) {
  console.log(user.name); // safe access
}
// OR
console.log(user?.name); // optional chaining
```

## 3. Common Gotchas

**Gotcha 1: Type annotation vs type assertion**
`let x: string` is a type annotation (compiler checks it).
`let x = y as string` is a type assertion (you override the compiler — dangerous on external data).

**Gotcha 2: `enum` compiles to real JavaScript**
Unlike type aliases and interfaces (which are erased), `enum` compiles to a JavaScript object with reverse mappings. This adds bytes to your bundle and can't be tree-shaken easily. Use `as const` objects instead.

**Gotcha 3: `number[]` vs `Array<number>`**
These are identical. `number[]` is shorthand syntax. `Array<number>` is generic syntax. Prefer `number[]` for simple cases, `Array<T>` when using a complex generic type.

**Gotcha 4: `null` and `undefined` are NOT the same**
With `strictNullChecks: true`:
- `null` = intentional absence of value
- `undefined` = value not yet assigned
`value === null` and `value === undefined` are different checks. Use `value == null` (loose equality) to check for both at once.

**Gotcha 5: Type inference with `const` vs `let`**
```typescript
const x = 'hello'; // type is 'hello' (literal type)
let y = 'hello';   // type is 'string' (widened)
```

## 4. Active Recall Questions

*(No answers here — answer from memory.)*

**Q1:** What is the difference between `any` and `unknown`? When should you use each?

**Q2:** You have a function `function getId(): string | number`. Inside the function body, before returning, how do you check which type the value is at runtime?

**Q3:** Explain the difference between a tuple `[string, number]` and an array `(string | number)[]`. When would you choose tuple over array?

## 5. Quick Reference

| Syntax | Meaning |
|--------|---------|
| `x: string` | x must be a string |
| `x: string \| number` | x can be string or number |
| `x?: string` | x is optional (string or undefined) |
| `x!` | assert x is not null/undefined (use carefully) |
| `x as string` | type assertion (override compiler) |
| `typeof x === 'string'` | runtime type narrowing |
| `x instanceof MyClass` | class instance narrowing |

## 6. Done Criteria

Session is complete when you can answer all 3 Active Recall questions without looking at notes. Specifically:
- Name the difference between `any` and `unknown` in one sentence
- Write a union type guard function from memory
- Explain when to use tuple vs array in one sentence
