---
type: code-walkthrough
course_slug: understanding-typescript
mode: practice
created: 2026-05-04
---

# Code Walkthrough Guide — Understanding TypeScript

Hướng dẫn đọc code trong `resources/` theo thứ tự — để học qua code thực tế thay vì chỉ đọc transcript.

## Cách dùng file này

1. Mở resource folder theo thứ tự bên dưới
2. Đọc code trước, tự đoán concept
3. Sau đó đọc transcript tương ứng để confirm
4. Ghi chú phát hiện quan trọng vào session file

---

## M02 — Type System: Code Progression

### Bước 1: Đọc theo lecture pair (starter → finished)

Với mỗi concept, so sánh starter vs finished code:

| Starter | Finished | Key Diff |
|---|---|---|
| `resources/215-starting-project/` | — | Xem cấu trúc TypeScript project |

> Note: M02 không có nhiều resource folders riêng — đọc từ `lectures/13.txt` đến `lectures/39.txt` + tự code.

### Pattern: Nhận biết "TypeScript thinking"

Khi đọc code TS, luôn hỏi:
1. Tại sao biến này cần annotation? (vs để inference)
2. Type này đến từ đâu? (inferred / explicit / imported)
3. Nếu type sai → lỗi ở đâu?

---

## M06 — Classes: Resource Walkthrough

### Không có resource folders cho M06
Tự tạo code theo transcript. Dùng `resources/215-starting-project/` làm TypeScript sandbox.

### Key patterns để tìm trong transcript:

```typescript
// Pattern 1: Constructor shorthand
constructor(public name: string, private age: number) {}
// vs verbose:
public name: string; private age: number;
constructor(name: string, age: number) { this.name = name; this.age = age; }

// Pattern 2: Abstract class
abstract class Shape {
  abstract area(): number; // must implement in subclass
  toString(): string { return `Area: ${this.area()}`; } // can provide default
}

// Pattern 3: Interface implementation
interface Flyable { fly(): void; altitude: number; }
class Bird extends Animal implements Flyable {
  altitude = 100;
  fly() { console.log("flap"); }
}
```

---

## M13 — DnD Project: Full Walkthrough

### Resource folder progression

Work through these in order:

| Step | Folder | What to Study |
|---|---|---|
| 0 | `resources/163-prj-00-initial-starting-setup/` | Starter HTML + empty TS |
| 1 | `resources/164-prj-02-prj-input-form/` | Form HTML structure |
| 2 | `resources/165-prj-03-form-access-and-bind-this/` | `querySelector` + `bind(this)` |
| 3 | `resources/166-prj-04-autobind-decorator/` | `@autobind` decorator pattern |
| 4 | `resources/167-prj-05-fetching-user-input-with-validation/` | Input capture |
| 5 | `resources/168-prj-06-more-elaborate-validation/` | Validation interface |
| 6 | `resources/169-prj-07-rendering-a-project-section/` | Project list render |
| 7 | `resources/170-prj-08-basic-list-rendering-basic-state-mgmt/` | State class |
| 8 | `resources/171-prj-09-project-and-listener-types/` | Types + listeners |
| 9 | `resources/172-prj-10-filtering-added/` | Active/finished filter |
| 10 | `resources/173-prj-11-inheritance-and-generics/` | Generic Component class |
| 11 | `resources/174-prj-12-added-projectitem-class/` | ProjectItem class |
| 12 | `resources/175-prj-13-added-a-getter/` | Getter syntax |
| 13 | `resources/176-prj-14-draggable-list-item/` | Draggable interface |
| 14 | `resources/177-prj-15-visual-drag-and-drop-feedback/` | DragTarget interface |
| 15 | `resources/179-prj-16-finished/` | Final solution |

### Reading Strategy for Each Step

1. Open `src/app.ts` (or wherever code lives)
2. Find what CHANGED from previous step (`diff` mentally or with git)
3. Identify: which TypeScript concept was introduced?
4. Ask: how would I write this without looking?

### Key TS Patterns in DnD Project

```typescript
// Pattern: Generic base Component class
abstract class Component<T extends HTMLElement, U extends HTMLElement> {
  templateElement: HTMLTemplateElement;
  hostElement: T;
  element: U;
  constructor(templateId: string, hostElementId: string, ...) { ... }
  abstract configure(): void;
  abstract renderContent(): void;
}

// Pattern: Drag & Drop interfaces
interface Draggable {
  dragStartHandler(event: DragEvent): void;
  dragEndHandler(event: DragEvent): void;
}
interface DragTarget {
  dragOverHandler(event: DragEvent): void;
  dropHandler(event: DragEvent): void;
  dragLeaveHandler(event: DragEvent): void;
}

// Pattern: State management with listeners
type Listener<T> = (items: T[]) => void;
class State<T> {
  protected listeners: Listener<T>[] = [];
  addListener(listenerFn: Listener<T>) { this.listeners.push(listenerFn); }
}
```

---

## M10 — Type Utilities: Resource Walkthrough

### Resources
`resources/195-starting-project/` — starting point for utility types section.

### Key code to find and understand

```typescript
// typeof — get type from value
const config = { host: "localhost", port: 3000 };
type Config = typeof config; // { host: string; port: number }
function createServer(opts: typeof config) { ... }

// keyof — get union of key types
type ConfigKey = keyof Config; // "host" | "port"
function getConfigValue<K extends keyof Config>(key: K): Config[K] { ... }

// Mapped type — transform every property
type ReadonlyConfig = { readonly [K in keyof Config]: Config[K] };
// Equivalent to: { readonly host: string; readonly port: number }

// Conditional type
type IsString<T> = T extends string ? true : false;
// IsString<"hello"> → true
// IsString<42> → false

// infer — extract inner type
type UnwrapPromise<T> = T extends Promise<infer R> ? R : T;
// UnwrapPromise<Promise<string>> → string
// UnwrapPromise<number> → number
```

---

## Experimental Decorators: Resource Walkthrough

### Resources (L147-L161 = experimental, M12)

| Folder | Content |
|---|---|
| `resources/147-decorators-01-starting-setup/` | Baseline |
| `resources/147-decorators-02-first-class-decorator/` | First decorator |
| `resources/148-decorators-03-decorator-factories/` | Factory pattern |
| `resources/149-decorators-04-adv-decorators/` | Advanced |
| `resources/150-decorators-05-finished-class-decorators/` | Class decorators done |
| `resources/151-decorators-06-different-decorators/` | Method/property/accessor |
| `resources/152-decorators-07-non-class-decorators-finished/` | All decorator types |
| `resources/155-decorators-08-returning-values-in-decorators/` | Return values |
| `resources/156-decorators-09-example-autobind/` | autobind use-case |
| `resources/158-decorators-10-decorator-validation/` | Validation decorator |

### What to look for:
1. `@Logger("HTML")` → decorator factory vs plain `@Logger`
2. `@autobind` on method → replaces with bound version
3. `@required`, `@min(0)` → validation decorators with metadata

---

## M14 — Modules: Resource Walkthrough

| Folder | Content |
|---|---|
| `resources/182-starting-project/` | Single-file starting point |
| `resources/184-prj-16-finished/` | (DnD completed) |
| `resources/186-modules-01-namespaces/` | TS namespaces approach |
| `resources/187-modules-02-es-modules-basics/` | ES modules (import/export) |
| `resources/190-modules-03-finished-modules/` | Split into modules |

### Key change to spot:
```typescript
// Before (namespaces):
namespace App {
  export interface Draggable { ... }
}
/// <reference path="drag-drop-interfaces.ts" />

// After (ES modules):
// drag-drop-interfaces.ts
export interface Draggable { ... }
// app.ts
import { Draggable } from "./drag-drop-interfaces.js";
```

---

## M15 — Build Tools / Webpack: Resource Walkthrough

| Folder | Content |
|---|---|
| `resources/207-webpack-01-basic-setup/` | Base webpack config |
| `resources/208-webpack-02-added-ts-loader/` | ts-loader added |
| `resources/210-webpack-03-finished-dev-setup/` | Full dev config |
| `resources/211-webpack-04-added-prod-workflow/` | Production config |

### Only read if using Webpack — Vite is simpler for new projects.

---

## React + TS: Resource Walkthrough

| Folder | Content |
|---|---|
| `resources/225-prj-libs-01-starting-setup/` | React TS starter |
| `resources/225-prj-libs-02-basic-form-and-markup/` | Form component |

### Key patterns in React + TS:

```typescript
// Typed props
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: "primary" | "secondary";
  children?: React.ReactNode;
}
const Button: React.FC<ButtonProps> = ({ label, onClick, variant = "primary" }) => ...

// Typed useState
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);

// Typed useRef
const inputRef = useRef<HTMLInputElement>(null);
// inputRef.current?.focus(); // safe call

// Typed event handler
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};
```
