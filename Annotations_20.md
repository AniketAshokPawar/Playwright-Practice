# Playwright Notes - Annotations

---

# What are Annotations?

Annotations are special methods in Playwright that **control how tests are executed**.

They help to:

* Run only selected tests.
* Skip unnecessary tests.
* Mark tests as expected failures.
* Increase timeout for slow tests.
* Temporarily disable tests.

---

# Types of Annotations

| Annotation     | Purpose                                                        |
| -------------- | -------------------------------------------------------------- |
| `test.only()`  | Run only the selected test(s).                                 |
| `test.skip()`  | Skip the test completely.                                      |
| `test.fixme()` | Skip a test that is known to be broken or not yet implemented. |
| `test.fail()`  | Mark a test as an expected failure.                            |
| `test.slow()`  | Triple the timeout for the current test.                       |

---

# 1. `test.only()`

Runs **only the marked test(s)** and ignores all others.

### Syntax

```ts
test.only("Login Test", async ({ page }) => {

});
```

### Example

```ts
test.only("Verify Login", async ({ page }) => {

});
```

Only this test executes.

---

# 2. `test.skip()`

Skips the test.

The test appears as **Skipped** in the report.

### Syntax

```ts
test.skip("Login Test", async ({ page }) => {

});
```

### Example

```ts
test.skip("Verify Login", async ({ page }) => {

});
```

### Use Cases

* Feature not ready.
* Environment issue.
* Test temporarily disabled.
* Third-party dependency unavailable.

---

# 3. `test.fixme()`

Marks the test as **known broken**.

Playwright skips executing it.

### Syntax

```ts
test.fixme("Broken Test", async ({ page }) => {

});
```

### Use Cases

* Feature under development.
* Known application bug.
* Test implementation pending.

---

# Difference: `skip()` vs `fixme()`

| `skip()`                   | `fixme()`                                              |
| -------------------------- | ------------------------------------------------------ |
| Intentionally skip a test. | Skip because the feature/test is broken or incomplete. |
| Test is not required now.  | Test is expected to be fixed later.                    |

---

# 4. `test.fail()`

Marks the test as **expected to fail**.

If the test actually fails → ✅ Test is reported as Passed (Expected Failure).

If the test unexpectedly passes → ❌ Playwright reports it as a failure because it was expected to fail.

### Syntax

```ts
test.fail();
```

or

```ts
test.fail("Known application bug");
```

### Example

```ts
test("Login", async ({ page }) => {

    test.fail();

    await expect(page).toHaveTitle("Wrong Title");

});
```

---

# 5. `test.slow()`

Marks the current test as slow.

### Important

`test.slow()` **does NOT slow down test execution.**

It only **triples the test timeout**.

Example

Default timeout

```ts
timeout = 30000 ms
```

After

```ts
test.slow();
```

Timeout becomes

```text
90000 ms (90 seconds)
```

---

### Syntax

```ts
test("Long Running Test", async ({ page }) => {

    test.slow();

});
```

---

### When to Use

* Large file upload.
* Report generation.
* Long API processing.
* Slow application pages.
* Complex workflows.

---

# Example from Your Code

```ts
test("test 4", async ({ page }) => {

    test.slow();

    await page.goto(...);

    await expect(start).toBeVisible();

    console.log(await start.textContent());

    await start.click();

});
```

### What happens?

Default timeout

```text
30 seconds
```

After

```ts
test.slow();
```

Playwright changes timeout to

```text
90 seconds
```

It **does not**:

* Slow each step.
* Add delay between actions.
* Pause before the test.

It only gives the **entire test** more time to finish.

---

# Can We Use Multiple Annotations?

Yes.

Example

```ts
test.only("Login Test", async ({ page }) => {

    test.slow();

});
```

Only this test runs, and it receives a longer timeout.

---

# Interview Questions

## What are annotations?

Annotations control the execution behavior of Playwright tests (run, skip, expected failure, slow, etc.).

---

## Difference between `skip()` and `fixme()`?

`skip()` is used when you intentionally don't want to execute a test.

`fixme()` is used when the feature or test is known to be broken and needs fixing.

---

## What does `test.slow()` do?

It **triples the test timeout**.

It **does not slow down** actions or add waits.

---

## What does `test.fail()` do?

It marks a test as an **expected failure**.

* Test fails → Reported as expected.
* Test passes → Reported as a failure because Playwright expected it to fail.

---

## Can we have multiple `test.only()`?

Yes.

If multiple tests use `test.only()`, Playwright runs **only those tests**.

---

# Quick Revision

| Annotation     | Meaning                      |
| -------------- | ---------------------------- |
| `test.only()`  | Run only selected test(s).   |
| `test.skip()`  | Skip test.                   |
| `test.fixme()` | Skip broken/incomplete test. |
| `test.fail()`  | Expected failure.            |
| `test.slow()`  | Triple the test timeout.     |

---

# Memory Trick

```text
only   → Run only this

skip   → Ignore this

fixme  → Broken, fix later

fail   → Failure is expected

slow   → More timeout (3×), NOT slower execution
```
