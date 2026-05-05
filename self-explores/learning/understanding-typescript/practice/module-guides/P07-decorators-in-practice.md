# P07 — Decorators in Practice

**Practice module** | Mastery target: L2→L4 | ~75 min

---

## 1. Build Task

> **No solution provided.** Build from spec only — no looking at course decorator files.

**Objective:** Implement `@autobind` using BOTH experimental and ECMAScript decorator syntax.

**Part A — Experimental decorators** (`experimentalDecorators: true`):

Create `autobind-experimental.ts`:

1. `function autobind(target: any, methodName: string, descriptor: PropertyDescriptor): PropertyDescriptor`
   - Gets the original method from `descriptor.value`
   - Returns an adjusted descriptor with:
     - `configurable: true`
     - A `get()` accessor that returns `originalMethod.bind(this)`

**Part B — ECMAScript decorators** (no `experimentalDecorators`):

Create `autobind-ecmascript.ts`:

2. `function autobind<T extends (...args: unknown[]) => unknown>(method: T, context: ClassMethodDecoratorContext<object, T>): void`
   - Uses `context.addInitializer(function(this) { ... })` to bind the method on the instance
   - Sets `this[context.name as string] = method.bind(this)` inside the initializer

**Verify both work** with:
```typescript
class Button {
  label: string = 'Click me';
  
  @autobind
  handleClick(event: Event): void {
    console.log(this.label); // Should NOT be undefined even when passed as callback
  }
}

const btn = new Button();
document.addEventListener('click', btn.handleClick); // this = Button instance
```

---

## 2. Break It

**Experimental version:**
1. Remove the `get()` accessor and instead do `descriptor.value = originalMethod.bind(this)` directly — what is `this` at decoration time (not call time)? What value do you get?

2. Remove `configurable: true` from the returned descriptor — then subclass `Button` and override `handleClick`. What happens?

3. Change `bind(this)` to `bind(null)` — what happens to `this.label` when the handler runs?

**ECMAScript version:**
4. Remove `context.addInitializer` and try to bind in the decorator function body directly — what value is `this` there?

---

## 3. Fix It

> **Hints only — no solution shown.**

```typescript
// Experimental decorator with bug
function autobind(target: any, methodName: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  return {
    configurable: true,
    get() {
      return original; // Bug: method is returned but not bound
    }
  };
}
```

**Hint 1:** The `get()` accessor returns the original method — but `this` will be whatever the caller sets it to.

**Hint 2:** To ensure `this` is always the class instance, you need to... what operation on the function?

**Hint 3:** The `this` inside `get()` refers to the object the property is accessed on — which is the instance.

---

## 4. Explain It

1. Why does passing `btn.handleClick` as an event listener lose `this`? What does JavaScript do to `this` in this scenario?

2. In the experimental `@autobind`, why is a `get()` accessor used instead of replacing `descriptor.value` directly? What timing difference does this enable?

3. `context.addInitializer` in the ECMAScript spec — when exactly does the initializer run? Before constructor body? After? For each instance?

---

## 5. Apply It

Build a `@once` decorator that ensures a method is called at most once. After the first call, subsequent calls are no-ops:

```typescript
class EventEmitter {
  @once
  initialize() {
    console.log('Initialized!'); // Should only print once
  }
}

const e = new EventEmitter();
e.initialize(); // "Initialized!"
e.initialize(); // (nothing)
e.initialize(); // (nothing)
```

Implement using either experimental OR ECMAScript spec. TypeScript must compile cleanly.

---

## 6. Acceptance Criteria

```
Given: @autobind on handleClick
When: btn.handleClick is passed as a callback (no direct call)
Then: this.label is accessible (not undefined)

Given: experimental @autobind
When: tsc compiles without experimentalDecorators: true
Then: compile error (experimental decorators not enabled)

Given: ECMAScript @autobind
When: tsc compiles without experimentalDecorators
Then: compiles cleanly (uses spec-compliant API)
```

---

## 7. Resources

- Speed guide: [`M10-ecmascript-decorators.md`](../../../speed/module-guides/M10-ecmascript-decorators.md)
- Speed guide: [`M11-experimental-decorators.md`](../../../speed/module-guides/M11-experimental-decorators.md)
- Expert guide: [`E07-decorators.md`](../../../expert/module-guides/E07-decorators.md)
- Course code: [`resources/156-decorators-09-example-autobind/`](../../../../../resources/156-decorators-09-example-autobind/)
