# Playwright - Flaky Tests & Retries

# What is a Flaky Test?

A **flaky test** is a test that:

* ❌ Fails on one execution.
* ✅ Passes on a retry without any code changes.

Example:

| Attempt   | Result |
| --------- | ------ |
| First Run | ❌ Fail |
| Retry 1   | ✅ Pass |

Playwright reports this test as **Flaky**.

---

# Why Do Flaky Tests Occur?

Common reasons:

* Slow page loading
* Dynamic UI updates
* Timing issues
* Network delays
* Unstable locators
* Animations
* API response delays
* Shared test data
* Test dependency on another test

---

# Why Are Flaky Tests Bad?

* Reduce confidence in automation.
* Cause unnecessary pipeline failures.
* Waste debugging time.
* Hide real application defects.

A good automation suite should have **zero flaky tests**.

---

# What is Retry?

Retry means Playwright automatically re-executes a failed test.

If the retry passes, the test is marked as **Flaky** instead of Failed.

---

# Configure Retries Globally

Inside `playwright.config.ts`

```ts
export default defineConfig({

    retries: 1,

});
```

---

# Retry Values

## No Retry

```ts
retries: 0
```

Behavior

| Attempt   | Result       |
| --------- | ------------ |
| First Run | Final Result |

Maximum executions = **1**

---

## One Retry

```ts
retries: 1
```

Behavior

| Attempt   | Result |
| --------- | ------ |
| First Run | ❌ Fail |
| Retry 1   | ✅/❌    |

Maximum executions = **2**

---

## Two Retries

```ts
retries: 2
```

Behavior

| Attempt   | Result |
| --------- | ------ |
| First Run | ❌      |
| Retry 1   | ❌      |
| Retry 2   | ✅/❌    |

Maximum executions = **3**

---

## Three Retries

```ts
retries: 3
```

Behavior

| Attempt   | Result |
| --------- | ------ |
| First Run | ❌      |
| Retry 1   | ❌      |
| Retry 2   | ❌      |
| Retry 3   | ✅/❌    |

Maximum executions = **4**

---

# Formula

```
Maximum Executions = 1 + retries
```

Example

| retries | Maximum Executions |
| ------: | -----------------: |
|       0 |                  1 |
|       1 |                  2 |
|       2 |                  3 |
|       3 |                  4 |

---

# Configure Retries from Command Line

Override the config value while running tests.

Example:

```bash
npx playwright test --retries=1
```

Run a specific test with retries:

```bash
npx playwright test tests/Login.spec.ts --retries=2
```

This affects only the current execution.

---

# Test Results

## Passed

Passed on the first attempt.

```text
✅ Passed
```

---

## Failed

Failed on all attempts.

Example (`retries:1`)

| Attempt   | Result |
| --------- | ------ |
| First Run | ❌      |
| Retry 1   | ❌      |

Output:

```text
❌ Failed
```

---

## Flaky

Failed initially but passed on a retry.

Example (`retries:1`)

| Attempt   | Result |
| --------- | ------ |
| First Run | ❌      |
| Retry 1   | ✅      |

Output:

```text
⚠ Flaky
```

---

# Relation with Trace

Retries work well with trace options.

Example:

```ts
trace: 'on-first-retry'
```

Meaning:

* First execution → No trace
* First retry → Trace recorded

Useful for debugging flaky tests.

---

# Relation with Video

Example:

```ts
video: 'on-first-retry'
```

Meaning:

* No video during first attempt.
* Video recorded only during the first retry.

---

# Relation with Screenshot

Example:

```ts
screenshot: 'on-first-failure'
```

If the first attempt fails, Playwright captures a screenshot to help debugging.

---

# Best Practices

* Do not use retries to hide real issues.
* Fix unstable locators and timing problems.
* Use retries only for occasional environmental instability.
* Combine retries with traces and videos to investigate flaky tests.

---

# Interview Questions

### What is a flaky test?

A test that fails on the initial execution but passes on a retry without any code changes.

---

### What does `retries: 2` mean?

Playwright executes:

* 1 initial run
* Up to 2 additional retries if needed

Maximum executions = **3**

---

### Does `retries: 3` include the first execution?

**Yes.**

`retries: 3` means:

* 1 initial execution
* Up to 3 additional retry attempts

Maximum executions = **4**

---

### How do you configure retries globally?

```ts
retries: 1
```

inside `playwright.config.ts`.

---

### How do you configure retries from the terminal?

```bash
npx playwright test --retries=2
```

---

### What happens if the first run fails and the retry passes?

The test is marked as **Flaky**.

---

### What happens if every retry fails?

The test is marked as **Failed**.

---

# Quick Revision

| Result   | Meaning                       |
| -------- | ----------------------------- |
| ✅ Passed | Passed on first attempt       |
| ⚠ Flaky  | Failed first, passed on retry |
| ❌ Failed | Failed on every attempt       |

---

# Memory Tip

* **Passed** → Passes immediately.
* **Flaky** → Fails once, then passes.
* **Failed** → Never passes, even after all retries.
