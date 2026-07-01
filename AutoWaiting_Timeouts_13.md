# Playwright - Auto Waiting

## What is Auto Waiting?

**Auto Waiting** is one of Playwright's most powerful features.

Before performing an action or assertion, Playwright **automatically waits** for the required condition to be satisfied.

This means you usually **do not need** to write:

```ts
await page.waitForTimeout(5000);
```

or

```ts
await page.waitForSelector(...);
```

---

# Default Auto-Wait Timeout

By default, Playwright waits for **30 seconds** for most actions and navigations.

Examples:

* `click()`
* `fill()`
* `check()`
* `selectOption()`
* `locator()`
* `goto()`

If the condition is not met within **30 seconds**, Playwright throws a `TimeoutError`.

---

# Auto Waiting in Assertions

Playwright assertions (`expect`) automatically retry until they pass.

Default assertion timeout:

```text
5 seconds
```

Examples:

```ts
await expect(locator).toBeVisible();

await expect(locator).toHaveText("Success");

await expect(locator).toHaveValue("Aniket");
```

Playwright keeps checking the condition until:

* ✅ It becomes true
* ❌ 5 seconds expire

---

# Example Code

```ts
import { test, expect } from "@playwright/test";

test("Verify Autowaits", async ({ page }) => {

    await page.goto("https://testautomationpractice.blogspot.com/");

    expect(await page.title()).toBe("Automation Testing Practice");

    await expect(page).toHaveURL("https://testautomationpractice.blogspot.com/");

    await page.locator(".wikipedia-search-input").fill("Playwright");

    await page.locator(".wikipedia-search-button").click();

});
```

---

# How Auto Waiting Works in This Example

## Step 1

```ts
await page.goto("https://testautomationpractice.blogspot.com/");
```

### What Playwright waits for

* Browser navigation starts.
* Waits until the page reaches the default **load** state.
* Only then does it move to the next line.

If the page takes 8 seconds to load:

```text
Playwright waits 8 seconds automatically.
```

No manual wait is required.

---

## Step 2

```ts
expect(await page.title()).toBe("Automation Testing Practice");
```

### Does this use Auto Waiting?

❌ No.

Reason:

```ts
await page.title()
```

immediately fetches the current page title.

If the title hasn't updated yet, this statement does **not retry**.

It simply checks the value once.

---

## Better Alternative

```ts
await expect(page).toHaveTitle("Automation Testing Practice");
```

This **does** use auto waiting.

Playwright keeps checking the title until it matches (up to the assertion timeout).

---

## Step 3

```ts
await expect(page).toHaveURL(
    "https://testautomationpractice.blogspot.com/"
);
```

### Auto Waiting

Playwright keeps checking:

```text
Current URL

↓

Expected URL?

↓

No

↓

Check again

↓

Check again

↓

Pass
```

It retries automatically until:

* URL matches
* or assertion timeout expires

---

## Step 4

```ts
await page
    .locator(".wikipedia-search-input")
    .fill("Playwright");
```

Before filling the textbox, Playwright automatically waits for the element to:

* Exist in the DOM
* Be visible
* Be enabled
* Be editable
* Be stable (not moving due to animations)

Only then does it type:

```text
Playwright
```

---

## Step 5

```ts
await page
    .locator(".wikipedia-search-button")
    .click();
```

Before clicking, Playwright automatically waits until the button is:

* Attached to the DOM
* Visible
* Enabled
* Stable
* Able to receive pointer events (not covered by another element)

Then it performs the click.

---

# What Playwright Waits For

| Action           | Auto Waits For                                        |
| ---------------- | ----------------------------------------------------- |
| `click()`        | Visible, enabled, stable, receives events             |
| `fill()`         | Visible, editable, enabled                            |
| `check()`        | Visible, enabled                                      |
| `selectOption()` | `<select>` element available and enabled              |
| `hover()`        | Visible and stable                                    |
| `goto()`         | Navigation completes                                  |
| `locator()`      | Lazily resolves when used with an action or assertion |

---

# Actions vs Assertions

## Actions

Default timeout:

```text
30 seconds
```

Examples:

```ts
click()

fill()

check()

selectOption()
```

---

## Assertions

Default timeout:

```text
5 seconds
```

Examples:

```ts
await expect(locator).toBeVisible();

await expect(locator).toHaveText(...);

await expect(locator).toHaveValue(...);
```

Assertions automatically retry until they pass or time out.

---

# Why Auto Waiting Is Useful

Without auto waiting:

```ts
await page.waitForTimeout(5000);

await page.click("#submit");
```

You are guessing that 5 seconds is enough.

With Playwright:

```ts
await page.locator("#submit").click();
```

Playwright waits only as long as needed.

If the button becomes clickable after:

* 1 second → clicks after 1 second.
* 8 seconds → waits 8 seconds and clicks.
* Never becomes clickable → fails after the timeout.

---

# Best Practices

* Prefer Playwright's built-in auto waiting over `waitForTimeout()`.
* Use Playwright assertions such as:

  * `toBeVisible()`
  * `toHaveText()`
  * `toHaveValue()`
  * `toHaveTitle()`
  * `toHaveURL()`
* Avoid fixed delays except for debugging or demonstrations.
* Prefer `await expect(page).toHaveTitle(...)` over `expect(await page.title()).toBe(...)` because the former automatically retries.

---

# Interview Questions

### 1. What is Auto Waiting?

Auto Waiting is Playwright's mechanism for automatically waiting until an element or page reaches the required state before performing an action or assertion.

---

### 2. What is the default timeout for actions?

**30 seconds.**

---

### 3. What is the default timeout for Playwright assertions?

**5 seconds.**

---

### 4. Does `page.click()` wait automatically?

✅ Yes.

It waits for the element to become actionable before clicking.

---

### 5. Which is better?

```ts
expect(await page.title()).toBe("Google");
```

or

```ts
await expect(page).toHaveTitle("Google");
```

✅ The second one (`toHaveTitle`) is preferred because it supports Playwright's automatic retry mechanism.
