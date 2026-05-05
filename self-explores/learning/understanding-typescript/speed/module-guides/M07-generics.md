---
type: module-guide
mode: speed
module: M07
course_slug: understanding-typescript
lectures: L100-L109
sessions: S07
created: 2026-05-06
---

# M07 — Generics (L100–L109)

## 1. Key Concepts

Generics make functions and classes work with multiple types while preserving type information. Every React component, every Express middleware, every array method uses generics.

**The core idea:** Instead of `string | number`, use a type parameter `<T>` that captures WHICH specific type was used.

```typescript
// Without generics — loses type info
function identity(x: string | number): string | number { return x; }
const result = identity('hello'); // result is string | number, not string

// With generics — preserves type info
function identity<T>(x: T): T { return x; }
const result = identity('hello'); // result is string ✓
```

**Generic constraints:** Use `extends` to limit what `T` can be.
```typescript
function longest<T extends { length: number }>(a: T, b: T): T {
  return a.length >= b.length ? a : b;
}
// T must have a .length property — works for string, array, etc.
```

**Generic interfaces and classes:**
```typescript
interface Repository<T> {
  findById(id: string): T | null;
  save(item: T): void;
}

class UserRepository implements Repository<User> {
  findById(id: string): User | null { ... }
  save(user: User): void { ... }
}
```

**Multiple type parameters:**
```typescript
function merge<T extends object, U extends object>(a: T, b: U): T & U {
  return { ...a, ...b };
}
```

## 2. Code Patterns

**Generic observer (from DnD project):**
```typescript
type Listener<T> = (items: T[]) => void;

class State<T> {
  protected listeners: Listener<T>[] = [];
  
  addListener(listenerFn: Listener<T>) {
    this.listeners.push(listenerFn);
  }
}

class ProjectState extends State<Project> {
  // listeners is Listener<Project>[] — type-safe
}
```

**Generic function with constraint:**
```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { name: 'Alice', age: 30 };
const name = getProperty(user, 'name'); // type is string
const age = getProperty(user, 'age');   // type is number
// getProperty(user, 'xyz'); // ERROR — 'xyz' not a key of user
```

**Generic React component (preview):**
```typescript
interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
}

function List<T>({ items, renderItem }: ListProps<T>) {
  return <ul>{items.map(renderItem)}</ul>;
}
```

## 3. Common Gotchas

**Gotcha 1: Constraint too narrow**
`<T extends MyClass>` means ONLY MyClass instances work. If you only need `.length`, use `<T extends { length: number }>`. The constraint should be the MINIMUM interface your code uses.

**Gotcha 2: `T extends object` vs `T extends Record<string, unknown>`**
`object` includes arrays, functions, classes. `Record<string, unknown>` is a plain key-value object. Be specific.

**Gotcha 3: Type inference works unless types are unrelated**
TypeScript infers `T` from the call site. But if you have `<T>(a: T, b: T)` and call it with `(1, 'hello')`, TypeScript infers `T = string | number`. This may not be what you want — use two type parameters: `<T, U>(a: T, b: U)`.

**Gotcha 4: Generic classes require type annotation or inference at construction**
```typescript
class Stack<T> { ... }
const s = new Stack<string>(); // explicit
const s2 = new Stack();        // T = unknown without inference
```

## 4. Active Recall Questions

*(No answers here — answer from memory.)*

**Q1:** What is the difference between `<T>(x: T): T` and `(x: unknown): unknown`? Why use generics instead of `unknown`?

**Q2:** Write a generic function `getFirstItem<T>(arr: T[]): T | null` from memory.

**Q3:** What does `<T extends { id: string }>` mean? Write an example of a function that uses this constraint and explain what it guarantees.

## 5. Quick Reference

| Syntax | Meaning |
|--------|---------|
| `<T>` | Generic type parameter (placeholder) |
| `<T extends X>` | T must satisfy X (be assignable to X) |
| `<T extends keyof U>` | T must be a key of U |
| `T[K]` | Type of property K in T |
| `Listener<T>` | Generic type alias |
| `class X<T>` | Generic class |
| `interface I<T>` | Generic interface |

## 6. Done Criteria

Session complete when you can:
1. Write a generic function with a constraint from memory
2. Explain why `<T>(x: T): T` is better than `(x: unknown): unknown`
3. Write the `State<T>` class with generic `Listener<T>` type from memory
