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

By default, Playwright waits for **30 seconds** for test, for action it is zero.

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

🔥 Playwright Auto-Waiting --- 10 VVIP Interview Q&A
==================================================

### Q1 ⭐⭐⭐⭐⭐ What is auto-waiting in Playwright? How does it make tests more reliable?

**Answer:**

> Playwright has built-in auto-waiting. Before performing an action, Playwright automatically waits for the target element to satisfy the required actionability conditions, instead of making us add explicit waits.
>
> For example, before clicking a button, Playwright waits for it to be visible, stable, enabled, able to receive pointer events, and uniquely resolved.
>
> This reduces flaky tests caused by timing issues in dynamic web applications.

Example:

await page.locator("#submit").click();

If the button appears after 3 seconds, Playwright waits for it instead of immediately failing.

**Interview one-liner:**

> **Auto-waiting means Playwright automatically waits for elements to become ready for an action, which reduces synchronization issues and flaky tests.**

* * * * *

### Q2 ⭐⭐⭐⭐⭐ What actionability checks does Playwright perform before `locator.click()`?

**Answer:**

Before `click()`, Playwright performs several checks:

1.  **Locator resolves to exactly one element**
2.  Element is **visible**
3.  Element is **stable** --- not currently moving/animating
4.  Element **receives pointer events**
5.  Element is **enabled**

Only after these checks pass does Playwright perform the click.

await page.locator("#submit").click();

Conceptually:

Find element

     ↓

Exactly one?

     ↓

Visible?

     ↓

Stable?

     ↓

Receives events?

     ↓

Enabled?

     ↓

CLICK

**Interview one-liner:**

> **Before clicking, Playwright waits for the element to be uniquely resolved, visible, stable, enabled, and able to receive pointer events.**

* * * * *

### Q3 ⭐⭐⭐⭐⭐ Does `page.locator()` itself wait for an element?

**Answer:**

> **No. `page.locator()` itself does not wait for the element to exist. It creates a Locator object, which is a lazy reference to the element.**
>
> The actual element resolution happens when we perform an action or use an auto-retrying assertion.

Example:

const button = page.locator("#submit");

This does **not** mean Playwright immediately searches the DOM and waits for `#submit`.

It creates the locator.

Then:

await button.click();

Now Playwright resolves the locator and performs the required actionability checks.

Your **box analogy** is actually perfect for remembering this:

page.locator("#submit")

        ↓

📦 Creates a Locator

        ↓

No need to check element yet

        ↓

button.click()

        ↓

🔍 Find + check + wait

        ↓

CLICK

**Interview one-liner:**

> **A Locator is lazy. Creating it doesn't require the element to exist; Playwright resolves it and waits when we use it in an action or assertion.**

* * * * *

### Q4 ⭐⭐⭐⭐⭐ What is the difference between auto-waiting for actions and auto-retrying assertions?

**Answer:**

They are related but serve different purposes.

### Action

await page.locator("#submit").click();

Playwright waits for the element to become **actionable** before performing the click.

### Assertion

await expect(page.locator("#status")).toHaveText("Success");

Playwright repeatedly checks whether the expected condition is satisfied until it passes or the assertion timeout is reached.

So:

| Action | Assertion |
| --- | --- |
| `click()` | `toBeVisible()` |
| `fill()` | `toHaveText()` |
| `check()` | `toHaveValue()` |
| Waits for actionability | Retries expected condition |

**Interview one-liner:**

> **Actions auto-wait for actionability, while web-first assertions automatically retry the expected condition until it passes or times out.**

* * * * *

### Q5 ⭐⭐⭐⭐⭐ What is the difference between these two?

expect(await page.title()).toBe("Google");

and

await expect(page).toHaveTitle("Google");

**Answer:**

The first one is a **normal assertion**.

expect(await page.title()).toBe("Google");

First:

await page.title()

gets the title **once**.

Then:

expect(...).toBe(...)

checks that value.

There is no retry of the assertion.

The second one:

await expect(page).toHaveTitle("Google");

is a **Playwright web-first assertion**.

Playwright automatically retries the assertion until the title becomes `"Google"` or the assertion timeout is reached.

### Easy way to remember:

expect(await page.title())

        ↓

Get value once

        ↓

Check once

versus:

await expect(page).toHaveTitle()

        ↓

Check

 ↓

Retry

 ↓

Retry

 ↓

Pass / Timeout

**Interview one-liner:**

> **`expect(await page.title())` checks a value that was retrieved once, whereas `toHaveTitle()` is a web-first assertion that automatically retries until the expected condition is met.**

* * * * *

### Q6 ⭐⭐⭐⭐ Why should we avoid `waitForTimeout()` in real automation tests?

**Answer:**

`waitForTimeout()` is a **hard-coded wait**.

await page.waitForTimeout(5000);

It always waits 5 seconds regardless of whether the application is ready.

Suppose the element becomes ready in 1 second:

Element ready → 1 sec

Still waiting → 2

Still waiting → 3

Still waiting → 4

Still waiting → 5

We've wasted 4 seconds.

Or suppose it takes 7 seconds:

Wait → 5 seconds

Element still not ready

        ↓

Test may fail

So it can make tests both **slow and flaky**.

Instead:

await page.locator("#submit").click();

allows Playwright to wait only as long as necessary for the action.

**Interview one-liner:**

> **`waitForTimeout()` is a fixed delay, so it can make tests unnecessarily slow and still fail if the application takes longer than the specified time. Playwright's auto-waiting is preferable because it waits based on the actual state of the element.**

* * * * *

### Q7 ⭐⭐⭐⭐⭐ What are the default timeout values in Playwright?

This one is important because interviewers may try to confuse you.

The commonly relevant defaults are:

| Timeout | Default |
| --- | --- |
| **Test timeout** | **30 seconds** |
| **Assertion timeout** | **5 seconds** |
| **Action timeout** | **0** |
| **Navigation timeout** | **0** |

The important meaning of `0` here is **no separate timeout is configured for that category by default**; it doesn't mean Playwright doesn't wait.

You can configure them in `playwright.config.ts`.

For example:

export default defineConfig({

  timeout: 30_000,

  expect: {

    timeout: 5_000

  },

  use: {

    actionTimeout: 10_000,

    navigationTimeout: 30_000

  }

});

**Interview one-liner:**

> **By default, the test timeout is 30 seconds, assertion timeout is 5 seconds, while action and navigation timeouts are 0 unless configured separately.**

⚠️ **Don't say "action timeout is zero, therefore Playwright doesn't wait."** That's incorrect. Auto-waiting still happens; `0` means there's no separately configured action timeout.

* * * * *

### Q8 ⭐⭐⭐⭐ Suppose an element appears dynamically after 8 seconds. What happens here?

await page.locator("#submit").click();

**Answer:**

First:

page.locator("#submit")

creates the locator. It doesn't immediately wait.

Then:

click()

starts the action.

Playwright tries to find the element and waits for the required actionability conditions.

If the element appears after 8 seconds and becomes actionable within the applicable timeout:

click()

  ↓

Element not found

  ↓

Wait/retry

  ↓

Wait/retry

  ↓

8 seconds

  ↓

Element appears

  ↓

Actionability checks pass

  ↓

CLICK ✅

No:

await page.waitForTimeout(8000);

is required.

**Interview one-liner:**

> **When the click action is executed, Playwright automatically waits and retries until the element appears and becomes actionable, provided the operation doesn't exceed its applicable timeout.**

* * * * *

### Q9 ⭐⭐⭐⭐⭐ What is the difference between an auto-retrying assertion and a normal JavaScript assertion?

**Answer:**

A normal assertion evaluates the value that you provide to it.

Example:

expect(await page.title()).toBe("Google");

The title is retrieved first:

await page.title()

Then the result is checked.

If the title is temporarily `"Loading..."`, the assertion can fail immediately.

With a Playwright web-first assertion:

await expect(page).toHaveTitle("Google");

Playwright keeps checking until:

Title = "Google" → PASS

or:

Assertion timeout → FAIL

This is especially useful for dynamic applications where the UI doesn't update instantly.

**Interview one-liner:**

> **A normal assertion checks the current value, whereas a Playwright web-first assertion automatically retries the condition, making it suitable for asynchronous UI changes.**

* * * * *

### Q10 ⭐⭐⭐⭐⭐ Real-world scenario: Button is initially disabled and becomes enabled after an API call. Sometimes it takes 2 seconds, sometimes 7 seconds. How would you automate it?

Suppose we have:

<button id="submit" disabled>Submit</button>

and after the API call finishes:

<button id="submit">Submit</button>

We can simply do:

await page.locator("#submit").click();

Playwright's click action will wait for the button to become actionable, including becoming enabled.

We don't need:

await page.waitForTimeout(7000);

The flow is:

Button disabled

      ↓

click()

      ↓

Playwright waits

      ↓

API completes

      ↓

Button enabled

      ↓

Actionability checks pass

      ↓

CLICK ✅

If you specifically want to verify the state before clicking, you could also write:

const submitButton = page.locator("#submit");

await expect(submitButton).toBeEnabled();

await submitButton.click();

Here the assertion itself retries until the button is enabled.

**Interview answer:**

> **I would avoid a fixed wait. I would use the locator action directly because Playwright automatically waits for the button to become actionable. If I specifically need to validate the state, I can use `await expect(locator).toBeEnabled()` before clicking.**

## ⭐ Interview question you should now be ready for

An interviewer might ask:

### "What is the difference between test timeout, action timeout, and assertion timeout in Playwright?"

Your answer can be:

"Test timeout is the maximum time allowed for the entire test. Action timeout controls how long an individual action such as click or fill can wait for the element to become actionable. Assertion timeout controls how long a web-first assertion can retry before failing. These can all be configured independently."

Then if they ask:

### "What are the defaults?"

Say:

"By default, Playwright Test has a 30-second test timeout and 5-second assertion timeout. Action and navigation timeouts are 0 by default, meaning no separate timeout is configured. However, these values can be customized according to the application."

And if they ask:

### "What is your current project configuration?"

You can give your actual real-world answer:

"In my current project, the test timeout is 15 minutes, action timeout is 60 seconds, navigation timeout is 2 minutes, and assertion timeout is 15 seconds, because we're testing a CAD application where operations can take longer."
