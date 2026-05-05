# E05 — Generics

**Expert module** | Covers: L100–L117 | ~65 min

---

## 1. Core Mental Model

> "Generic constraints should express the minimum contract the function needs, not the maximum specificity of what you tested with. A generic that's over-constrained to a class is usually a sign the function should accept an interface."

Expert insight: generics are **parametric polymorphism** — they say "this function works the same way regardless of what type T is, as long as T meets these structural constraints." The constraint is a structural type, not a class hierarchy.

---

## 2. Beginner Misunderstanding

> "I made the function generic but I still need to specify the type every time I call it. Generics are supposed to save me typing."

Misunderstanding: generic type inference works when the type can be inferred from arguments. If you're calling `fn<string>()` with no argument, TypeScript can't infer — that's correct behavior. The fix is usually to pass the value as an argument, not annotate the call.

---

## 3. Professional Concern

> "Our generic `Repository<T>` class is over-constrained to `T extends BaseEntity`. Every model must extend BaseEntity to use repositories, even models that don't need database features."

The smell: `extends ClassName` constraint. This couples the generic to an implementation, not a contract. Better: extract an interface `interface Identifiable { id: string }` and constrain to that. Now any object with `id: string` works.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: Generic function infers `unknown` when it should infer the argument type**
```typescript
function identity<T>(value: T): T { return value; }
const result = identity(42);   // T = number ✓
const result2 = identity([]);  // T = never[] — not what you want?
// What should the constraint be to get T = number[] instead of never[]?
```

**Trap 2: `extends` doesn't mean "is a subclass"**
```typescript
function getLength<T extends { length: number }>(value: T): number {
  return value.length;
}
// Can you call this with a string? An array? A custom class without extending anything?
// What does T extends { length: number } actually require?
```

**Trap 3: Generic class vs generic function — different scoping**
```typescript
class Container<T> {
  value: T;
  constructor(val: T) { this.value = val; }
  // Does the method below need its own <U> parameter or can it use T?
  map(fn: (v: T) => ???): Container<???> { ... }
}
// What type does map return? What happens if you want to transform to a different type?
```

---

## 5. Expert Shortcut

**Minimum constraint principle:**
```typescript
// Over-constrained (couples to implementation)
function processItem<T extends ProjectItem>(item: T): string {
  return item.title;
}

// Correctly constrained (structural minimum)
function processItem<T extends { title: string }>(item: T): string {
  return item.title;
}
// Now works with any object that has a title — not just ProjectItem instances
```

**Generic observer pattern (from DnD course project):**
```typescript
type Listener<T> = (items: T[]) => void;

class State<T> {
  protected listeners: Listener<T>[] = [];
  addListener(fn: Listener<T>) { this.listeners.push(fn); }
  protected notify(items: T[]) { this.listeners.forEach(fn => fn(items)); }
}

class ProjectState extends State<Project> {
  // T is now Project — all listeners are (items: Project[]) => void
}
```

**`ReturnType<T>` and `Parameters<T>` for utility:**
```typescript
function createUser(name: string, role: 'admin' | 'viewer'): User { ... }

type UserParams = Parameters<typeof createUser>;  // [string, 'admin' | 'viewer']
type UserReturn = ReturnType<typeof createUser>;  // User
```

---

## 6. Real-world Scenario

You have a `fetch<T>` wrapper:
```typescript
async function fetchJson<T>(url: string): Promise<T> {
  const res = await fetch(url);
  return res.json() as T; // the cast
}
```

The `as T` is a lie — `res.json()` returns `unknown`. The caller writes `fetchJson<User>(url)` and gets a `User` without any validation. Two weeks later, the API changes and you get runtime crashes with no type errors.

How would you redesign this function to provide real type safety? What role does Zod play?

---

## 7. Deliberate Drill

**Constraint:** Write a generic `pipe` function that takes up to 3 unary functions and composes them left-to-right. **No `any`. Types must flow correctly so the output type of fn1 is the input type of fn2.**

```typescript
// pipe(double, toString, addBang)("5") should type-check to string
// pipe(double)(5) should type-check to number
function pipe<A, B>(fn1: (a: A) => B): (a: A) => B;
// ... add overloads for 2 and 3 functions
```

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can write a generic function with a structural constraint (not a class constraint)
- [ ] Understand why `T extends {}` and `T extends object` behave differently
- [ ] Can write a generic class with a type-safe `map` method
- [ ] Know when TypeScript infers a generic vs when you must specify it explicitly

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. Write a generic `first<T>` function that returns the first element of an array. What happens to the return type when the array is empty? How do you represent that in the return type?

2. You want a `pick<T, K>` function that works like `_.pick` but is type-safe. The return type should be `Pick<T, K>`. Write the function signature including the constraint on `K`.

3. The `State<T>` observer pattern in the DnD project uses `protected listeners: Listener<T>[]`. Why is `protected` used instead of `private`? What does this enable in `ProjectState extends State<Project>`?
