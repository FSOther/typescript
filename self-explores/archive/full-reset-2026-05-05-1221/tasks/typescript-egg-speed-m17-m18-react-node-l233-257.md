---
date: 2026-05-05
type: task-worklog
task: typescript-egg
title: "Speed Mode — M17+M18 React+TS & Node+Express L233-257"
status: closed
detail_score: ready-for-dev
tags: [typescript, learning, react, nodejs, express, speed-mode]
---

# Speed Mode — M17+M18 React+TS & Node+Express L233-257

## Objective

Hoàn thành M17 (React + TypeScript) và M18 (Node.js + Express + TypeScript) theo **DEEP_DIVE mode** (speed mode ngoại lệ cho các module liên quan trực tiếp đến web dev). Mục tiêu: nắm TypeScript patterns trong React (hooks, props, events) và Node (Express request/response types, middleware typing).

## Scope

**In scope (M17 — L233-L245):**
- L232: SKIP (intro)
- L233-L234: React + TS project setup
- L235: Typed `useState<T>`
- L236: Typed `useRef<HTMLElement>`
- L237: Typed props (`interface Props { ... }`)
- L238: Typed events (`React.ChangeEvent<HTMLInputElement>`)
- L239: Optional props + children types
- L240: Generic components
- L241-L242: Context API with TypeScript
- L243: `useReducer<State, Action>` typing
- L244-L245: Custom hooks with TypeScript

**In scope (M18 — L247-L257):**
- L246: SKIP (intro)
- L247: Express + TS setup, `@types/express`
- L248: `Request`, `Response`, `NextFunction` types
- L249: Typed route handlers
- L250: Middleware typing
- L251: Request body/params/query typing
- L252-L253: Error handling middleware
- L254-L255: Router typing + controller pattern
- L256-L257: Integration with Zod for request validation

**Out of scope:**
- Full CRUD API implementation (practice mode challenge)
- Next.js / tRPC patterns (expert mode real-world scenarios)
- Authentication middleware (out of course scope)

## Input / Output

**Input:** Transcripts `lectures/233.*.txt` → `lectures/257.*.txt` (skip L232, L246)

**Output:**
- `speed/study-progress.md` — L233-L257 done
- Can annotate React component props + state types
- Can type Express route handler + middleware
- Mental models:
  - React: "Type at the boundaries — props interface + event types"
  - Node: "Request generics `Request<Params, ResBody, ReqBody, Query>` for full typing"

## Dependencies

- **Requires:** `typescript-ml6` (M13 DnD) done
  - Generic patterns and interface contracts from DnD carry over to React components
- **Prerequisite knowledge (outside course):**
  - React hooks basics (useState, useEffect, useRef, useContext, useReducer, custom hooks)
  - Express.js basics (routing, middleware chain, req/res)
  - If weak on these: 30-min review before starting this module

## Flow

### Session 1: M17 React + TypeScript (~2.5h)

#### Step 1: L233-L234 — Project setup (~15 phút)

```bash
cat lectures/233.*.txt lectures/234.*.txt
```

**Đọc:**
- CRA with TS: `create-react-app myapp --template typescript`
- Vite with TS (modern): `npm create vite@latest myapp -- --template react-ts`
- `.tsx` extension requirement
- tsconfig for React: `"jsx": "react-jsx"` (modern) vs `"react"`

**Verify:** Know the 2 project setup approaches and tsconfig jsx setting

---

#### Step 2: L235-L236 — useState & useRef (~20 phút)

```bash
cat lectures/235.*.txt lectures/236.*.txt
```

**Đọc và trả lời:**
- `useState<User | null>(null)` — why explicit generic?
- `useRef<HTMLInputElement>(null)` — why `null` initial value?
- `inputRef.current?.focus()` — why optional chaining?

**Key patterns:**
```typescript
const [user, setUser] = useState<User | null>(null);
const inputRef = useRef<HTMLInputElement>(null);
```

**Verify:** Can explain why `useRef<T>` needs the element type parameter

---

#### Step 3: L237-L239 — Props typing & events (~25 phút)

```bash
cat lectures/237.*.txt lectures/238.*.txt lectures/239.*.txt
```

**IMPORTANT patterns:**

```typescript
// Props interface
interface ButtonProps {
  label: string;
  onClick: () => void;
  disabled?: boolean;
  children?: React.ReactNode;
}

// Event types
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};

const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  // ...
};
```

**Đọc và trả lời:**
- `React.ReactNode` vs `React.ReactElement` — difference?
- Why `React.ChangeEvent<HTMLInputElement>` not just `Event`?

**Verify:** Can annotate a complete form component with proper event types

---

#### Step 4: L240-L242 — Generic components & Context (~30 phút)

```bash
cat lectures/240.*.txt lectures/241.*.txt lectures/242.*.txt
```

**Đọc:**
- Generic component: `function List<T>({ items, renderItem }: { items: T[]; renderItem: (item: T) => React.ReactNode })`
- Context with TypeScript: create typed context, provide + consume
- Context default value typing issue (`null` vs proper default)

**Key pattern:**
```typescript
const ThemeContext = React.createContext<Theme | null>(null);
// In consumer:
const theme = useContext(ThemeContext);
if (!theme) throw new Error("Must use within ThemeProvider");
```

**Verify:** Know why context is often typed as `T | null` and how to handle null

---

#### Step 5: L243-L245 — useReducer & custom hooks (~30 phút)

```bash
cat lectures/243.*.txt lectures/244.*.txt lectures/245.*.txt
```

**Đọc:**
- `useReducer<State, Action>` — discriminated union for Action type
- Custom hook return type annotation: explicit `[T, Dispatch<SetStateAction<T>>]` vs tuple assertion `as const`

**Pattern:**
```typescript
type Action =
  | { type: "increment" }
  | { type: "decrement" }
  | { type: "reset"; payload: number };

function reducer(state: number, action: Action): number {
  switch (action.type) {
    case "increment": return state + 1;
    case "decrement": return state - 1;
    case "reset": return action.payload;  // TS knows payload exists here
  }
}
```

**Verify:** Can write discriminated union for reducer actions + exhaustive switch

---

### Session 2: M18 Node + Express (~2h)

#### Step 6: L247-L249 — Express setup & route types (~25 phút)

```bash
cat lectures/247.*.txt lectures/248.*.txt lectures/249.*.txt
```

**Đọc:**
- `@types/express` install: `npm i -D @types/express`
- Request generics: `Request<Params, ResBody, ReqBody, Query>`
- Response methods: `res.json()`, `res.status(400).json()`

**Key pattern:**
```typescript
import { Request, Response } from "express";

const getUser = (req: Request<{ id: string }>, res: Response) => {
  const { id } = req.params;  // string — typed
  res.json({ id, name: "Alice" });
};
```

**Verify:** Know Request generic param order: Params, ResBody, ReqBody, Query

---

#### Step 7: L250-L253 — Middleware & error handling (~25 phút)

```bash
cat lectures/250.*.txt lectures/251.*.txt lectures/252.*.txt lectures/253.*.txt
```

**Đọc:**
- Middleware type: `(req: Request, res: Response, next: NextFunction) => void`
- Error middleware: 4 params `(err: Error, req: Request, res: Response, next: NextFunction)`
- Request body typing: `Request<{}, {}, CreateUserDto>`
- Query params: `Request<{}, {}, {}, { page: string }>`

**Pattern for body typing:**
```typescript
interface CreateUserDto {
  name: string;
  email: string;
}

app.post("/users", (req: Request<{}, {}, CreateUserDto>, res: Response) => {
  const { name, email } = req.body;  // typed!
});
```

**Verify:** Know error middleware 4-param signature

---

#### Step 8: L254-L257 — Router, controller pattern, Zod validation (~30 phút)

```bash
cat lectures/254.*.txt lectures/255.*.txt lectures/256.*.txt lectures/257.*.txt
```

**Đọc:**
- `Router` from express — typed route handlers
- Controller pattern: class with typed methods
- Zod integration: `schema.parse(req.body)` + error handling

**Zod + Express pattern:**
```typescript
const userSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
});
type CreateUserInput = z.infer<typeof userSchema>;

app.post("/users", (req: Request<{}, {}, CreateUserInput>, res: Response) => {
  const result = userSchema.safeParse(req.body);
  if (!result.success) return res.status(400).json(result.error);
  // result.data is CreateUserInput
});
```

**Verify:** Can explain how `z.infer<typeof schema>` connects Zod schema to TypeScript type

---

#### Step 9: Update progress (~5 phút)

Update `speed/study-progress.md`:
- L232, L246 = `skipped`
- L233-L245, L247-L257 = `done`

---

## Edge Cases

| Tình huống | Xử lý |
|---|---|
| React hooks not familiar | Do 30-min React hooks review before starting session 1 |
| Express not familiar | Do 30-min Express basics review before starting session 2 |
| `Request<{}, {}, Body, {}>` syntax verbose | In practice, often define type alias: `type TypedRequest<B> = Request<{}, {}, B, {}>` |
| Zod covered in M15 but not done yet | L256-257 self-contained — Zod basics obvious from code |
| React generic components complex (L240) | It's OK to mark as done with partial understanding — revisit in practice mode |

## Acceptance Criteria

**AC1 — Happy path: React form component typed**
- **Given** a text input form with onChange and onSubmit handlers
- **When** write the component with TypeScript
- **Then** props use interface, onChange uses `React.ChangeEvent<HTMLInputElement>`, onSubmit uses `React.FormEvent<HTMLFormElement>`

**AC2 — Happy path: Express typed route**
- **Given** POST `/users` endpoint expecting `{name: string, email: string}` body
- **When** write the route handler
- **Then** use `Request<{}, {}, {name: string; email: string}>` and `res.status(201).json({...})`

**AC3 — Negative: Untyped event handler**
- **Given** handler `const handleChange = (e) => { setValue(e.target.value) }`
- **When** asked if this is good TS
- **Then** identify: missing event type annotation — should be `React.ChangeEvent<HTMLInputElement>`

## Technical Notes

- React: Type at component boundaries (props, events) — let TS infer inside
- Node: `Request<Params, ResBody, ReqBody, Query>` — memorize param order
- Zod: `z.infer<typeof schema>` creates TypeScript type from Zod schema
- `@types/express` provides all Express types — must install separately from `express`
- Estimated time: ~4.5h (Session 1 ~2.5h React, Session 2 ~2h Node)
- Prerequisite check: 30-min review for React hooks OR Express basics if weak

## Risks

| Risk | Probability | Mitigation |
|---|---|---|
| Weak on React hooks baseline | Medium | Step 2-5 assume useState/useEffect knowledge — review first |
| Weak on Express baseline | Medium | Step 6-8 assume Express middleware chain — review first |
| Sessions 1 + 2 too long in one sitting | High | Explicitly split across 2 days |
| Generic components L240 confusing | Medium | Speed mode: understand concept, practice in practice mode |

## Worklog

### [2026-05-05] Hoàn thành — 25 lectures M17+M18 trong 1 session

**Kết quả:** 25 lectures hoàn thành (L233-L257), trừ L232 và L246 (SKIP).

**Session log:** [2026-05-05-6.md](../../learning/understanding-typescript/speed/sessions/2026-05-05-6.md)

**Output mong đợi:**
- [x] L233-L245 (M17 React+TS) tất cả status=done trong study-progress.md
- [x] L247-L257 (M18 Node+Express) tất cả status=done trong study-progress.md
- [x] Active recall 3 câu/lecture
- [x] React+TS patterns: props typing, useState<T>, useRef<HTMLInputElement>, FormEvent<HTMLFormElement>
- [x] Node+Express patterns: @types/node, @types/express, Request/Response/NextFunction, URL param conversion
- [x] Native Node TS support: strip-only vs transform-types distinction

**Key findings:**
- React props: `function Comp({ prop }: Props)` — plain function preferred over `FC<Props>`
- `useRef<HTMLInputElement>(null)` — explicit generic required, TS can't infer from null + JSX
- `req.params.id` always `string` → `+req.params.id` converts to number
- Import `.js` extension when compiling even for `.ts` source files (Node.js requirement)
- `verbatimModuleSyntax: true` + `import { type X }` = correct pattern for native Node TS
- Bun/Deno = full TS support; Node strip-only = drop annotations only (no enum/decorator)

**Progress:** 70% (125 lectures done — skips counted, L258-L339 SKIP)

**AC checks:**
- ✅ AC1: Biết type `React.ChangeEvent<HTMLInputElement>` — `event.target.value` via `FormEvent<HTMLFormElement>`
- ✅ AC2: Biết dùng `Request<Params, ResBody, ReqBody>` khi cần — và khi nào để TS infer vs explicit
- ✅ AC3: Hiểu native Node TS support options và trade-offs
