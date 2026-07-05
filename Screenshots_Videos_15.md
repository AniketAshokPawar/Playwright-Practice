# Playwright - Screenshots & Videos

## Introduction

Playwright provides built-in support for capturing:

* 📸 Screenshots
* 🎥 Videos

These can be captured:

1. **Manually in test code**
2. **Automatically using `playwright.config.ts`**

---

# 1. Capturing Screenshots Using Test Code

## Full Page Screenshot

Captures the **entire webpage**, including content outside the visible viewport.

```ts
await page.screenshot({
    path: "Screenshots/Screenshot1.png",
    fullPage: true
});
```

**Output**

```
Screenshots/
    Screenshot1.png
```

---

## Viewport Screenshot

Captures **only the visible portion** of the webpage.

```ts
await page.screenshot({
    path: "Screenshots/Screenshot2.png"
});
```

Since `fullPage` is omitted, Playwright captures only the current viewport.

---

## Element Screenshot

Captures only a specific element.

```ts
let sections = page.locator("div.fauxborder-left.tabs-fauxborder-left");

await sections.screenshot({
    path: "Screenshots/Screenshot3.png"
});
```

Only the selected element is captured.

---

# Screenshot Methods

## Page Screenshot

```ts
await page.screenshot();
```

Captures the entire page or viewport.

---

## Locator Screenshot

```ts
await locator.screenshot();
```

Captures only the specified element.

---

# Common Screenshot Options

| Option           | Description                                        |
| ---------------- | -------------------------------------------------- |
| `path`           | File location where the screenshot will be saved.  |
| `fullPage`       | Captures the complete webpage. Default is `false`. |
| `type`           | Image format (`png` or `jpeg`).                    |
| `quality`        | JPEG quality (0–100). Applicable only for JPEG.    |
| `omitBackground` | Makes the background transparent (PNG only).       |

---

# Example

```ts
await page.screenshot({
    path: "Screenshots/HomePage.png",
    fullPage: true
});
```

---

# 2. Automatic Screenshots Using `playwright.config.ts`

Instead of writing screenshot code in every test, screenshots can be configured globally.

```ts
use: {

    screenshot: 'off'

}
```

---

## Screenshot Options

---

### 1. `off` (Default)

```ts
screenshot: 'off'
```

* Screenshots are disabled.
* No screenshots are generated.

---

### 2. `on`

```ts
screenshot: 'on'
```

* Takes a screenshot for **every test**.
* Screenshots are saved regardless of pass or fail.

---

### 3. `only-on-failure`

```ts
screenshot: 'only-on-failure'
```

* Screenshots are captured **only when a test fails**.

Example:

| Test | Screenshot |
| ---- | ---------- |
| Pass | ❌          |
| Fail | ✅          |

---

### 4. `on-first-failure`

```ts
screenshot: 'on-first-failure'
```

* Takes a screenshot only on the **first failed attempt**.
* If retries are enabled, later failed retries do not generate additional screenshots.

Useful for reducing duplicate screenshots.

---

# Where are Screenshots Stored?

Automatically generated screenshots are stored inside:

```
test-results/
```

Example:

```
test-results/
    Verify-login/
        test-failed-1.png
```

---

# 3. Recording Videos

Playwright can automatically record videos during test execution.

Configuration:

```ts
use: {

    video: 'on'

}
```

---

# Video Options

---

## 1. `on`

```ts
video: 'on'
```

* Records a video for **every test**.
* Passed and failed tests both have videos.

Example:

| Test | Video |
| ---- | ----- |
| Pass | ✅     |
| Fail | ✅     |

---

## 2. `retain-on-failure`

```ts
video: 'retain-on-failure'
```

* Records every test.
* Deletes videos for passed tests.
* Keeps videos only for failed tests.

Example:

| Test | Video Saved |
| ---- | ----------- |
| Pass | ❌           |
| Fail | ✅           |

**Most commonly used in CI/CD pipelines.**

---

## 3. `on-first-retry`

```ts
video: 'on-first-retry'
```

Requires retries to be enabled.

Example:

```ts
retries: 1
```

Behavior:

| Attempt     | Video |
| ----------- | ----- |
| First Run   | ❌     |
| First Retry | ✅     |

Useful for debugging flaky tests.

---

# Where are Videos Stored?

Videos are stored in:

```
test-results/
```

Example:

```
test-results/
    Login-Test/
        video.webm
```

---

# Viewing Screenshots & Videos

Open the HTML report:

```bash
npx playwright show-report
```

The report displays:

* Screenshots
* Videos
* Trace files (if enabled)
* Error messages
* Execution timeline

---

# Manual vs Global Screenshots

| Manual Screenshot             | Global Screenshot                          |
| ----------------------------- | ------------------------------------------ |
| Written in test code.         | Configured once in `playwright.config.ts`. |
| Captured at any desired step. | Captured automatically.                    |
| Gives precise control.        | Useful for all tests.                      |

---

# Manual vs Global Videos

| Manual           | Global                                       |
| ---------------- | -------------------------------------------- |
| ❌ Not supported. | ✅ Configured through `playwright.config.ts`. |

Videos **cannot** be manually started or stopped using Playwright test code. They are managed through the browser context configuration.

---

# Best Practices

## Screenshots

* Use **manual screenshots** to capture important checkpoints.
* Use **element screenshots** when validating specific UI components.
* Use **global screenshots** for failure analysis in CI.

---

## Videos

* Prefer **`retain-on-failure`** for CI/CD pipelines.
* Use **`on`** during local debugging if you need recordings for every test.
* Use **`on-first-retry`** to investigate flaky tests while saving disk space.

---

# Interview Questions

### 1. How do you capture a full-page screenshot?

```ts
await page.screenshot({
    path: "Screenshots/Home.png",
    fullPage: true
});
```

---

### 2. How do you capture a screenshot of a specific element?

```ts
await locator.screenshot({
    path: "Screenshots/Section.png"
});
```

---

### 3. Where do globally captured screenshots and videos get stored?

```
test-results/
```

---

### 4. Can videos be captured manually using test code?

**No.**

Videos are configured using the Browser Context through `playwright.config.ts`.

---

### 5. Difference between `retain-on-failure` and `on-first-retry`?

| `retain-on-failure`                                        | `on-first-retry`                                                            |
| ---------------------------------------------------------- | --------------------------------------------------------------------------- |
| Records every test but keeps videos only for failed tests. | Records video only during the first retry (requires retries to be enabled). |

---

### 6. Which screenshot option is best for CI/CD?

```
screenshot: 'only-on-failure'
```

---

### 7. Which video option is most commonly used in real projects?

```
video: 'retain-on-failure'
```

or

```
video: 'on-first-retry'
```

depending on whether the team wants failure recordings or recordings only for flaky test retries.
