# P09 — React + Node Capstone

**Practice module** | Mastery target: L3→L5 | ~3 hours (split into 2 parts)

---

## 1. Build Task

> **No solution provided.** Build from spec.

**Part A — Typed Express API:**

Create `server/app.ts`:

1. Setup Express with `@types/express`, `@types/node`
2. Define `interface Todo { id: string; text: string; completed: boolean; createdAt: Date; }`
3. In-memory `todos: Todo[]` store
4. Routes:
   - `GET /todos` — returns `Todo[]`
   - `POST /todos` — accepts `{ text: string }` body, creates Todo, returns `201` with new todo
   - `PATCH /todos/:id` — accepts `{ completed: boolean }` body, updates todo, returns updated todo or `404`
   - `DELETE /todos/:id` — removes todo, returns `204` or `404`
5. Error handling middleware typed with Express's error handler signature
6. **Constraint:** `req.body` must be validated — add a Zod schema for POST and PATCH bodies (or use manual type guard)

**Part B — React TypeScript Frontend:**

Create a React component tree:
1. `TodoItem`: accepts `todo: Todo` and `onToggle: (id: string) => void` and `onDelete: (id: string) => void`
2. `TodoList`: manages list state via `useState`, fetches from API on mount with `useEffect`, renders `TodoItem` list
3. `AddTodoForm`: controlled form with one input, `onAdd: (text: string) => void` prop
4. `App`: composes the above, owns state lifted from `TodoList`

**Type requirements:**
- `fetch` calls typed with generic: `await fetch('/todos').then(r => r.json()) as Todo[]`
   OR use Zod to validate the response
- All event handlers typed correctly
- No `any`

---

## 2. Break It

**Server side:**
1. Remove `@types/express` — what TypeScript errors appear? What's no longer typed?

2. Change `req.body` access without type guard: `const { text } = req.body`. What is `req.body`'s type? What TypeScript check would catch a missing `text` field at compile time?

3. Change `res.status(201).json(todo)` to `res.status(201).json(null)` — does TypeScript catch this? Why not? What would catch it?

**React side:**
4. Change `useState<Todo[]>([])` to `useState([])` — what is the inferred type? What breaks when you try to map over it with `todo.text`?

5. Remove the type annotation from `onToggle: (id: string) => void` in `TodoItemProps` — use `onToggle: Function` instead. What type safety is lost at the call site?

---

## 3. Fix It

> **Hints only — no solution shown.**

```typescript
// React component with 3 type errors
interface FormProps {
  onSubmit: (value: string) => void;
}

function AddTodoForm(props: FormProps) {
  const inputRef = useRef();  // Error 1: no type parameter or initial value
  
  function handleSubmit(e) {  // Error 2: implicit any on e
    e.preventDefault();
    props.onSubmit(inputRef.current.value); // Error 3: possibly undefined
  }
  
  return <form onSubmit={handleSubmit}><input ref={inputRef} /></form>;
}
```

**Hint 1:** `useRef` for DOM elements needs the element type as generic and `null` as initial value.

**Hint 2:** Form submit event in React: use `React.FormEvent<HTMLFormElement>`.

**Hint 3:** After fixing the ref type, `inputRef.current` is `HTMLInputElement | null`. Use optional chaining or a null check.

---

## 4. Explain It

1. Why is `req.body` typed as `any` by default in Express? What are the two approaches to get a real type from it?

2. `useEffect(() => { fetchTodos(); }, [])` — what does the empty dependency array `[]` mean? When would you put something in it?

3. The `as Todo[]` cast in `fetch().then(r => r.json()) as Todo[]` — TypeScript accepts this but it's a lie. What can happen at runtime if the API returns a different shape? What's the safer alternative?

---

## 5. Apply It

Add these features:
1. **Optimistic updates**: when toggling a todo, update the UI immediately before the server confirms, then rollback on error
2. **Type-safe API client**: extract all `fetch` calls into a `todoApi` object with fully typed methods, replacing inline `fetch` calls

Type requirements for the API client:
```typescript
const todoApi = {
  getAll(): Promise<Todo[]>,
  create(text: string): Promise<Todo>,
  update(id: string, patch: Partial<Pick<Todo, 'completed'>>): Promise<Todo>,
  delete(id: string): Promise<void>,
};
```

---

## 6. Acceptance Criteria

```
Given: POST /todos with body { text: "Buy milk" }
When: request is sent
Then: response is 201 with typed Todo object (id, text, completed, createdAt)

Given: POST /todos with body {} (missing text)
When: request is sent and validation is in place
Then: response is 400 (server-side validation via Zod or type guard)

Given: TodoList component mounts
When: useEffect runs
Then: todos state is typed as Todo[] (not any[])

Given: TodoItem receives a todo prop
When: TypeScript compiles
Then: missing required props produce compile errors
```

---

## 7. Resources

- Speed guides: [`M18-react-typescript.md`](../../../speed/module-guides/M18-react-typescript.md), [`M19-nodejs-typescript.md`](../../../speed/module-guides/M19-nodejs-typescript.md)
- Speed guide: [`M16-types-packages.md`](../../../speed/module-guides/M16-types-packages.md) (Zod)
- Reference lectures: L232–L257
- React + TS starting project: [`resources/215-starting-project/`](../../../../../resources/215-starting-project/)
- Node + TS starting project: [`resources/195-starting-project/`](../../../../../resources/195-starting-project/)
