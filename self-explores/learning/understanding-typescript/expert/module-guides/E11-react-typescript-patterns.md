# E11 — React + TypeScript Patterns

**Expert module** | Covers: L232–L245 | ~60 min

---

## 1. Core Mental Model

> "React's utility types (`ComponentProps<typeof X>`, `ReturnType<typeof useCustomHook>`, `Parameters<typeof fn>`) derive types from the source of truth automatically. Manual duplication of prop types diverges from implementation over time."

Expert insight: TypeScript's type inference is most powerful when you derive types rather than declare them. In React, the component IS the type definition. Use `ComponentProps` to extract what a component accepts — don't rewrite it.

---

## 2. Beginner Misunderstanding

> "I write an interface for every component's props. It's clean and explicit."

The maintenance problem: when you change `Button`'s `disabled` prop to accept `string | boolean`, you update the component signature — but anyone who manually typed `ButtonProps` or spread `{ disabled: boolean }` still compiles, silently wrong.

`ComponentProps<typeof Button>` stays in sync automatically.

---

## 3. Professional Concern

> "Our team has three different patterns for typing React components: `FC<Props>`, `function Comp(props: Props)`, and `(props: Props): JSX.Element`. They're inconsistent and new developers don't know which to use."

The current recommendation (React 18+): plain function with typed props parameter. `FC` was popular but has two drawbacks: (1) implicit `children` prop removed in React 18, (2) it prevents you from using generics in JSX. Just use `function Component({ prop }: Props)`.

---

## 4. Mistake Traps (no solution shown)

**Trap 1: `useState` without generic type infers `never`**
```typescript
const [items, setItems] = useState([]);
// items: never[]
// setItems([{ id: '1', name: 'Alice' }]); // Error: Type '...' is not assignable to 'never[]'
// Why does this happen? What's the fix?
```

**Trap 2: `useRef` null vs undefined**
```typescript
const inputRef = useRef<HTMLInputElement>();      // current: HTMLInputElement | undefined
const inputRef2 = useRef<HTMLInputElement>(null); // current: HTMLInputElement | null
// Which one should you use for attaching to a JSX element?
// What breaks if you use the wrong one with the ref prop?
```

**Trap 3: Event handler `currentTarget` vs `target` types**
```typescript
function handleChange(e: React.ChangeEvent<HTMLInputElement>) {
  const value = e.target.value;      // HTMLInputElement — correct?
  const value2 = e.currentTarget.value; // HTMLInputElement — same?
  // In React, are these always the same? What about event delegation?
}
```

---

## 5. Expert Shortcut

**`ComponentProps` — derive props from component:**
```typescript
import { ComponentProps } from 'react';

// Extend native element props
interface ButtonProps extends ComponentProps<'button'> {
  variant: 'primary' | 'secondary';
}

function Button({ variant, ...rest }: ButtonProps) {
  return <button {...rest} className={`btn-${variant}`} />;
}

// Get all props a custom component accepts
type MyButtonProps = ComponentProps<typeof Button>;
```

**Generic component (impossible with `FC`):**
```typescript
interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
}

function List<T>({ items, renderItem }: ListProps<T>) {
  return <ul>{items.map(renderItem)}</ul>;
}
// <List items={['a', 'b']} renderItem={s => <li>{s}</li>} /> — T inferred as string
```

**Custom hook return type:**
```typescript
function useToggle(initial = false) {
  const [state, setState] = useState(initial);
  const toggle = useCallback(() => setState(s => !s), []);
  return [state, toggle] as const; // 'as const' gives tuple type, not (boolean | (() => void))[]
}

type UseToggleReturn = ReturnType<typeof useToggle>;
// [boolean, () => void]
```

---

## 6. Real-world Scenario

You're building a `DataTable<T>` component that:
1. Accepts `rows: T[]`
2. Accepts `columns: Array<{ key: keyof T; label: string; render?: (value: T[keyof T]) => React.ReactNode }>`
3. Renders a table where each cell calls `column.render(row[column.key])` if provided

Write the TypeScript component signature. The challenge: `column.key` should only accept actual keys of `T`, and `render`'s argument type should match `T[column.key]`.

---

## 7. Deliberate Drill

**Constraint:** Convert this component to use `ComponentProps<'form'>` extension and a generic type parameter — **without `React.FC`, without manual `children: ReactNode`**:

```typescript
interface FormProps {
  onSubmit: (values: { email: string; password: string }) => void;
  children?: React.ReactNode;
  className?: string;
  disabled?: boolean;
}

const Form: React.FC<FormProps> = ({ onSubmit, children, className, disabled }) => {
  // ...
};
```

New requirement: the form values type should be a generic parameter so the same `Form` works with different field shapes.

---

## 8. Expert AC

Before moving on, verify:
- [ ] Know why plain function is preferred over `React.FC` for new code
- [ ] Can use `ComponentProps<typeof X>` to extend a component's props
- [ ] Can write a generic React component without `FC`
- [ ] Know why `as const` is needed for custom hook tuple returns

---

## 9. Self-review Questions

> **No answers here.** Write answers before checking.

1. `React.ReactNode` vs `React.ReactElement` vs `JSX.Element` — what's the difference? Which is widest? When would you want the narrowest one?

2. You have `const [count, setCount] = useState(0)`. TypeScript infers `count: number`. Now you want to allow `null` as a "not yet set" state. How do you change the declaration?

3. A colleague uses `useRef<HTMLDivElement>()` (no initial value) and then accesses `ref.current.scrollTop` in a `useEffect`. What TypeScript error do they get, and what's the correct pattern?
