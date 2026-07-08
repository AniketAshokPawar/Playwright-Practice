# Playwright - Grouping & Hooks

---

# What is Grouping?

Grouping allows you to organize related test cases together using `test.describe()`.

Benefits:

* Organizes test cases logically.
* Apply hooks (`beforeEach`, `afterEach`, etc.) to a specific group.
* Run a particular group using `--grep`.
* Improves readability and maintainability.

---

# Syntax

```ts
test.describe("Login Tests", () => {

    test("Valid Login", async ({ page }) => {

    });

    test("Invalid Login", async ({ page }) => {

    });

});
```

> **Note:** `test.describe()` callback **should not be `async`**.

❌ Incorrect

```ts
test.describe("Group", async () => {

});
```

✅ Correct

```ts
test.describe("Group", () => {

});
```

---

# Running a Specific Group

Example:

```bash
npx playwright test --grep "Login Tests"
```

or

```bash
npx playwright test tests/Login.spec.ts --grep "Login Tests"
```

---

# What are Hooks?

Hooks are special methods that execute **before or after tests**.

They help avoid duplicate code.

Typical use cases:

* Launch application
* Login
* Logout
* Close browser
* Create test data
* Delete test data
* Database setup/cleanup

---

# Types of Hooks

## 1. `beforeAll()`

Runs **once before all tests**.

Example

```ts
test.beforeAll(async () => {

});
```

Use for:

* Launch application
* Create Browser Context/Page manually
* One-time setup

---

## 2. `beforeEach()`

Runs **before every test**.

Example

```ts
test.beforeEach(async ({ page }) => {

});
```

Use for:

* Login
* Navigate to a page
* Test data setup

---

## 3. `afterEach()`

Runs **after every test**.

Example

```ts
test.afterEach(async ({ page }) => {

});
```

Use for:

* Logout
* Cleanup
* Delete created records

---

## 4. `afterAll()`

Runs **once after all tests**.

Example

```ts
test.afterAll(async () => {

});
```

Use for:

* Close browser/context
* Final cleanup
* Generate reports (if needed)

---

# Execution Order

Suppose there are two tests.

```text
beforeAll()

↓

beforeEach()

↓

Test 1

↓

afterEach()

↓

beforeEach()

↓

Test 2

↓

afterEach()

↓

afterAll()
```

---

# Example

```ts
test.beforeAll()

test.beforeEach()

test("Test 1")

test("Test 2")

test.afterEach()

test.afterAll()
```

Execution

```text
beforeAll

beforeEach

Test 1

afterEach

beforeEach

Test 2

afterEach

afterAll
```

---

# Relation with Your Example

## `beforeAll()`

```ts
page = await browser.newPage();

await page.goto("https://www.demoblaze.com/");
```

Runs only once.

Purpose:

* Create page.
* Open application.

---

## `beforeEach()`

```ts
Login

↓

Username

↓

Password

↓

Click Login
```

Runs before every test.

Purpose:

* Ensure each test starts with a logged-in user.

---

## `afterEach()`

```ts
Logout
```

Runs after every test.

Purpose:

* Restore the application to a clean state.

---

## `afterAll()`

```ts
await page.close();
```

Runs once after all tests finish.

Purpose:

* Close the page.

---

# Why Use Hooks?

Without hooks

```text
Test 1

Launch App

Login

Execute Test

Logout

----------------

Test 2

Launch App

Login

Execute Test

Logout
```

Repeated code everywhere.

---

With hooks

```text
beforeAll

Launch App

------------

beforeEach

Login

------------

Test

------------

afterEach

Logout

------------

afterAll

Close Page
```

Cleaner and reusable.

---

# Using Browser in `beforeAll()`

❌ Incorrect

```ts
test.beforeAll(async ({ page }) => {

});
```

Reason:

`page` fixture is created **per test**, so it is **not available** in `beforeAll()` or `afterAll()`.

---

✅ Correct

```ts
let page: Page;

test.beforeAll(async ({ browser }) => {

    page = await browser.newPage();

});
```

or preferably

```ts
let context;
let page: Page;

test.beforeAll(async ({ browser }) => {

    context = await browser.newContext();

    page = await context.newPage();

});
```

---

# Why `browser` Works but `page` Doesn't?

* `browser` is created before tests start.
* `page` is created for each individual test.

Therefore:

| Fixture |          beforeAll          | beforeEach | Test | afterEach |           afterAll          |
| ------- | :-------------------------: | :--------: | :--: | :-------: | :-------------------------: |
| browser |              ✅              |      ✅     |   ✅  |     ✅     |              ✅              |
| context | ❌ (unless created manually) |      ✅     |   ✅  |     ✅     | ❌ (unless created manually) |
| page    |              ❌              |      ✅     |   ✅  |     ✅     |              ❌              |

---

# Common Mistakes

### 1. Using `page` in `beforeAll()`

❌

```ts
test.beforeAll(async ({ page }) => {

});
```

---

### 2. Making `describe()` async

❌

```ts
test.describe("Tests", async () => {

});
```

✅

```ts
test.describe("Tests", () => {

});
```

---

### 3. Not waiting after Login

```ts
await login.click();
```

Immediately running the test may fail because login is still in progress.

Better

```ts
await login.click();

await expect(page.locator("#logout2")).toBeVisible();
```

---

# Interview Questions

### What is Grouping?

Grouping organizes related test cases using `test.describe()`.

---

### Why do we use Hooks?

To avoid duplicate setup and cleanup code across multiple tests.

---

### Difference between `beforeAll()` and `beforeEach()`?

| beforeAll                  | beforeEach             |
| -------------------------- | ---------------------- |
| Runs once before all tests | Runs before every test |

---

### Difference between `afterEach()` and `afterAll()`?

| afterEach             | afterAll                  |
| --------------------- | ------------------------- |
| Runs after every test | Runs once after all tests |

---

### Can we use `page` in `beforeAll()`?

**No.**

`page` is a per-test fixture.

Use `browser.newContext()` and `context.newPage()` if a shared page is needed.

---

### Can `test.describe()` be async?

**No.**

Use:

```ts
test.describe("Group", () => {

});
```

---

# Quick Revision

| Concept           | Purpose                |
| ----------------- | ---------------------- |
| `test.describe()` | Group related tests    |
| `beforeAll()`     | One-time setup         |
| `beforeEach()`    | Runs before every test |
| `afterEach()`     | Runs after every test  |
| `afterAll()`      | One-time cleanup       |

---

# Memory Trick

```
beforeAll  → Start Project

beforeEach → Login

Test

afterEach → Logout

afterAll → Close Project
```
