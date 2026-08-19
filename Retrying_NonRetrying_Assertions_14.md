# Playwright - Auto Waiting (Actions vs Assertions)

## What is Auto Waiting?

Playwright automatically waits for elements or page conditions before performing actions or assertions.

This reduces the need for explicit waits like:

```ts
await page.waitForTimeout(5000);
```

Playwright waits only as long as needed and proceeds immediately once the condition is satisfied.

---

# Default Timeouts

## Test Timeout

* **Default:** 30 seconds

Applicable to actions such as:

* `click()`
* `fill()`
* `check()`
* `uncheck()`
* `selectOption()`
* `hover()`

Example:

```ts
await page.locator("#male").check({
    timeout: 7000
});
```

Playwright waits up to **7 seconds** for the element to become actionable.

---

## Assertion Timeout

* **Default:** 5 seconds

Applicable to Playwright assertions:

* `toHaveTitle()`
* `toHaveURL()`
* `toHaveText()`
* `toHaveValue()`
* `toBeVisible()`
* `toBeChecked()`

Example:

```ts
await expect(locator).toBeVisible({
    timeout: 7000
});
```

Playwright keeps retrying until the assertion passes or the timeout expires.

---

# Retrying Assertions

These assertions automatically retry.

---

## Verify Page Title

```ts
await expect(page).toHaveTitle(
    "Automation Testing Practice",
    { timeout: 7000 }
);
```

✔ Retries until the page title matches.

---

## Verify URL

```ts
await expect(page).toHaveURL(
    "https://testautomationpractice.blogspot.com/",
    { timeout: 7000 }
);
```

✔ Retries until the URL matches.

---

## Verify Input Value

```ts
await expect(name).toHaveValue(
    "Aniket",
    { timeout: 7000 }
);
```

✔ Retries until the textbox contains the expected value.

---

## Verify Text

```ts
await expect(header).toHaveText(
    "Automation Testing Practice",
    { timeout: 7000 }
);
```

✔ Retries until the text matches.

---

## Verify Button Text

```ts
await expect(startBtn).toHaveText(
    "START",
    { timeout: 7000 }
);
```

✔ Retries until the button displays the expected text.

---

## Verify Checkbox State

Initially:

```ts
await expect(sunday).not.toBeChecked({
    timeout: 7000
});
```

After checking:

```ts
await sunday.check();

await expect(sunday).toBeChecked({
    timeout: 7000
});
```

✔ Retries until the checkbox becomes checked.

---

## Verify Radio Button

```ts
await male.check();

await expect(male).toBeChecked({
    timeout: 7000
});
```

✔ Retries until the radio button is selected.

---

## Verify Dropdown Selection

```ts
await canada.selectOption("canada");

await expect(canada).toHaveValue(
    "canada",
    { timeout: 7000 }
);
```

✔ Retries until the selected value matches.

---

## Verify Visibility

```ts
await expect(
    page.locator("//input[@type='submit']")
).toBeVisible({
    timeout: 7000
});
```

✔ Retries until the element becomes visible.

---

# Non-Retrying Methods

The following methods **do not retry**.

They return the current value immediately.

---

## isChecked()

```ts
console.log(await male.isChecked());
```

Returns:

```text
true
```

or

```text
false
```

✔ Returns a boolean.

❌ Does not retry.

---

## textContent()

```ts
console.log(await startBtn.textContent());
```

Returns the current text immediately.

Example:

```text
START
```

❌ Does not retry.

---

## title()

```ts
console.log(await page.title());
```

Returns the current page title.

❌ Does not retry.

---

## url()

```ts
console.log(page.url());
```

Returns the current URL.

❌ Does not retry.

---

## inputValue()

```ts
console.log(await name.inputValue());
```

Returns the current value of the textbox.

❌ Does not retry.

---

# Actions with Custom Timeout

Actions also support a custom timeout.

---

## fill()

```ts
await name.fill("Aniket", {
    timeout: 7000
});
```

Waits until the textbox becomes editable.

---

## check()

```ts
await sunday.check({
    timeout: 7000
});
```

Waits until the checkbox becomes actionable.

---

## selectOption()

```ts
await canada.selectOption("canada", {
    timeout: 7000
});
```

Waits until the dropdown is available.

---

## click()

```ts
await startBtn.click({
    timeout: 7000
});
```

Waits until the button becomes clickable.

---

# Action Timeout vs Assertion Timeout

| Action Timeout                                                           | Assertion Timeout                               |
| ------------------------------------------------------------------------ | ----------------------------------------------- |
| Waits until an action can be performed.                                  | Waits until an expected condition becomes true. |
| Used with actions like `click()`, `fill()`, `check()`, `selectOption()`. | Used with `expect()` assertions.                |
| Default: **0 seconds** (rely on 30 sec test titmeout)                    | Default: **5 seconds**                          |
| Supports custom timeout.                                                 | Supports custom timeout.                        |

---

# Retrying vs Non-Retrying

## Retrying

* `toHaveTitle()`
* `toHaveURL()`
* `toHaveText()`
* `toHaveValue()`
* `toBeChecked()`
* `toBeVisible()`
* `toBeHidden()`
* `toHaveAttribute()`
* `toHaveCount()`

These assertions automatically retry until they pass or the timeout expires.

---

## Non-Retrying

* `page.title()`
* `page.url()`
* `locator.textContent()`
* `locator.innerText()`
* `locator.inputValue()`
* `locator.isChecked()`
* `locator.isVisible()`
* `locator.isEnabled()`

These methods return the current state immediately without retrying.

---

# Best Practices

* Prefer Playwright's **web-first assertions** (`await expect(...)`) whenever possible.
* Avoid using `waitForTimeout()` in production tests.
* Use action timeouts only when necessary.
* Use assertion timeouts when waiting for dynamic UI changes.

---

# Interview Questions

### 1. What is Auto Waiting?

Playwright automatically waits for elements or page conditions before performing actions or assertions.

---

### 2. What is the default action timeout?

**0 seconds.** rely on test default timeout of 30 sec.

---

### 3. What is the default assertion timeout?

**5 seconds**

---

### 4. Does `isChecked()` retry?

❌ No.

It immediately returns `true` or `false`.

---

### 5. Which is better?

```ts
expect(await male.isChecked()).toBe(true);
```

or

```ts
await expect(male).toBeChecked();
```

✅ Prefer:

```ts
await expect(male).toBeChecked();
```

because it is a retrying Playwright assertion.

---

### 6. Can actions have custom timeouts?

✅ Yes.

Example:

```ts
await locator.click({
    timeout: 10000
});
```

---

### 7. Can `page.title()` have a timeout?

❌ No.

Use:

```ts
await expect(page).toHaveTitle("Expected Title", {
    timeout: 10000
});
```

instead.

Playwright Actions vs Assertions --- VVIP Q&A
===========================================

Q1 ⭐⭐⭐⭐⭐
--------

### What is the difference between auto-waiting for actions and auto-retrying assertions?

**Answer:**

> For actions, Playwright waits for the target element to satisfy the required actionability conditions before performing the action. For assertions, Playwright repeatedly checks the expected condition until it passes or the assertion timeout expires.

Example:

await button.click();

Playwright waits for the button to become actionable.

Whereas:

await expect(status).toHaveText("Success");

Playwright keeps checking until the text becomes `"Success"`.

**Easy memory:**

Action     → Wait until actionable

Assertion  → Retry until expected condition is true

Playwright officially documents these as two related but distinct mechanisms.

* * * * *

Q2 ⭐⭐⭐⭐⭐
--------

### What are actionability checks in Playwright?

**Answer:**

Before actions such as `click()`, Playwright performs relevant checks. For `click()`, these include:

-   Locator resolves to exactly one element
-   Element is visible
-   Element is stable
-   Element receives pointer events
-   Element is enabled

Only after those checks pass does Playwright perform the click.

Example:

await page.locator("#submit").click();

**Interview one-liner:**

> **Actionability checks ensure that the element is actually ready to receive the requested user action.**

* * * * *

Q3 ⭐⭐⭐⭐⭐
--------

### What is the difference between `isChecked()` and `toBeChecked()`?

expect(await checkbox.isChecked()).toBe(true);

vs.

await expect(checkbox).toBeChecked();

**Answer:**

`isChecked()` is a **getter**. It returns the current state:

true

or

false

It doesn't retry until the checkbox becomes checked.

`toBeChecked()` is a **Playwright web-first assertion**. It automatically retries until the checkbox becomes checked or the assertion timeout is reached.

Therefore, for UI synchronization, prefer:

await expect(checkbox).toBeChecked();

* * * * *

Q4 ⭐⭐⭐⭐⭐
========

### What is the difference between `page.title()` + `expect()` and `toHaveTitle()`?

expect(await page.title()).toBe("Google");

vs.

await expect(page).toHaveTitle("Google");

**Answer:**

The first one:

expect(await page.title()).toBe("Google");

gets the title **once** and then checks that returned value.

The second:

await expect(page).toHaveTitle("Google");

is a **web-first assertion** and automatically retries until the title matches or the assertion timeout expires.

### Interview one-liner:

> **`page.title()` gives me the current value, while `toHaveTitle()` is a retrying Playwright assertion designed for dynamic UI conditions.**

* * * * *

Q5 ⭐⭐⭐⭐⭐
========

### Why are web-first assertions preferred in Playwright?

**Answer:**

Because web applications are often asynchronous.

For example:

Loading

   ↓

Processing

   ↓

Success

If I write:

expect(await status.textContent()).toBe("Success");

the text is retrieved once. If it is still `"Processing"` at that moment, the test can fail.

Instead:

await expect(status).toHaveText("Success");

Playwright retries the assertion until `"Success"` appears or the timeout expires.

This reduces timing-related flaky tests.

* * * * *

Q6 ⭐⭐⭐⭐⭐
========

### Does `locator.textContent()` retry?

**Answer:**

No.

const text = await locator.textContent();

returns the current text value. It doesn't keep retrying until some expected text appears.

If I want to verify dynamic text, I would use:

await expect(locator).toHaveText("Success");

**Interview distinction:**

textContent()        → Get current value

toHaveText()         → Retry expected condition

* * * * *

Q7 ⭐⭐⭐⭐⭐
========

### Does `locator.isVisible()` automatically wait until an element becomes visible?

**Answer:**

No. It returns the current visibility state.

const visible = await locator.isVisible();

returns:

true

or:

false

If I want Playwright to wait until the element becomes visible, I use:

await expect(locator).toBeVisible();

`toBeVisible()` is a retrying web-first assertion.

* * * * *

Q8 ⭐⭐⭐⭐⭐
========

### What is the default assertion timeout in Playwright?

**Answer:**

The default assertion timeout is **5 seconds**.

Example:

await expect(locator).toBeVisible();

If the condition doesn't become true within the assertion timeout, the assertion fails.

We can customize it:

await expect(locator).toBeVisible({

    timeout: 10000

});

Now the assertion can retry for up to 10 seconds.

* * * * *

Q9 ⭐⭐⭐⭐⭐
========

### If I specify a 10-second assertion timeout, does Playwright always wait 10 seconds?

**Answer:**

**No.**

Suppose:

await expect(status).toHaveText("Success", {

    timeout: 10000

});

If `"Success"` appears after 3 seconds:

0 sec → check

1 sec → retry

2 sec → retry

3 sec → Success ✅

             ↓

           PASS

It does **not** continue waiting until 10 seconds.

> **The timeout is the maximum time Playwright is allowed to retry, not a fixed waiting period.**

This is exactly the same principle you learned with auto-waiting.

* * * * *

Q10 ⭐⭐⭐⭐⭐
=========

### What is the difference between `waitForTimeout()` and Playwright's auto-waiting/assertions?

**Answer:**

`waitForTimeout()` is a **fixed wait**.

await page.waitForTimeout(5000);

It waits 5 seconds regardless of whether the application becomes ready after 1 second or 5 seconds.

Playwright's auto-waiting and assertions are condition-based.

await button.click();

or:

await expect(status).toHaveText("Success");

They wait only until the required condition is satisfied or the applicable timeout is reached.

**Interview one-liner:**

> **`waitForTimeout()` waits for a fixed duration, whereas Playwright's auto-waiting and web-first assertions wait based on the actual application state.**

This is a common interview topic because avoiding hard-coded waits is a core Playwright practice.

* * * * *

Q11 ⭐⭐⭐⭐⭐
=========

### What are the default Playwright timeouts?

**Answer:**

For Playwright Test, the commonly relevant defaults are:

| Timeout | Default |
| --- | --- |
| **Test timeout** | 30 seconds |
| **Assertion timeout** | 5 seconds |
| **Action timeout** | 0 |
| **Navigation timeout** | 0 |

But remember: **these are configurable**.

For example, your current company project has:

Test timeout       → 15 minutes

Action timeout     → 60 seconds

Assertion timeout  → 15 seconds

Navigation timeout → 2 minutes

So in an interview, first give the Playwright defaults, then mention that real projects commonly override them.

* * * * *

Q12 ⭐⭐⭐⭐⭐
=========

### What is the difference between test timeout, action timeout, and assertion timeout?

**Answer:**

### Test timeout

Controls the **overall execution time of the test**.

Entire test

   ↓

Actions + assertions + other operations

   ↓

Must stay within test timeout

### Action timeout

Controls how long an **individual action** can wait for the required actionability conditions.

await button.click();

### Assertion timeout

Controls how long a **retrying assertion** can retry.

await expect(status).toHaveText("Success");

### Interview answer:

> **Test timeout is for the whole test, action timeout is for an individual action, and assertion timeout is for retrying an assertion.**

* * * * *

Q13 ⭐⭐⭐⭐⭐
=========

### Scenario: A button becomes enabled after an API call. It can take 2--10 seconds. How would you automate it?

**Answer:**

I would avoid:

await page.waitForTimeout(10000);

await button.click();

Instead:

await button.click();

Playwright's click action will wait for the button to satisfy the relevant actionability requirements, including being enabled.

If I specifically want to verify the state:

await expect(button).toBeEnabled();

await button.click();

This is useful when **enabled state itself is part of what I want to verify**.

* * * * *

Q14 ⭐⭐⭐⭐⭐
=========

### What happens if a web-first assertion fails to meet the expected condition within its timeout?

**Answer:**

Playwright keeps retrying the assertion until the timeout expires.

If the condition is still not satisfied:

Assertion

   ↓

Retry

   ↓

Retry

   ↓

Retry

   ↓

Timeout reached

   ↓

❌ Assertion fails

For example:

await expect(status).toHaveText("Success", {

    timeout: 5000

});

If `"Success"` never appears within 5 seconds, the assertion fails.

* * * * *

Q15 ⭐⭐⭐⭐⭐
=========

### What are non-retrying assertions, and why can they cause flaky tests?

**Answer:**

Normal assertions such as:

expect(value).toBe("Success");

do not automatically retry the condition.

For example:

expect(await status.textContent()).toBe("Success");

`textContent()` gets the value at that moment.

If the application changes from:

Processing → Success

a moment later, the assertion may fail even though the application eventually reaches the correct state.

For dynamic web UI, Playwright recommends web-first assertions such as:

await expect(status).toHaveText("Success");

because they retry automatically.
