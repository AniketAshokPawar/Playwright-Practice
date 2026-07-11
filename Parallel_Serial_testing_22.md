# Playwright Notes - Parallel & Serial Test Execution

---

# Why do we control Test Execution?

By default, Playwright executes tests efficiently using workers. Depending on the project requirement, we may want to:

* Run tests faster (Parallel execution)
* Run tests one after another (Serial execution)
* Control execution at the project level or file level

---

# Types of Test Execution

| Execution Mode | Description                                                                                   |
| -------------- | --------------------------------------------------------------------------------------------- |
| Parallel       | Multiple tests run simultaneously.                                                            |
| Serial         | Tests run one after another in sequence.                                                      |
| Default        | Tests in a file execute sequentially, while multiple files may run in parallel using workers. |

---

# Default Behaviour

Suppose a file contains:

```ts
test("Test 1", async()=>{});

test("Test 2", async()=>{});

test("Test 3", async()=>{});
```

Execution:

```text
Test 1

↓

Test 2

↓

Test 3
```

Tests inside the same file execute sequentially by default.

Different test files can execute in parallel depending on the number of workers.

---

# Control Parallel Execution from Config File

In `playwright.config.ts`

```ts
export default defineConfig({

    fullyParallel: true,

});
```

### Meaning

* Every test inside every file can run in parallel.
* Best for independent tests.
* Reduces execution time.

---

# Disable Fully Parallel Execution

```ts
fullyParallel: false
```

This is the default behavior.

---

# Control Number of Workers

Workers determine how many tests/files execute simultaneously.

Example

```ts
workers: 4
```

Meaning

Maximum **4 workers** will execute tests in parallel.

---

### Run with One Worker

```ts
workers: 1
```

Execution becomes almost sequential.

Useful for:

* Debugging
* Shared test data
* Login-dependent tests

---

# Override Workers from Terminal

Run with 2 workers

```bash
npx playwright test --workers=2
```

Run with 1 worker

```bash
npx playwright test --workers=1
```

Terminal value overrides the config file.

---

# Serial Execution (Specific Test File)

Sometimes only one file should execute sequentially.

Use

```ts
test.describe.configure({
    mode: "serial"
});
```

Example

```ts
import { test } from "@playwright/test";

test.describe.configure({
    mode: "serial"
});

test("Test 1", async()=>{

});

test("Test 2", async()=>{

});

test("Test 3", async()=>{

});
```

Execution

```text
Test 1

↓

Test 2

↓

Test 3
```

---

# Why use Serial Mode?

Use when tests depend on one another.

Example

```text
Test 1

↓

Create User

↓

Test 2

↓

Edit User

↓

Test 3

↓

Delete User
```

If **Create User** fails, remaining tests should not execute.

Serial mode automatically skips remaining tests if one fails.

---

# Parallel Execution for a Specific File

Instead of serial

```ts
test.describe.configure({
    mode: "parallel"
});
```

Example

```ts
test.describe.configure({
    mode: "parallel"
});
```

Execution

```text
Test 1

Test 2

Test 3

Test 4
```

All tests can execute simultaneously.

---

# Default Mode Explicitly

```ts
test.describe.configure({
    mode: "default"
});
```

Uses Playwright's normal execution behavior.

---

# Available Modes

| Mode         | Meaning                          |
| ------------ | -------------------------------- |
| `"default"`  | Normal Playwright execution.     |
| `"parallel"` | Execute tests in parallel.       |
| `"serial"`   | Execute tests one after another. |

---

# Example from Your Code

```ts
test.describe.configure({
    mode: "serial"
});

test("test 1", async()=>{

});

test("test 2", async()=>{

});

test("test 3", async()=>{

});

test("test 4", async()=>{

});

test("test 5", async()=>{

});
```

Execution

```text
test 1

↓

test 2

↓

test 3

↓

test 4

↓

test 5
```

If **test 2** fails

```text
test 1 ✅

test 2 ❌

test 3 Skipped

test 4 Skipped

test 5 Skipped
```

---

# Config File vs describe.configure()

| Config File                         | describe.configure()                                |
| ----------------------------------- | --------------------------------------------------- |
| Applies to the whole project.       | Applies only to the current file or describe block. |
| Used for global execution behavior. | Used for specific scenarios.                        |

---

# When to Use Parallel?

Use when:

* Tests are independent.
* No shared data.
* No dependency between tests.
* Faster execution is required.

Example

```text
Login Test

Search Test

Cart Test

Profile Test
```

Each can run independently.

---

# When to Use Serial?

Use when:

* Tests depend on previous tests.
* Same data is reused.
* Order matters.

Example

```text
Create Customer

↓

Edit Customer

↓

Delete Customer
```

---

# Interview Questions

## What is the default execution in Playwright?

* Tests inside a file execute sequentially.
* Multiple test files may execute in parallel depending on available workers.

---

## How do you execute all tests in parallel?

```ts
fullyParallel: true
```

---

## How do you execute only one file serially?

```ts
test.describe.configure({
    mode: "serial"
});
```

---

## Difference between `workers` and `fullyParallel`

| Workers                                       | fullyParallel                                 |
| --------------------------------------------- | --------------------------------------------- |
| Controls how many workers run simultaneously. | Allows tests within files to run in parallel. |

---

## What happens if one serial test fails?

Remaining tests in that serial group are skipped.

---

## Can we override workers from terminal?

Yes.

```bash
npx playwright test --workers=2
```

---

# Quick Revision

| Method                | Purpose                                                  |
| --------------------- | -------------------------------------------------------- |
| `fullyParallel: true` | Run all tests in parallel.                               |
| `workers: 4`          | Use 4 workers.                                           |
| `--workers=2`         | Override workers from terminal.                          |
| `mode: "parallel"`    | Parallel execution for a file/describe block.            |
| `mode: "serial"`      | Sequential execution; stop remaining tests if one fails. |
| `mode: "default"`     | Restore normal Playwright behavior.                      |

---

# Memory Trick

```text
workers

↓

How MANY workers?

------------------------

fullyParallel

↓

Should tests run together?

------------------------

mode: "parallel"

↓

This file → Parallel

------------------------

mode: "serial"

↓

This file → One by one

Stop if one fails.
```
