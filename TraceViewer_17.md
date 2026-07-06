# Playwright - Trace Viewer Notes

## What is Trace Viewer?

**Trace Viewer** is Playwright's debugging tool that records everything that happens during test execution.

It helps you analyze test failures by showing:

* 📸 Screenshots at every step
* 🎥 Video (if enabled)
* 🖱️ User actions (click, fill, select, etc.)
* 🌐 Network requests
* 📝 Console logs
* ❌ Error messages
* ⏱️ Timeline of execution
* 📍 DOM snapshot before and after each action

Think of it as a **"time machine"** for your test.

---

# Why Use Trace Viewer?

Instead of rerunning a failed test multiple times, you can inspect exactly what happened.

Useful for:

* Debugging failed tests
* Investigating flaky tests
* Checking locator failures
* Verifying page state at each step
* Understanding test execution flow

---

# Enable Trace from Command Line

Run a specific test and generate a trace:

```bash
npx playwright test tests/TraceViewer_19.spec.ts --project=Chromium --trace on
```

Run all tests with trace:

```bash
npx playwright test --trace on
```

---

# Open Trace Viewer

After execution:

```bash
npx playwright show-trace trace.zip
```

or simply open the generated `trace.zip` file.

---

# Configure Trace Globally

Inside `playwright.config.ts`

```ts
use:{
    trace:'on'
}
```

---

# Trace Options

---

## 1. `off` (Default)

```ts
trace:'off'
```

* No trace is recorded.

Use when:

* Fast execution is required.
* No debugging is needed.

---

## 2. `on`

```ts
trace:'on'
```

* Records a trace for **every test**.
* Passed and failed tests both generate traces.

Use when:

* Learning Playwright
* Local debugging
* Small test suites

---

## 3. `retain-on-failure` ⭐ (Most Common)

```ts
trace:'retain-on-failure'
```

* Records every test.
* Deletes traces for passed tests.
* Keeps traces only for failed tests.

Example

| Test | Trace     |
| ---- | --------- |
| Pass | ❌ Deleted |
| Fail | ✅ Saved   |

Best for:

* CI/CD
* Real-world automation projects

---

## 4. `on-first-retry`

```ts
trace:'on-first-retry'
```

Requires retries.

Example

```ts
retries:1
```

Behavior

| Attempt     | Trace |
| ----------- | ----- |
| First Run   | ❌     |
| First Retry | ✅     |

Best for:

* Flaky test investigation
* Saving storage

---

## 5. `on-all-retries`

```ts
trace:'on-all-retries'
```

Requires retries.

Behavior

| Attempt   | Trace |
| --------- | ----- |
| First Run | ❌     |
| Retry 1   | ✅     |
| Retry 2   | ✅     |

Useful when:

* Multiple retries are configured.
* You want to analyze every retry attempt.

---

# Where are Traces Stored?

Inside

```text
test-results/
```

Example

```text
test-results/

    Login-Test/

        trace.zip
```

---

# What Can You See Inside Trace Viewer?

## Timeline

Shows every Playwright action in execution order.

Example

```
goto()

↓

fill()

↓

click()

↓

expect()
```

---

## DOM Snapshot

Shows the exact HTML at that point in time.

Useful when:

* Locator fails
* Element disappears
* Dynamic UI changes

---

## Before & After Screenshots

Each action records page snapshots.

You can visually inspect what changed.

---

## Action Details

Click any action to see:

* Locator used
* Execution time
* Target element
* Parameters
* Result

---

## Network

Shows:

* API requests
* Responses
* Status codes
* Failed requests

Useful for API debugging.

---

## Console Logs

Displays browser console logs.

Useful for:

* JavaScript errors
* Console warnings
* Debug messages

---

## Source Code

Highlights the exact test line being executed.

Makes debugging much easier.

---

## Metadata

Shows:

* Browser
* OS
* Playwright version
* Test duration

---

# Difference Between Trace Viewer & HTML Report

| HTML Report                               | Trace Viewer                                                                       |
| ----------------------------------------- | ---------------------------------------------------------------------------------- |
| Shows overall test results.               | Shows detailed execution of a single test.                                         |
| Summary of passed/failed tests.           | Step-by-step debugging.                                                            |
| Contains screenshots/videos (if enabled). | Includes DOM snapshots, timeline, network, console, actions, screenshots and more. |

---

# Difference Between Video & Trace

| Video                    | Trace                                           |
| ------------------------ | ----------------------------------------------- |
| Screen recording only.   | Complete debugging information.                 |
| Shows what happened.     | Shows what happened **and why**.                |
| Cannot inspect locators. | Can inspect locators, DOM, network and console. |

---

# Recommended Settings

## Local Learning

```ts
trace:'on'
```

---

## CI/CD

```ts
trace:'retain-on-failure'
```

---

## Flaky Tests

```ts
trace:'on-first-retry'
```

---

# Quick Revision

| Option              | When is Trace Saved? |
| ------------------- | -------------------- |
| `off`               | Never                |
| `on`                | Every test           |
| `retain-on-failure` | Failed tests only    |
| `on-first-retry`    | First retry only     |
| `on-all-retries`    | Every retry          |

---

# Common Commands

Generate trace

```bash
npx playwright test tests/TraceViewer_19.spec.ts --project=Chromium --trace on
```

Generate trace for all tests

```bash
npx playwright test --trace on
```

Open Trace Viewer

```bash
npx playwright show-report -> then check trace zip file.
npx playwright show-trace trace.zip
```

Open HTML Report

```bash
npx playwright show-report
```

---

# Interview Questions

### What is Trace Viewer?

A Playwright debugging tool that records the complete execution of a test, including actions, DOM snapshots, screenshots, network activity, console logs, and timeline.

---

### Where is Trace configured?

Inside `playwright.config.ts`

```ts
use:{
    trace:'retain-on-failure'
}
```

or from the terminal:

```bash
npx playwright test --trace on
```

---

### Which trace option is most commonly used in real projects?

```ts
trace:'retain-on-failure'
```

---

### Which trace option is best for flaky tests?

```ts
trace:'on-first-retry'
```

---

### What is the difference between Video and Trace?

* **Video** records only the screen.
* **Trace** records the screen **plus** DOM snapshots, locator details, network requests, console logs, source code, and execution timeline.

---

## Memory Tip

**Screenshot → Single Image**

**Video → Screen Recording**

**Trace → Complete Test History (Time Machine)**
