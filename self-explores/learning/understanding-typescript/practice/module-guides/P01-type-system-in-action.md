# P01 — Type System in Action

**Practice module** | Mastery target: L2→L4 | ~90 min

---

## 1. Build Task

> **No solution provided.** Implement from this spec only.

**Objective:** Build a standalone `validate()` function with full TypeScript typing. No peeking at course code.

**Spec:**

Create a file `validate.ts` with:

1. `interface Validatable` — describes a validation rule set:
   - `value`: accepts both strings and numbers
   - `required`: optional — if true, value must be non-empty/non-zero
   - `minLength` / `maxLength`: optional, numbers — only apply when value is a string
   - `min` / `max`: optional, numbers — only apply when value is a number

2. `function validate(input: Validatable): boolean` — returns `true` if ALL applicable rules pass:
   - Check `required` first
   - For string-specific rules: only check when value is a string (use TypeScript narrowing, not a cast)
   - For number-specific rules: only check when value is a number
   - Rules that are `undefined` or `null` are skipped

3. `function createTitleValidator(title: string): Validatable` — factory that returns a Validatable object requiring a non-empty string, minimum 3 characters, maximum 50 characters.

4. `function createPeopleValidator(people: number): Validatable` — factory that returns a Validatable object requiring a positive integer between 1 and 10.

**Acceptance:** All four functions compile with `strict: true`. Running:
```typescript
console.log(validate(createTitleValidator('')));         // false
console.log(validate(createTitleValidator('abc')));      // true
console.log(validate(createPeopleValidator(0)));         // false
console.log(validate(createPeopleValidator(5)));         // true
```

---

## 2. Break It

After your implementation works, introduce each of these bugs ONE AT A TIME and observe the error:

1. Change `value: string | number` to `value: string` — then call `validate` with a number. What error do you get?

2. Remove the `typeof validatable.value === 'string'` check before the `minLength` comparison. What TypeScript error appears?

3. Change `return isValid` to `return String(isValid)`. What error catches this?

4. Make `minLength` accept `boolean | number` instead of `number`. What breaks at the call site?

After each break: undo and move to the next. Do not move on until you've seen and understood each error.

---

## 3. Fix It

> **Hints only — no solution shown.**

Given this broken `validate` function, fix it using only TypeScript error messages as guidance:

```typescript
interface Validatable {
  value: string | number;
  required?: boolean;
  minLength?: number;
  maxLength?: number;
}

function validate(input: Validatable): boolean {
  let isValid = true;
  if (input.required) {
    isValid = isValid && input.value.toString().trim() !== '';
  }
  if (input.minLength != null) {
    isValid = isValid && input.value.length >= input.minLength; // ERROR HERE
  }
  return isValid;
}
```

**Hint 1:** The error on `input.value.length` is TypeScript narrowing — it doesn't know `value` is a string at that point.

**Hint 2:** You need to add a check before accessing `.length`. What TypeScript construct proves to the compiler that `value` is a string?

**Hint 3:** The check should use `typeof`, not `instanceof`.

---

## 4. Explain It

Answer these in writing (1–3 sentences each) before moving on:

1. Why does `input.value.length` fail when `value: string | number`? What does the error message say, exactly?

2. What is the difference between `null != null` (true/false?) and `undefined != null` (true/false?)? Why is `!= null` used instead of `!== undefined` in the course code?

3. If you remove `let isValid = true` and change to early-return style, does the function still compile? Does the logic change?

---

## 5. Apply It

Extend your `validate` function to support:
- `pattern?: RegExp` — if present, the string must match the regex (only applies to string values)
- `custom?: (value: string | number) => boolean` — custom validator function

Requirements:
- The new fields must be optional
- Pattern check must only run if value is a string
- Custom check runs last
- No `any` types

Write test calls that exercise each new rule.

---

## 6. Acceptance Criteria

```
Given: a Validatable with required=true and value=""
When: validate() is called
Then: returns false

Given: a Validatable with minLength=5 and value="abc"
When: validate() is called
Then: returns false (TypeScript must narrow value to string before comparing .length)

Given: a Validatable with min=1, max=10, value=5
When: validate() is called
Then: returns true (TypeScript must narrow value to number)

Given: a Validatable with minLength and a numeric value
When: TypeScript compiles the validate function
Then: compile error if minLength check is applied without typeof narrowing
```

---

## 7. Resources

- Speed guide: [`M01-types-foundation.md`](../../../speed/module-guides/M01-types-foundation.md)
- Course code: [`resources/168-prj-06-more-elaborate-validation/`](../../../../../resources/168-prj-06-more-elaborate-validation/)
- Reference lectures: L95–L99 (type guards), L30–L33 (function types)
