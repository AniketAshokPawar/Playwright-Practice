# Playwright - Frames (iframe)

## What is an iframe?

An **iframe (Inline Frame)** is an HTML element that embeds another web page inside the current web page.

Elements inside an iframe **cannot be accessed directly** using `page.locator()`. You must switch to the frame using `frameLocator()`.

---

# Frame Hierarchy

```text
Main Page
│
├── Main Page Elements
│     └── #mainName
│
└── Outer Frame (#outerFrame)
      │
      ├── #outerInput
      ├── #country
      ├── #agree
      │
      └── Inner Frame (#innerFrame)
            │
            ├── #innerName
            ├── #colors
            └── #bookTable
```

---

# Count Total Frames

Use `page.frames()` to get all frames on the page.

```ts
console.log(page.frames().length);
```

Example Output:

```text
3
```

(Main Page + Outer Frame + Inner Frame)

---

# Access Elements on the Main Page

Elements on the main page are located using `page.locator()`.

```ts
await page.locator("#mainName").fill("Aniket");
```

---

# Access Elements Inside an iframe

Use `frameLocator()` to switch into an iframe.

```ts
const outerFrame = page.frameLocator("#outerFrame");
```

Now locate elements inside it.

```ts
await outerFrame.locator("#outerInput").fill("Aniket");
```

---

# Select Dropdown Inside Frame

```ts
await outerFrame.locator("#country").selectOption("Canada");

await expect(
    outerFrame.locator("#country")
).toHaveValue("Canada");
```

---

# Checkbox Inside Frame

```ts
await outerFrame.locator("#agree").check();

await expect(
    outerFrame.locator("#agree")
).toBeChecked();
```

---

# Access Nested (Inner) Frame

When one iframe contains another iframe, chain `frameLocator()`.

```ts
const innerFrame = outerFrame.frameLocator("#innerFrame");
```

Now locate elements inside the inner frame.

Example:

```ts
await innerFrame
    .locator("#innerName")
    .fill("Playwright");
```

---

# Checking Frame Availability

```ts
const innerFrame = outerFrame.frameLocator("#innerFrame");

if(innerFrame){
    console.log("Inner frame is present");
}
```

> **Note:** `frameLocator()` always returns a `FrameLocator` object. The above `if` statement does **not** actually verify that the frame exists.

A better way is to verify an element inside the frame:

```ts
await expect(
    innerFrame.locator("#innerName")
).toBeVisible();
```

---

# Common Assertions

### Verify Dropdown Value

```ts
await expect(dropdown).toHaveValue("Canada");
```

---

### Verify Checkbox

```ts
await expect(checkbox).toBeChecked();
```

---

### Verify Element Visibility

```ts
await expect(locator).toBeVisible();
```

---

### Verify Input Value

```ts
await expect(input).toHaveValue("Aniket");
```

---

# Locator Summary

| Location             | Locator                                        |
| -------------------- | ---------------------------------------------- |
| Main page            | `page.locator()`                               |
| Inside one iframe    | `page.frameLocator().locator()`                |
| Inside nested iframe | `page.frameLocator().frameLocator().locator()` |

---

# Best Practices

* Use `page.locator()` only for elements on the main page.
* Use `frameLocator()` for elements inside an iframe.
* Chain `frameLocator()` for nested iframes.
* Prefer assertions like `toBeVisible()`, `toHaveValue()`, and `toBeChecked()` to verify interactions.
* Avoid using `waitForTimeout()` unless it's for learning or debugging; rely on Playwright's auto-waiting in real tests.

---

# Interview Questions

### 1. What is an iframe?

An iframe is an HTML element that embeds another web page inside the current page.

---

### 2. How do you interact with elements inside an iframe?

Using:

```ts
const frame = page.frameLocator("#frameId");
```

---

### 3. How do you access nested iframes?

```ts
const innerFrame = page
    .frameLocator("#outerFrame")
    .frameLocator("#innerFrame");
```

---

### 4. Can `page.locator()` locate elements inside an iframe?

❌ No.

You must first switch to the iframe using `frameLocator()`.

---

### 5. How do you count the total number of frames?

```ts
console.log(page.frames().length);
```

---

# Summary

* `page.locator()` → Main page elements.
* `frameLocator()` → Elements inside an iframe.
* Chain `frameLocator()` for nested iframes.
* Use assertions to validate interactions.
* `page.frames().length` returns the total number of frames on the page.

🔥 Playwright Iframes --- VVIP Interview Questions
------------------------------------------------

### Q1. What is an iframe, and why can't we directly use `page.locator()` for elements inside it?

**Answer:** An iframe is a webpage embedded inside another webpage. Its DOM is a separate document, so we need to enter the frame context before locating elements inside it. In Playwright, `frameLocator()` is the usual approach.

* * * * *

### Q2. How do you handle an iframe in Playwright?

const frame = page.frameLocator("#outerFrame");

await frame.locator("#username").fill("Aniket");

`frameLocator()` returns a `FrameLocator`, which lets you locate and interact with elements inside the iframe.

* * * * *

### Q3. What is the difference between `page.frames()` and `page.frameLocator()`? ⭐⭐⭐

**Answer:**

-   `page.frames()` → returns an array of all `Frame` objects attached to the page.
-   `page.frameLocator()` → creates a `FrameLocator` used to locate/interact with elements inside a specific iframe.

Example:

page.frames()

vs.

page.frameLocator("#outerFrame")

* * * * *

### Q4. Does `page.frames()` include the main page?

**Answer:** **Yes.**

For:

Main Page

 └── Outer iframe

      └── Inner iframe

`page.frames().length` would be **3**:

1.  Main frame
2.  Outer frame
3.  Inner frame

Playwright's `frames()` returns all frames attached to the page.

* * * * *

### Q5. How do you handle a nested iframe? ⭐⭐⭐

Use chained `frameLocator()`:

const innerFrame = page

    .frameLocator("#outerFrame")

    .frameLocator("#innerFrame");

await innerFrame.locator("#innerName").fill("Aniket");

This is one of the **most important practical iframe questions**.

* * * * *

### Q6. What is the difference between `Frame` and `FrameLocator`? ⭐⭐⭐

**Frame** represents an actual frame in Playwright and provides APIs such as `locator()`, `childFrames()`, etc.

**FrameLocator** is a locator-oriented abstraction used to locate elements inside an iframe.

In normal Playwright test automation, `frameLocator()` is generally the simpler approach for interacting with iframe elements.

* * * * *

### Q7. What happens if multiple iframes match the `frameLocator()` selector?

**Answer:** `FrameLocator` is **strict**. If the selector matches multiple frames, an operation can throw a strictness violation. Therefore, make the iframe locator unique or explicitly select the required iframe.

* * * * *

### Q8. Can you use Playwright's built-in locators inside an iframe?

**Yes.** ⭐

For example:

const frame = page.frameLocator("#outerFrame");

await frame.getByRole("button", { name: "Login" }).click();

You can use `getByRole()`, `getByText()`, `getByLabel()`, `getByPlaceholder()`, etc. through a `FrameLocator`.

* * * * *

### Q9. How would you locate an element if the iframe has a dynamic ID?

Don't depend on the dynamic ID. Use a **stable attribute** such as:

<iframe title="Payment Frame">

Then:

const frame = page.frameLocator('iframe[title="Payment Frame"]');

Or use another stable attribute/name available in the application.

**Interview point:** choose a stable locator rather than hardcoding a dynamic value.

* * * * *

### Q10. ⭐⭐⭐ What is the best approach for handling iframes in a real Playwright framework?

**Answer:**

> I prefer `frameLocator()` because it allows me to directly locate and interact with elements inside the iframe without manually switching contexts. For nested iframes, I chain `frameLocator()`. I use `page.frames()` when I specifically need to inspect or work with the frame objects themselves.

Example:

const outerFrame = page.frameLocator("#outerFrame");

const innerFrame = outerFrame.frameLocator("#innerFrame");

await innerFrame.locator("#innerName").fill("Aniket");
