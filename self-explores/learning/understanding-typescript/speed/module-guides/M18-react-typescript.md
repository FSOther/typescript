# M18 — React + TypeScript (L232–L245)

**Speed module** | Lectures: 232–245 | ~45 min

---

## 1. Key Concepts

| Concept | One-liner |
|---------|-----------|
| `.tsx` extension | Files with JSX code; same as `.jsx` but TypeScript |
| `FC` type | `React.FC<Props>` — function component type; optional but common |
| Props interface | Define component props with `interface` or `type` |
| `children` prop | `ReactNode` type; or `PropsWithChildren<YourProps>` utility |
| `useState<T>` | Generic hook — infer from initial value or provide explicit `<T>` |
| `useRef<T>` | `useRef<HTMLInputElement>(null)` — attach to DOM elements |
| Event types | `React.FormEvent`, `React.ChangeEvent<HTMLInputElement>` |
| `as const` for union props | `type Direction = 'up' \| 'down'` instead of string |

---

## 2. Code Patterns

### Component with typed props
```typescript
// GoalList.tsx
interface Goal {
  id: string;
  title: string;
  description: string;
}

interface GoalListProps {
  goals: Goal[];
  onDelete: (id: string) => void;
}

// Option 1: plain function (preferred over FC)
export default function GoalList({ goals, onDelete }: GoalListProps) {
  return (
    <ul>
      {goals.map(g => (
        <li key={g.id}>
          {g.title}
          <button onClick={() => onDelete(g.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}

// Option 2: React.FC with generic props
const GoalList: React.FC<GoalListProps> = ({ goals, onDelete }) => { ... };
```

### Children prop
```typescript
import { type ReactNode } from 'react';

interface CardProps {
  children: ReactNode;
  className?: string;
}
// Alternative: React.PropsWithChildren<{ className?: string }>
```

### useState with explicit type
```typescript
interface Goal { id: string; title: string; }

// Inferred from initial value (array of Goals)
const [goals, setGoals] = useState<Goal[]>([]);

// When initial state doesn't reveal type
const [selected, setSelected] = useState<Goal | null>(null);
```

### useRef attached to DOM element
```typescript
const inputRef = useRef<HTMLInputElement>(null);

// In JSX: <input ref={inputRef} />

function handleSubmit(e: React.FormEvent) {
  e.preventDefault();
  const value = inputRef.current?.value ?? '';
  // inputRef.current is HTMLInputElement | null
}
```

### Passing functions as props
```typescript
interface NewGoalProps {
  onAddGoal: (title: string, description: string) => void;
}

function NewGoal({ onAddGoal }: NewGoalProps) {
  function handleSubmit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault();
    const fd = new FormData(e.currentTarget);
    onAddGoal(fd.get('title') as string, fd.get('description') as string);
  }
  return <form onSubmit={handleSubmit}>...</form>;
}
```

### Image import (Vite)
```typescript
import logoUrl from './assets/logo.png'; // string
// <img src={logoUrl} />
```

---

## 3. Common Gotchas

| Gotcha | Detail |
|--------|--------|
| `FC` includes `children` implicitly (v17 and below) | In React 18+, `FC` does NOT include `children`; must declare explicitly |
| `useRef<T>(null)` vs `useRef<T \| undefined>()` | `null` initial → attaches to DOM; `undefined` initial → mutable ref box |
| Event `currentTarget` vs `target` | `currentTarget` is the element with handler (typed correctly); `target` is loosely typed |
| `as string` on FormData | `fd.get('key')` returns `string \| null`; cast needed if you know it exists |
| Image imports in Vite need `vite-env.d.ts` | Without it, TS doesn't know `.png` is a `string` |

---

## 4. Active Recall

> **No answers here.** Close this file and answer from memory.

1. You have a `Modal` component that wraps any content. Write its props interface and component signature. What type do you use for `children`?

2. A `Counter` component starts at `null` (no selection) until a user picks an item. Write the `useState` call that properly types this state as `Item | null`.

3. You create `useRef` for a form input and access `.value` in a submit handler. TypeScript complains that `.current` might be `null`. What's the idiomatic way to handle this?

---

## 5. Quick Reference

```typescript
// Props pattern
interface Props { label: string; onClick: () => void; }
function Button({ label, onClick }: Props) { ... }

// State patterns
useState<T[]>([])          // typed array state
useState<T | null>(null)   // nullable state
useState(0)                // inferred number state

// Ref pattern
useRef<HTMLInputElement>(null)
ref.current?.value         // safe access

// Common event types
React.FormEvent<HTMLFormElement>
React.ChangeEvent<HTMLInputElement>
React.MouseEvent<HTMLButtonElement>
React.KeyboardEvent<HTMLInputElement>

// Children
children: React.ReactNode   // any renderable thing
children: React.ReactElement // JSX element only
```

---

## 6. Done Criteria

- [ ] Can type props, state, and refs without looking up docs
- [ ] Know the difference between `.tsx` and `.ts`
- [ ] Understand why `useRef<HTMLInputElement>(null)` needs explicit type
- [ ] Can type event handlers correctly for form submit and input change
