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
