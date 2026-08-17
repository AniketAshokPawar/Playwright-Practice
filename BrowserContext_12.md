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

### Q1. What is the difference between Browser, BrowserContext, and Page?

**Interview answer:**

> **Browser** represents the actual browser instance, such as Chromium, Firefox, or WebKit.
>
> **BrowserContext** represents an isolated browser session inside the browser. Each context has its own cookies, local storage, session storage, and other session data. It is mainly used for test isolation.
>
> **Page** represents a single browser tab or page inside a BrowserContext. We perform actions such as `goto()`, `click()`, `fill()`, and locator operations on a Page.

### Simple example

const browser = await chromium.launch();

const context = await browser.newContext();

const page = await context.newPage();

### Q2. Why does Playwright use BrowserContext? What is its purpose?

**Interview answer:**

> **BrowserContext is used to create an isolated browser session within a Browser. Each BrowserContext has its own cookies, local storage, session storage, and other session data. This provides test isolation, so one test's data or authentication state does not affect another test. It can also be used to simulate multiple independent users or sessions within the same browser instance.**

**3\. What is the difference between `context.newPage()` and `context.waitForEvent("page")`? ⭐**

This is particularly important based on what you just practiced.

const page = await context.newPage();

→ **You explicitly create a new page.**

const [newPage] = await Promise.all([

    context.waitForEvent("page"),

    button.click()

]);

→ **The application creates a new page/tab, and you capture it.**

* * * * *

**4\. How do you handle a new tab opened after clicking a button? ⭐⭐⭐**

Typical answer:

const [newPage] = await Promise.all([

    context.waitForEvent("page"),

    button.click()

]);

await newPage.locator("#username").fill("Aniket");

### Q5. Why do we use `Promise.all()` when handling a new tab?

**Interview answer:**

> We use `Promise.all()` to **wait for the new-page event and trigger the action that opens the new tab at the same time**. This ensures that Playwright starts listening for the new page **before** the click happens, so we don't miss the event.

### Example

const [newPage] = await Promise.all([

    context.waitForEvent("page"),

    button.click()

]);

### Q6. How do you handle a popup in Playwright? ⭐⭐⭐

**Interview answer:**

> In Playwright, we handle a popup using the `page.waitForEvent("popup")` event. Since the popup is opened by a specific parent page, we listen for the `popup` event on that parent page and trigger the action that opens it using `Promise.all()`.

### Example

const [popup] = await Promise.all([

    page.waitForEvent("popup"),

    button.click()

]);

await popup.locator("#username").fill("Aniket");

### How it works

Parent Page

    │

    │ button.click()

    ↓

Popup opens

    │

    ↓

"popup" event

    │

    ↓

popup Page object

    │

    ↓

Interact with popup

Once Playwright captures the popup:

await popup.locator("#username").fill("Aniket");

you can treat `popup` like a normal `Page` object.

* * * * *

### ⭐ Difference: `context.waitForEvent("page")` vs `page.waitForEvent("popup")`

|  | `context.waitForEvent("page")` | `page.waitForEvent("popup")` |
| --- | --- | --- |
| Used for | New page/tab | Popup opened by a page |
| Event belongs to | `BrowserContext` | Parent `Page` |
| Example | New Tab | Popup window |
| Code | `context.waitForEvent("page")` | `page.waitForEvent("popup")` |

### New Tab

const [newPage] = await Promise.all([

    context.waitForEvent("page"),

    button.click()

]);

### Popup

const [popup] = await Promise.all([

    page.waitForEvent("popup"),

    button.click()

]);

### ⭐ Very short interview answer

> **For a new tab/page, I use `context.waitForEvent("page")`. For a popup opened by the current page, I use `page.waitForEvent("popup")`. In both cases, I generally use `Promise.all()` so the event listener is registered before triggering the action.**

**Key thing to remember:**\
`context → page event`\
`page → popup event`

**7\. What is the difference between a new tab and a popup in Playwright?**

Know the Playwright API distinction:

| Scenario | Event |
| --- | --- |
| New page created in context | `context.waitForEvent("page")` |
| Popup opened by a page | `page.waitForEvent("popup")` |

Don't focus too much on whether the browser visually shows a "tab" versus "window"; the important part is **which Playwright event you're handling**.

* * * * *

**8\. How do you get all open pages/tabs in a BrowserContext?**

const pages = context.pages();

console.log(pages.length);

And:

for (const page of context.pages()) {

    console.log(await page.title());

}

Be ready for a scenario where you have multiple tabs and need to identify the correct one.

* * * * *

**9\. Can multiple Pages exist inside the same BrowserContext?**

Yes.

const page1 = await context.newPage();

const page2 = await context.newPage();

const page3 = await context.newPage();

All three belong to the same context and share that context's session state.

* * * * *

**10\. ⭐ Scenario Question --- very important**

> A user clicks a button. The application opens a new tab. You need to verify its title and then enter username/password. How would you automate it?

A strong answer:

const [newPage] = await Promise.all([

    context.waitForEvent("page"),

    page.locator("#openTab").click()

]);

await expect(newPage).toHaveTitle("Login");

await newPage.locator("#username").fill("Aniket");

await newPage.locator("#password").fill("password");

Then explain:

> "I register the event listener before clicking because the click triggers creation of the new page. `waitForEvent("page")` captures the newly created Page object, which I can then use for assertions and interactions."
