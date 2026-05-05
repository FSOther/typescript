---
type: real-world-scenarios
course_slug: understanding-typescript
mode: expert
created: 2026-05-04
---

# Real-World Scenarios — TypeScript Expert Applications

Các tình huống thực tế đòi hỏi TypeScript expert-level thinking. Mỗi scenario có: context, challenge, solution pattern, và lessons learned.

---

## Scenario 1: Type-Safe API Client

**Context:** Building a REST API client where endpoints return different shapes.

**Challenge:** Make the return type depend on which endpoint is called.

```typescript
// Approach 1: Overloads (verbose but simple)
async function apiGet(endpoint: "/users"): Promise<User[]>;
async function apiGet(endpoint: "/users/:id"): Promise<User>;
async function apiGet(endpoint: "/posts"): Promise<Post[]>;
async function apiGet(endpoint: string): Promise<unknown> {
  const res = await fetch(endpoint);
  return res.json();
}

// Approach 2: Generic with endpoint-to-type mapping (scalable)
type ApiRoutes = {
  "/users": User[];
  "/users/:id": User;
  "/posts": Post[];
  "/posts/:id": Post;
};

async function apiGet<T extends keyof ApiRoutes>(
  endpoint: T
): Promise<ApiRoutes[T]> {
  const res = await fetch(endpoint);
  return res.json() as ApiRoutes[T];
}

// Usage: return type inferred!
const users = await apiGet("/users"); // users: User[]
const post = await apiGet("/posts/:id"); // post: Post
```

**Lesson:** Endpoint-type mapping is a pattern used by trpc, react-query, axios generics.

---

## Scenario 2: Form State Management

**Context:** Generic form state that works for any form shape.

**Challenge:** Build `useForm<T>()` where field names and types come from `T`.

```typescript
type FormState<T> = {
  values: T;
  errors: Partial<Record<keyof T, string>>;
  touched: Partial<Record<keyof T, boolean>>;
};

type FormActions<T> = {
  setValue<K extends keyof T>(field: K, value: T[K]): void;
  setError<K extends keyof T>(field: K, error: string | undefined): void;
  submit(handler: (values: T) => void | Promise<void>): void;
  reset(): void;
};

function useForm<T extends object>(initialValues: T): FormState<T> & FormActions<T> {
  // ...implementation
}

// Usage:
interface LoginForm { email: string; password: string; rememberMe: boolean; }

const form = useForm<LoginForm>({
  email: "",
  password: "",
  rememberMe: false,
});

form.setValue("email", "alice@example.com"); // ✅
form.setValue("email", 42); // ❌ TypeScript error: 42 not assignable to string
form.setValue("nonexistent", ""); // ❌ TypeScript error: "nonexistent" not in LoginForm
```

**Lesson:** Generic forms are the basis of formik, react-hook-form type definitions.

---

## Scenario 3: Plugin Architecture

**Context:** Building an extensible system where plugins register capabilities.

**Challenge:** Type-safe plugin registry where each plugin declares its API.

```typescript
// Step 1: Define plugin interface
interface Plugin<Name extends string, API> {
  name: Name;
  setup(): API;
}

// Step 2: Registry that accumulates APIs
type PluginRegistry<Plugins extends Plugin<string, unknown>[]> = {
  [P in Plugins[number] as P["name"]]: ReturnType<P["setup"]>;
};

// Step 3: App that uses registered plugins
function createApp<Plugins extends Plugin<string, unknown>[]>(
  ...plugins: Plugins
): PluginRegistry<Plugins> {
  const registry: Record<string, unknown> = {};
  for (const plugin of plugins) {
    registry[plugin.name] = plugin.setup();
  }
  return registry as PluginRegistry<Plugins>;
}

// Usage:
const authPlugin: Plugin<"auth", { login: () => void; logout: () => void }> = {
  name: "auth",
  setup: () => ({ login: () => {}, logout: () => {} }),
};

const dbPlugin: Plugin<"db", { query: (sql: string) => unknown[] }> = {
  name: "db",
  setup: () => ({ query: (sql) => [] }),
};

const app = createApp(authPlugin, dbPlugin);
app.auth.login(); // ✅
app.db.query("SELECT *"); // ✅
app.unknown.method(); // ❌ TypeScript error
```

---

## Scenario 4: State Machine Types

**Context:** Implementing a finite state machine where only valid transitions are allowed.

```typescript
// Define states and transitions as types
type States = "idle" | "loading" | "success" | "error";

type Transitions = {
  idle: { type: "FETCH" };
  loading: { type: "RESOLVE"; data: unknown } | { type: "REJECT"; error: string };
  success: { type: "RESET" };
  error: { type: "RETRY" } | { type: "RESET" };
};

type Transition<S extends States> = Transitions[S];

// Type-safe dispatch: can only dispatch valid events for current state
class StateMachine<S extends States> {
  constructor(private state: S) {}
  
  dispatch<E extends Transition<S>>(event: E): StateMachine<NextState<S, E["type"]>> {
    // ...transition logic
    return new StateMachine(this.nextState(event));
  }
}

// This pattern appears in XState's TypeScript support
```

---

## Scenario 5: Database Query Builder

**Context:** Typed query builder where selected columns determine result type.

```typescript
type TableSchema = {
  users: { id: number; name: string; email: string; age: number };
  posts: { id: number; title: string; body: string; authorId: number };
};

class QueryBuilder<
  Table extends keyof TableSchema,
  SelectedCols extends keyof TableSchema[Table] = keyof TableSchema[Table]
> {
  select<Cols extends keyof TableSchema[Table]>(
    ...cols: Cols[]
  ): QueryBuilder<Table, Cols> {
    return new QueryBuilder(this.table, cols);
  }
  
  execute(): Pick<TableSchema[Table], SelectedCols>[] {
    // ...runtime implementation
    return [];
  }
}

// Usage:
const results = new QueryBuilder("users")
  .select("id", "name") // narrows result type
  .execute(); // results: { id: number; name: string }[]

results[0].email; // ❌ TypeScript error: 'email' not in selected columns
```

**Lesson:** This pattern is used by Drizzle ORM, kysely, and similar type-safe ORMs.

---

## Scenario 6: Middleware Chain Typing

**Context:** Express-like middleware with type-safe request augmentation.

```typescript
// Each middleware can add properties to Request
type RequestWith<T> = Express.Request & T;

function withUser<Req extends Express.Request>(
  handler: (req: RequestWith<{ user: User }>) => void
) {
  return (req: Req, res: Express.Response, next: Express.NextFunction) => {
    const user = authenticateRequest(req);
    if (!user) { res.status(401).json({ error: "Unauthorized" }); return; }
    handler({ ...req, user });
  };
}

// Usage:
app.get("/profile",
  withUser((req) => {
    console.log(req.user.name); // req.user is typed!
  })
);
```

---

## Scenario 7: Configuration with Validation

**Context:** Runtime-validated configuration that's fully typed.

```typescript
import { z } from "zod";

// Define schema once — derive type AND validator from it
const ConfigSchema = z.object({
  port: z.number().int().min(1).max(65535),
  host: z.string().default("localhost"),
  database: z.object({
    url: z.string().url(),
    maxConnections: z.number().int().positive().default(10),
  }),
  features: z.record(z.boolean()).default({}),
});

type Config = z.infer<typeof ConfigSchema>;
// Config is fully typed: { port: number; host: string; database: { url: string; maxConnections: number }; features: Record<string, boolean> }

function loadConfig(raw: unknown): Config {
  const result = ConfigSchema.safeParse(raw);
  if (!result.success) {
    throw new Error(`Invalid config: ${result.error.message}`);
  }
  return result.data;
}

const config = loadConfig(process.env); // validated + typed
config.database.maxConnections; // number, not string
```

**Lesson:** Zod is the standard approach for runtime + compile-time validation in TypeScript.

---

## Scenario 8: Conditional Props in React

**Context:** Component that requires different props based on a mode prop.

```typescript
// Discriminated union for props
type ButtonProps =
  | { variant: "link"; href: string; external?: boolean }
  | { variant: "submit"; form?: string; loading?: boolean }
  | { variant: "button"; onClick: () => void; disabled?: boolean };

function Button(props: ButtonProps & { label: string }) {
  if (props.variant === "link") {
    // props.href is available, props.onClick is not
    return <a href={props.href}>{props.label}</a>;
  }
  if (props.variant === "submit") {
    return <button type="submit" form={props.form}>{props.label}</button>;
  }
  return <button onClick={props.onClick} disabled={props.disabled}>{props.label}</button>;
}

// Usage:
<Button variant="link" href="/about" label="About" />  // ✅
<Button variant="button" label="Click" />  // ❌ onClick is required
<Button variant="link" onClick={() => {}} label="X" />  // ❌ onClick not valid for link
```

---

## Scenario 9: Readonly Data Flow

**Context:** Ensuring immutability through a data pipeline.

```typescript
// Deep freeze pattern with types
type DeepReadonly<T> = T extends (infer U)[]
  ? ReadonlyArray<DeepReadonly<U>>
  : T extends object
  ? { readonly [K in keyof T]: DeepReadonly<T[K]> }
  : T;

function freeze<T>(obj: T): DeepReadonly<T> {
  return Object.freeze(
    Object.fromEntries(
      Object.entries(obj as object).map(([k, v]) =>
        [k, typeof v === "object" && v !== null ? freeze(v) : v]
      )
    )
  ) as DeepReadonly<T>;
}

const config = freeze({ db: { host: "localhost", port: 5432 } });
config.db.port = 3000; // ❌ TypeScript error: cannot assign to readonly property
```

---

## Expert Pattern Cheatsheet

| Scenario | Key Pattern | TypeScript Features |
|---|---|---|
| API client | Endpoint-type mapping | `keyof`, indexed access |
| Form state | Generic form with field narrowing | `keyof T`, `T[K]` |
| Plugin system | Registry accumulating APIs | Template literal + mapped types |
| State machine | Valid transitions only | Discriminated unions, conditional types |
| Query builder | Column selection narrows result | Generic chaining, `Pick` |
| Middleware | Request augmentation | Intersection types, generics |
| Configuration | Zod schema → inferred type | `z.infer<typeof Schema>` |
| Conditional props | Variant-based props | Discriminated union props |
| Immutability | Deep readonly | Recursive mapped types |
