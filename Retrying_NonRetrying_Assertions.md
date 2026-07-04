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

## Action Timeout

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
| Default: **30 seconds**                                                  | Default: **5 seconds**                          |
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

**30 seconds**

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
