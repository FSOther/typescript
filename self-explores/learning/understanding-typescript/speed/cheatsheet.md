# TypeScript Cheatsheet — Understanding TypeScript

**One-page reference. No explanations >3 lines. Code-first.**

---

## 1. Type System

```typescript
// Primitives + literal types
let x: string | number | boolean | null | undefined | symbol | bigint;
type Status = 'active' | 'inactive' | 'pending';  // union literal

// Special types
unknown  // must narrow before use
never    // unreachable code / exhaustiveness
any      // opt-out — avoid; prefer unknown
void     // function returns nothing meaningful

// Type annotations
const name: string = 'Alice';
const id: number = 42;
function greet(name: string): string { ... }
const fn: (x: number) => boolean = n => n > 0;

// Type alias vs interface
type Point = { x: number; y: number };
interface Point { x: number; y: number }  // use interface for OOP; type for unions/mapped

// Tuples
const pair: [string, number] = ['age', 30];
const [label, value] = pair;  // destructuring preserves types

// Enum vs const object
const Status = { Active: 'Active', Finished: 'Finished' } as const;
type Status = typeof Status[keyof typeof Status];  // 'Active' | 'Finished'

// Type guards
typeof x === 'string'       // narrows to string
x instanceof MyClass        // narrows to MyClass
'property' in obj           // narrows to type with 'property'
function isUser(x: unknown): x is User { return ... }  // user-defined guard

// Exhaustiveness
function assertNever(x: never): never { throw new Error(JSON.stringify(x)); }
// Use in default branch of switch over discriminated union
```

---

## 2. Classes & Interfaces

```typescript
// Access modifiers
class Foo {
  public name: string;       // accessible everywhere (default)
  private secret: string;    // class only
  protected value: number;   // class + subclasses
  readonly id: string;       // set once
  static count: number = 0;  // on class, not instance

  // Shorthand constructor (auto-assigns)
  constructor(
    public title: string,
    private status: string,
    protected readonly key: string
  ) {}

  get fullTitle() { return `${this.title} [${this.status}]`; }  // getter
  set fullTitle(val: string) { this.title = val; }              // setter
}

// Abstract class
abstract class Component<T extends HTMLElement, U extends HTMLElement> {
  abstract configure(): void;     // subclass must implement
  abstract renderContent(): void;
}

// Interface
interface Draggable {
  dragStartHandler(e: DragEvent): void;
}

class ProjectItem extends Component<HTMLUListElement, HTMLLIElement>
  implements Draggable {
  dragStartHandler(e: DragEvent) { ... }
  configure() { ... }
  renderContent() { ... }
}

// Singleton
class State {
  private static instance: State;
  private constructor() {}
  static getInstance(): State {
    return (State.instance ??= new State());
  }
}
```

---

## 3. Generics

```typescript
// Generic function
function identity<T>(value: T): T { return value; }
const n = identity(42);  // T = number

// Generic constraint (structural minimum)
function getLength<T extends { length: number }>(val: T): number {
  return val.length;
}

// Generic class
class Box<T> {
  constructor(public value: T) {}
  map<U>(fn: (v: T) => U): Box<U> { return new Box(fn(this.value)); }
}

// Utility types (key ones)
Partial<T>                    // all fields optional
Required<T>                   // all fields required
Readonly<T>                   // all fields readonly
Pick<T, 'a' | 'b'>           // subset of fields
Omit<T, 'password'>          // all fields except
Record<string, User>          // key-value map
Exclude<'a'|'b'|'c', 'a'>   // 'b' | 'c'
Extract<'a'|'b'|'c', 'a'|'d'> // 'a'
ReturnType<typeof fn>         // return type of function
Parameters<typeof fn>         // tuple of parameter types
keyof T                       // union of T's keys
T[K]                          // indexed access type (T at key K)

// Mapped type
type MyPartial<T> = { [K in keyof T]?: T[K] };
type MyReadonly<T> = { readonly [K in keyof T]: T[K] };

// Conditional type
type IsString<T> = T extends string ? true : false;
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;

// Update payload pattern
type UpdatePayload<T, Id extends keyof T = 'id'> =
  Pick<T, Id> & Partial<Omit<T, Id>>;
```

---

## 4. Advanced Types & Narrowing

```typescript
// Discriminated union
type Result<T> =
  | { ok: true; data: T }
  | { ok: false; error: string };

// Switch with exhaustiveness
function handle(result: Result<User>) {
  switch (result.ok) {
    case true:  return result.data.name;     // data: User
    case false: return result.error;          // error: string
    default:    assertNever(result);          // compile error if new branch
  }
}

// Intersection type
type Admin = User & { adminLevel: number };

// satisfies (TypeScript 4.9+)
const config = { timeout: 5000 } satisfies Partial<AppConfig>;
// timeout narrowed to 5000, but config validated against AppConfig shape

// Branded types
type UserId = string & { readonly _brand: 'UserId' };
function getUser(id: UserId) { ... }
// getUser('abc')  // error — must use: 'abc' as UserId
```

---

## 5. Decorators

```typescript
// ECMAScript spec (no experimentalDecorators needed)
function autobind<T extends (...args: unknown[]) => unknown>(
  method: T,
  context: ClassMethodDecoratorContext<object, T>
) {
  context.addInitializer(function(this: object) {
    (this as Record<string, unknown>)[context.name as string] = method.bind(this);
  });
}

// Decorator factory
function Logger(prefix: string) {
  return function<T extends new (...args: unknown[]) => object>(
    target: T, _context: ClassDecoratorContext
  ) {
    return class extends target {
      constructor(...args: unknown[]) {
        super(...args);
        console.log(`${prefix}: ${target.name} created`);
      }
    };
  };
}

// Experimental (requires experimentalDecorators: true in tsconfig)
function autobindExp(_: any, _2: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  return {
    configurable: true,
    get() { return original.bind(this); }
  };
}
```

---

## 6. Module System

```typescript
// Named export/import
export interface User { id: string; }
export function getUser(id: string): User { ... }
import { User, getUser } from './user.js';  // .js even for .ts files in ESM

// Default export/import
export default class UserService { ... }
import UserService from './user-service.js';

// Type-only import (required with isolatedModules: true)
import type { User } from './user.js';
import { type User, getUser } from './user.js';  // mixed

// Wildcard
import * as UserUtils from './user.js';

// Re-export
export { User } from './user.js';
export type { UserRole } from './user.js';

// tsconfig for browser (Vite)
"module": "es2022", "moduleResolution": "bundler", "noEmit": true, "isolatedModules": true

// tsconfig for Node.js
"module": "commonjs", "moduleResolution": "node", "outDir": "./dist"
```

---

## 7. Common Patterns

```typescript
// Generic observer (State<T>)
type Listener<T> = (items: T[]) => void;
class State<T> {
  protected listeners: Listener<T>[] = [];
  addListener(fn: Listener<T>) { this.listeners.push(fn); }
  protected notify(items: T[]) { this.listeners.forEach(fn => fn([...items])); }
}

// React: typed props + state + ref
interface Props { label: string; onClick: () => void; }
function Button({ label, onClick }: Props) { ... }
const [items, setItems] = useState<Item[]>([]);
const inputRef = useRef<HTMLInputElement>(null);
// inputRef.current?.value  — safe access

// Express: typed request body
app.post('/users', (req: Request, res: Response) => {
  const body = req.body as { name: string; email: string }; // cast
  // OR: const body = CreateUserSchema.parse(req.body);      // Zod (safer)
});

// Module augmentation (Express req.user)
declare module 'express' {
  interface Request { user?: AuthenticatedUser; }
}

// Const enum alternative (tree-shakable)
const Direction = { Up: 'Up', Down: 'Down' } as const;
type Direction = typeof Direction[keyof typeof Direction]; // 'Up' | 'Down'

// Non-null assertion (use sparingly)
const el = document.getElementById('app')!; // you guarantee non-null
const el2 = document.getElementById('app') as HTMLDivElement; // with type
```
