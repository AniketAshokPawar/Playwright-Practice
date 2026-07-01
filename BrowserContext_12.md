# Playwright - Browser, Browser Context & Page

## Browser

A **Browser** represents the browser application (Chromium, Firefox, or WebKit).

```ts
const browser = await chromium.launch();
```

---

## Browser Context

A **Browser Context** is an isolated browser session.

* Each context has its own:

  * Cookies
  * Local Storage
  * Session Storage
* Multiple contexts are completely independent.
* A browser can contain multiple contexts.

```ts
const context = await browser.newContext();
```

---

## Page

A **Page** represents a browser tab.

All user interactions such as `goto()`, `click()`, `fill()`, and `locator()` are performed on a Page.

```ts
const page = await context.newPage();
```

---

# Browser → Context → Page Hierarchy

```text
Browser
│
└── Context
      ├── Page 1
      ├── Page 2
      └── Page 3
```

---

# Creating Multiple Pages in One Context

A single context can contain multiple tabs.

```ts
const browser = await chromium.launch();

const context = await browser.newContext();

const page1 = await context.newPage();
const page2 = await context.newPage();
const page3 = await context.newPage();
```

---

# Navigate to Different Websites

```ts
await page1.goto("https://www.google.com");

await page2.goto("https://www.bing.com");

await page3.goto("https://designmodo.com/website/website-examples/");
```

---

# Verify Page Title

```ts
await expect(page1).toHaveTitle("Google");

await expect(page2).toHaveTitle("Search - Microsoft Bing");

await expect(page3).toHaveTitle("Website Examples - Designmodo");
```

---

# Print Page Title

```ts
console.log(await page1.title());

console.log(await page2.title());

console.log(await page3.title());
```

---

# Count Total Pages in Context

```ts
console.log(context.pages().length);
```

Example Output:

```text
3
```

---

# Handling New Tabs

## What is a New Tab?

A new tab opens **within the same browser context**.

Playwright waits for the new page using:

```ts
context.waitForEvent("page")
```

---

## Example

```ts
const childPage = await Promise.all([
    context.waitForEvent("page"),
    newTab.click()
]);
```

---

## Why `context.waitForEvent("page")`?

A new tab belongs to the **Browser Context**, not to a specific page.

```text
Context
│
├── Parent Page
└── Child Tab
```

---

## Access the New Tab

```ts
console.log(await childPage[0].title());
```

---

# Handling Popups

## What is a Popup?

A popup is a new browser window opened by the current page.

Playwright waits for it using:

```ts
parentPage.waitForEvent("popup")
```

---

## Example

```ts
const childPage = await Promise.all([
    parentPage.waitForEvent("popup"),
    newPop.click()
]);
```

---

## Why `parentPage.waitForEvent("popup")`?

A popup is opened **by the current page**, so the event belongs to the page.

```text
Parent Page
      │
      └── Popup Window
```

---

# Get All Pages in Context

```ts
const allPages = context.pages();
```

---

# Count Total Pages

```ts
console.log(context.pages().length);
```

---

# Print URLs of All Pages

```ts
console.log(context.pages()[0].url());

console.log(context.pages()[1].url());
```

---

# Iterate Through All Pages

```ts
for (const page of context.pages()) {

    console.log(await page.title());

}
```

---

# Perform Action on a Specific Page

Example:

```ts
for (const page of context.pages()) {

    const title = await page.title();

    if (title.includes("Playwright")) {

        await page.locator(".getStarted_Sjon").click();

    }

}
```

---

# Difference Between New Tab and Popup

| New Tab                                    | Popup                                    |
| ------------------------------------------ | ---------------------------------------- |
| Opens another browser tab.                 | Opens a new browser window.              |
| Wait using `context.waitForEvent("page")`. | Wait using `page.waitForEvent("popup")`. |
| Event belongs to **Context**.              | Event belongs to **Parent Page**.        |

---

# Browser → Context → Page Flow

```text
Browser
│
└── Context
      │
      ├── Page 1
      │     │
      │     ├── New Tab
      │     └── Popup
      │
      ├── Page 2
      └── Page 3
```

---

# Best Practices

* Use one **Browser Context** per test for isolation.
* Create multiple **Pages** when working with multiple tabs.
* Use `context.waitForEvent("page")` for new tabs.
* Use `page.waitForEvent("popup")` for popups.
* Use `context.pages()` to access all open pages.

---

# Interview Questions

### 1. What is a Browser Context?

An isolated browser session that maintains its own cookies, local storage, and session storage.

---

### 2. What is a Page?

A browser tab where all automation actions are performed.

---

### 3. How do you create multiple tabs?

```ts
const page1 = await context.newPage();

const page2 = await context.newPage();
```

---

### 4. How do you count the total pages?

```ts
console.log(context.pages().length);
```

---

### 5. How do you handle a newly opened tab?

```ts
const childPage = await Promise.all([
    context.waitForEvent("page"),
    button.click()
]);
```

---

### 6. How do you handle a popup?

```ts
const popup = await Promise.all([
    page.waitForEvent("popup"),
    button.click()
]);
```

---

### 7. Difference between `context.waitForEvent("page")` and `page.waitForEvent("popup")`?

* `context.waitForEvent("page")` → Waits for a **new tab** created within the browser context.
* `page.waitForEvent("popup")` → Waits for a **popup window** opened by the current page.
