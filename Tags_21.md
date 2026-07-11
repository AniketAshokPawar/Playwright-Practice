# Playwright Notes - Tags

---

# What are Tags?

Tags are labels assigned to test cases to **categorize and execute specific groups of tests**.

Instead of running the entire test suite, we can run only:

* Sanity tests
* Regression tests
* Smoke tests
* API tests
* UI tests
* Login tests

---

# Why do we use Tags?

Benefits:

* Execute selected tests.
* Reduce execution time.
* Organize large test suites.
* Useful in CI/CD pipelines.
* Separate Smoke, Sanity and Regression suites.

---

# Syntax

## Single Tag

```ts
test(
    "Sanity Test",
    { tag: "@sanity" },
    async ({ page }) => {

    }
);
```

---

## Multiple Tags

```ts
test(
    "Login Test",
    { tag: ["@sanity", "@regression"] },
    async ({ page }) => {

    }
);
```

A test can have **one or multiple tags**.

---

# Example

## Sanity Test

```ts
test("Sanity test", { tag: "@sanity" }, async ({ page }) => {

});
```

---

## Regression Test

```ts
test("Regression test", { tag: "@regression" }, async ({ page }) => {

});
```

---

## Test with Multiple Tags

```ts
test(
    "Both sanity & regression test",
    {
        tag: ["@sanity", "@regression"]
    },
    async ({ page }) => {

    }
);
```

---

# Running Tagged Tests

## Run all Sanity tests

```bash
npx playwright test tests/Tags_22.spec.ts --grep "@sanity"
```

Runs:

* ✅ Sanity test
* ✅ Both sanity & regression test

---

## Run all Regression tests

```bash
npx playwright test tests/Tags_22.spec.ts --grep "@regression"
```

Runs:

* ✅ Regression test
* ✅ Both sanity & regression test

---

## Run tests having BOTH tags

```bash
npx playwright test tests/Tags_22.spec.ts --grep "(?=.*@sanity)(?=.*@regression)"
```

Runs:

* ✅ Both sanity & regression test

Explanation:

* `(?=.*@sanity)` → Test must contain `@sanity`.
* `(?=.*@regression)` → Test must also contain `@regression`.

---

## Run Sanity tests EXCEPT Regression tests

```bash
npx playwright test tests/Tags_22.spec.ts --grep "@sanity" --grep-invert "@regression"
```

Runs:

* ✅ Sanity test

Does NOT run:

* ❌ Both sanity & regression test

because it also contains `@regression`.

---

# Understanding `--grep`

`--grep` filters tests whose name or tags match the given pattern.

Example

```bash
npx playwright test --grep "@sanity"
```

Only tests with the `@sanity` tag are executed.

---

# Understanding `--grep-invert`

`--grep-invert` excludes matching tests.

Example

```bash
npx playwright test --grep-invert "@regression"
```

Runs every test **except** those tagged with `@regression`.

---

# Common Tag Names

```text
@smoke

@sanity

@regression

@ui

@api

@login

@payment

@checkout

@critical

@negative
```

---

# Difference Between Tags and Groups

| Groups (`test.describe`)                                                  | Tags                                            |
| ------------------------------------------------------------------------- | ----------------------------------------------- |
| Organize related tests in code.                                           | Categorize tests for execution.                 |
| Improve readability.                                                      | Filter which tests to run.                      |
| Cannot directly filter by group name without using `--grep` on the title. | Designed for filtering using tags and `--grep`. |

---

# Best Practices

* Use meaningful tag names.
* Prefix tags with `@`.
* Keep naming consistent across the project.
* Assign multiple tags only when necessary.

Example

```text
@smoke

@sanity

@regression
```

---

# Interview Questions

## What are Tags?

Tags are labels used to categorize tests so that specific subsets of tests can be executed.

---

## Why use Tags?

* Execute selected tests.
* Save execution time.
* Separate Smoke, Sanity, Regression, API, and UI tests.
* Useful for CI/CD pipelines.

---

## Can a test have multiple tags?

**Yes.**

Example

```ts
tag: ["@sanity", "@regression"]
```

---

## How do you run only Sanity tests?

```bash
npx playwright test --grep "@sanity"
```

---

## How do you run tests having both Sanity and Regression tags?

```bash
npx playwright test --grep "(?=.*@sanity)(?=.*@regression)"
```

---

## How do you exclude Regression tests?

```bash
npx playwright test --grep-invert "@regression"
```

---

## Difference between `--grep` and `--grep-invert`

| `--grep`                 | `--grep-invert`          |
| ------------------------ | ------------------------ |
| Includes matching tests. | Excludes matching tests. |

---

# Quick Revision

| Command                                        | Purpose                                                     |
| ---------------------------------------------- | ----------------------------------------------------------- |
| `--grep "@sanity"`                             | Run only Sanity tests.                                      |
| `--grep "@regression"`                         | Run only Regression tests.                                  |
| `--grep "(?=.*@sanity)(?=.*@regression)"`      | Run tests having both tags.                                 |
| `--grep "@sanity" --grep-invert "@regression"` | Run Sanity tests excluding those that also have Regression. |

---

# Memory Trick

```text
Tags = Labels

@sanity      → Sanity Suite

@regression  → Regression Suite

--grep        → Include

--grep-invert → Exclude
```
