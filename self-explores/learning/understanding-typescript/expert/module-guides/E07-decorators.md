# E07 — Decorators (ECMAScript + Experimental)

**Expert module** | Covers: L133–L161 | ~60 min

---

## 1. Core Mental Model

> "ECMAScript decorators (2023 spec) are wrappers applied at class element initialization — not compile-time macros. They add behavior by replacing or augmenting the decorated element. The spec is now stable; experimental decorators are legacy."

Expert distinction: experimental decorators (`experimentalDecorators: true`) use a different execution model and are still used by NestJS (v9 and below). ECMAScript decorators use `context.addInitializer` and `context.name` — a clean, spec-compliant API.

---

## 2. Beginner Misunderstanding

> "Decorators are magic — I just put `@Something` on my class and it works. I don't need to understand what's happening."

What's actually happening: a decorator is a function. It receives the decorated target (class, method, field, or accessor) plus a context object. It optionally returns a replacement for that target. There is no magic — just higher-order functions with a special syntax.

---

## 3. Professional Concern

> "We're using NestJS with `experimentalDecorators` everywhere. Now our company policy requires moving to standard ECMAScript decorators. What breaks?"

Answer: NestJS's `@Module`, `@Injectable`, `@Controller` decorators rely on TypeScript metadata reflection (`emitDecoratorMetadata`), which only works with experimental decorators. The ECMAScript spec doesn't include metadata reflection. This is a real migration challenge — NestJS is working on a Reflect-free approach.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: Experimental decorator `this` binding in method decorator**
```typescript
function Log(target: any, key: string, desc: PropertyDescriptor) {
  const original = desc.value;
  desc.value = function(...args: any[]) {
    console.log(`Calling ${key}`);
    return original.apply(this, args); // 'this' — what is it here?
  };
  return desc;
}
// What value does 'this' have when the wrapped function is called?
// What breaks if the method is passed as a callback?
```

**Trap 2: ECMAScript decorator evaluation order**
```typescript
@A @B @C
class MyClass {}
// In what order are A, B, C applied?
// Does A receive the original class or the B-decorated class?
```

**Trap 3: Decorator on static vs instance method**
```typescript
class Greeter {
  @autobind
  greet(name: string) { return `Hello ${name}, I am ${this.name}`; }
  
  @autobind
  static create() { return new Greeter(); }
}
// For static method: what is 'this' inside the decorator? For instance method?
// Do both need the same autobind implementation, or different?
```

---

## 5. Expert Shortcut

**ECMAScript `@autobind` (spec-compliant, no `experimentalDecorators`):**
```typescript
function autobind<T extends (...args: unknown[]) => unknown>(
  method: T,
  context: ClassMethodDecoratorContext<object, T>
) {
  context.addInitializer(function (this: object) {
    (this as Record<string, unknown>)[context.name as string] =
      method.bind(this);
  });
}

class Button {
  @autobind
  handleClick(event: MouseEvent) {
    // 'this' is always the Button instance
  }
}
```

**Decorator factory pattern:**
```typescript
function Logger(prefix: string) {
  return function <T extends new (...args: unknown[]) => object>(
    target: T,
    _context: ClassDecoratorContext
  ) {
    return class extends target {
      constructor(...args: unknown[]) {
        super(...args);
        console.log(`${prefix}: ${target.name} created`);
      }
    };
  };
}

@Logger('DEBUG')
class ProductService { ... }
```

---

## 6. Real-world Scenario

You have a `@memoize` decorator for expensive getter methods. A colleague wants to use it on an instance method that takes arguments. The current implementation only works on zero-arg methods.

Design the `@memoize(keyFn)` decorator factory where `keyFn` converts the method arguments to a cache key string. Consider: what type should the generic parameter be?

---

## 7. Deliberate Drill

**Constraint:** Implement `@validate` field decorator (ECMAScript spec) that:
1. Accepts a validator function as argument: `(value: unknown) => boolean`
2. Throws on construction if the initial value fails validation
3. Uses `context.addInitializer`, NOT `Object.defineProperty`

```typescript
@validate(v => typeof v === 'string' && v.length > 0)
name: string;
```

---

## 8. Expert AC

Before moving on, verify:
- [ ] Can explain the difference between experimental and ECMAScript decorators in one paragraph
- [ ] Know what `context.addInitializer` does and when it runs
- [ ] Can implement `@autobind` using the ECMAScript spec
- [ ] Understand why NestJS still uses experimental decorators

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. What does `experimentalDecorators: true` enable that standard TypeScript doesn't support? Name one feature that only works with this flag.

2. In the ECMAScript decorator spec, a class method decorator returns either `undefined` or a replacement function. What happens if you return `undefined`? What's the contract?

3. The course shows `@autobind` solving the `this` problem for event handlers. What is the underlying JavaScript problem `@autobind` solves, and why does passing a method as `onClick={this.handleClick}` break `this`?
