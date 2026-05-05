---
type: module-guide
mode: speed
module: M10
course_slug: understanding-typescript
lectures: L133-L145
sessions: S09
created: 2026-05-06
---

# M10 — ECMAScript Decorators (L133–L145)

## 1. Key Concepts

ECMAScript decorators (2023 spec, TypeScript 5.0+) are functions that modify or observe class elements. They are NOT experimental decorators — see M11 for those.

**When to use ECMAScript decorators (this module):**
- New code, new projects
- When `experimentalDecorators` is NOT in tsconfig (or is `false`)
- Angular 17+, frameworks adopting the new spec

**Decorator types:**
- Class decorators — applied to the class itself
- Method decorators — applied to a method
- Accessor decorators — applied to `get`/`set` accessors
- Field decorators — applied to class fields
- Auto-accessor decorators — applied to `accessor` fields (new syntax)

**Execution order:**
Decorators run AFTER the class is defined. Class decorator runs last (outermost).

**Basic class decorator:**
```typescript
function sealed(target: typeof MyClass, context: ClassDecoratorContext) {
  Object.seal(target);
  Object.seal(target.prototype);
}

@sealed
class MyClass { ... }
```

**Method decorator:**
```typescript
function log(target: Function, context: ClassMethodDecoratorContext) {
  return function(this: any, ...args: any[]) {
    console.log(`Calling ${context.name as string}`);
    return target.call(this, ...args);
  };
}

class MyService {
  @log
  doSomething() { ... }
}
```

## 2. Code Patterns

**`@autobind` equivalent with ECMAScript decorators:**
```typescript
function autobind<T extends (this: any, ...args: any[]) => any>(
  target: T,
  context: ClassMethodDecoratorContext
): T {
  context.addInitializer(function(this: any) {
    this[context.name] = target.bind(this);
  });
  return target;
}

class Button {
  @autobind
  handleClick(event: Event) {
    console.log(this); // 'this' is always the Button instance
  }
}
```

**Decorator factory (returns a decorator):**
```typescript
function min(minimum: number) {
  return function validate(
    target: Function,
    context: ClassMethodDecoratorContext
  ) {
    return function(this: any, value: number) {
      if (value < minimum) throw new Error(`Must be >= ${minimum}`);
      return target.call(this, value);
    };
  };
}

class Cart {
  @min(1)
  setQuantity(qty: number) { ... }
}
```

## 3. Common Gotchas

**Gotcha 1: ECMAScript vs Experimental — completely different signatures**
ECMAScript method decorator: `(target: Function, context: ClassMethodDecoratorContext) => Function | void`
Experimental method decorator: `(target: any, propertyKey: string, descriptor: PropertyDescriptor) => PropertyDescriptor | void`
They are NOT compatible. Don't mix.

**Gotcha 2: ECMAScript decorators need `addInitializer` for `this` binding**
Unlike experimental decorators, ECMAScript method decorators don't have direct access to the instance at decoration time. Use `context.addInitializer` to run code during class instantiation.

**Gotcha 3: Decorator execution order — bottom to top**
When multiple decorators are stacked, the INNERMOST (closest to the method) runs first, outermost last.
```typescript
@decoratorA  // runs second
@decoratorB  // runs first
method() {}
```

**Gotcha 4: `tsconfig.json` — no flag needed for ECMAScript decorators**
ECMAScript decorators (2023 spec) work without `experimentalDecorators: true`. If you see that flag, you're using the OLD experimental spec (M11).

## 4. Active Recall Questions

*(No answers here — answer from memory.)*

**Q1:** What is the difference between ECMAScript decorators (M10) and experimental decorators (M11)? When should you use each?

**Q2:** Write a simple ECMAScript class decorator `@sealed` that calls `Object.seal` on the class and its prototype.

**Q3:** What is `context.addInitializer` used for in ECMAScript method decorators? Why can't you access `this` directly in the decorator function?

## 5. Quick Reference

| Decorator Type | Context Type |
|---------------|-------------|
| Class | `ClassDecoratorContext` |
| Method | `ClassMethodDecoratorContext` |
| Field | `ClassFieldDecoratorContext` |
| Accessor (get/set) | `ClassAccessorDecoratorContext` |
| Auto-accessor | `ClassAccessorDecoratorContext` |

**Key context properties:**
- `context.name` — name of the decorated element
- `context.kind` — 'class', 'method', 'field', 'accessor'
- `context.addInitializer(fn)` — run `fn` during instance initialization
- `context.static` — boolean, is this a static member?

## 6. Done Criteria

Session complete when you can:
1. Explain the key difference between ECMAScript and experimental decorators
2. Write a class decorator from memory using the ECMAScript signature
3. Explain why `addInitializer` is needed for binding `this` in method decorators
