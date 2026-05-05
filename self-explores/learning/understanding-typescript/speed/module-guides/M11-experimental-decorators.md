---
type: module-guide
mode: speed
module: M11
course_slug: understanding-typescript
lectures: L146-L160
sessions: S10
created: 2026-05-06
---

# M11 — Experimental Decorators (L146–L160)

## 1. Key Concepts

Experimental decorators (`experimentalDecorators: true`) are the OLDER decorator spec still used in many TypeScript codebases (Angular pre-v17, NestJS, TypeORM, class-validator). You need to understand them to read existing code.

**Requires in tsconfig:**
```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true  // needed for DI frameworks like NestJS
  }
}
```

**Decorator signatures (different from ECMAScript!):**

Class decorator:
```typescript
function MyClassDec(constructor: Function) { ... }
```

Method decorator:
```typescript
function MyMethodDec(
  target: any,            // class prototype
  propertyKey: string,   // method name
  descriptor: PropertyDescriptor  // method descriptor
): PropertyDescriptor | void { ... }
```

Property decorator:
```typescript
function MyPropDec(target: any, propertyKey: string): void { ... }
```

Parameter decorator:
```typescript
function MyParamDec(target: any, propertyKey: string, parameterIndex: number): void { ... }
```

## 2. Code Patterns

**`@autobind` (experimental decorator):**
```typescript
function autobind(
  _: any,
  _2: string,
  descriptor: PropertyDescriptor
): PropertyDescriptor {
  const originalMethod = descriptor.value;
  const adjDescriptor: PropertyDescriptor = {
    configurable: true,
    get() {
      const boundFn = originalMethod.bind(this);
      return boundFn;
    }
  };
  return adjDescriptor;
}
```
This is EXACTLY the `@autobind` from `resources/156-decorators-09-example-autobind/`.

**Class decorator that returns a new class:**
```typescript
function AddTimestamp<T extends new (...args: any[]) => object>(constructor: T) {
  return class extends constructor {
    createdAt = new Date();
  };
}

@AddTimestamp
class User { name: string; constructor(name: string) { this.name = name; } }
const u = new User('Alice'); 
(u as any).createdAt; // Date — added by decorator
```

**Decorator factory:**
```typescript
function Required(target: any, propertyKey: string) {
  // Store metadata that this property is required
  // (uses reflect-metadata in full implementations)
}
```

## 3. Common Gotchas

**Gotcha 1: `PropertyDescriptor.get()` vs `.value`**
For `@autobind`, we use `get()` instead of modifying `value` directly. The `get()` accessor runs on each access, ensuring `this` is always the current instance. If you set `value` directly, the binding is per-prototype (shared across instances).

**Gotcha 2: First two parameters are usually unused**
Method decorators receive `(target, propertyKey, descriptor)`. In `@autobind`, target and propertyKey are not used (convention: name them `_` and `_2`). Don't be confused when you see leading underscores.

**Gotcha 3: Decorators run at CLASS DEFINITION time, not at CALL time**
```typescript
@log // runs immediately when the class is parsed
class Service { }
// NOT when Service is instantiated
```
This means decorators cannot use values that are only available at runtime.

**Gotcha 4: NestJS uses experimental decorators**
`@Injectable()`, `@Controller()`, `@Get()` in NestJS are all experimental decorators. Even in 2025, NestJS has not fully migrated to the ECMAScript spec. If you see `emitDecoratorMetadata: true` in a project, it's using experimental decorators.

## 4. Active Recall Questions

*(No answers here — answer from memory.)*

**Q1:** Write the `@autobind` experimental decorator from memory. What does `PropertyDescriptor.get()` do differently than just replacing `value`?

**Q2:** What configuration must be in `tsconfig.json` to enable experimental decorators? What is `emitDecoratorMetadata` used for?

**Q3:** What is the execution order of decorators when you stack multiple on the same class or method?

## 5. Quick Reference

**Experimental decorator signatures:**

| Type | Parameters |
|------|-----------|
| Class | `(constructor: Function)` |
| Method | `(target, propertyKey, descriptor)` |
| Property | `(target, propertyKey)` |
| Parameter | `(target, propertyKey, parameterIndex)` |
| Accessor | `(target, propertyKey, descriptor)` |

**tsconfig flags:**
- `experimentalDecorators: true` — enables experimental decorator syntax
- `emitDecoratorMetadata: true` — emits runtime type metadata (needed for NestJS, TypeORM DI)

## 6. Done Criteria

Session complete when you can write `@autobind` from memory with the correct experimental decorator signature, and explain:
1. Why `get()` is used instead of replacing `value`
2. The difference from ECMAScript decorator `@autobind`
3. Why NestJS still uses experimental decorators
