# Playwright - Session 1 Notes

## What is Playwright?

Playwright is an automation testing framework developed by Microsoft.

It is used to automate web browsers and perform end-to-end testing of web applications.

---

## Supported Browsers

- Chromium (Chrome, Edge)
- Firefox
- WebKit (Safari)

---

## Supported Languages

- TypeScript
- JavaScript
- Python
- Java
- C#

---

# Advantages of Playwright

- Auto Waiting
- Fast Execution
- Cross Browser Testing
- Cross Platform Support
- Supports Multiple Tabs and Windows
- Supports Frames and IFrames
- API Testing Support
- Parallel Test Execution
- Built-in Reporting

---

# Project Structure

```text
Project Folder
│
├── tests
│
├── playwright.config.ts
│
├── package.json
│
├── package-lock.json
│
└── node_modules
```

---

## tests Folder

Contains all test files.

Example:

```text
login.spec.ts
search.spec.ts
```

---

## playwright.config.ts

Main configuration file.

Used for:

- Browser settings
- Timeout settings
- Base URL
- Reporting configuration

---

## package.json

Contains:

- Project information
- Dependencies
- Scripts

---

## node_modules

Contains all installed packages required by the project.

Examples:

- Playwright
- TypeScript
- Other dependencies

---

# First Playwright Test

```ts
import { test, expect } from '@playwright/test';

test('First Test', async ({ page }) => {

    await page.goto('https://example.com');

    await expect(page).toHaveTitle(/Example/);
});
```

---

# Understanding the Test

## test()

Used to create a test case.

```ts
test("Test Name", async ({ page }) => {

});
```

---

## page

Represents a browser tab/page.

Example:

```ts
await page.goto("https://example.com");
```

---

## expect()

Used for validation (assertions).

Example:

```ts
await expect(page).toHaveTitle(/Example/);
```

---

# Important Concepts

## Browser

Actual browser application.

Examples:

- Chrome
- Edge
- Firefox

---

## Browser Context

Acts like a fresh browser profile.

Used to isolate tests from each other.

Each context has its own:

- Cookies
- Local Storage
- Session Storage

---

## Page

Represents a browser tab.

Example:

```ts
const page = await context.newPage();
```

---

# Async/Await in Playwright

Most Playwright actions are asynchronous.

Example:

```ts
await page.goto("https://example.com");
```

Why?

Because browser actions take time to complete.

Without `await`, the next statement may execute before the current action finishes.

---

# Example

```ts
await page.goto("https://google.com");

await page.fill("#username", "admin");

await page.click("#login");
```

Execution:

```text
Open page
Wait for page load

Fill username
Wait for completion

Click login
Wait for completion
```

---

# Session 1 Interview Questions

### What is Playwright?

Playwright is a browser automation framework developed by Microsoft for end-to-end testing.

---

### Which browsers does Playwright support?

- Chromium
- Firefox
- WebKit

---

### What is page in Playwright?

`page` represents a browser tab.

---

### What is expect() used for?

Used for assertions/validations.

---

### Why do we use await in Playwright?

To ensure browser actions complete before moving to the next statement.

---

### What is Browser Context?

A Browser Context is an isolated browser session with its own cookies, cache, and storage.

---

# Key Takeaways

- Playwright is developed by Microsoft.
- Supports Chromium, Firefox, and WebKit.
- Tests are written using `test()`.
- Assertions are written using `expect()`.
- Browser tab is represented by `page`.
- Browser actions generally use `await`.
- Browser Context provides test isolation.


Topic 1 --- Interview Questions + Answers
---------------------------------------

### 1\. What is Playwright?

> **Playwright is a browser automation and end-to-end testing framework developed by Microsoft. It allows us to automate web applications across Chromium, Firefox, and WebKit.**

### 2\. Which browsers does Playwright support?

> **Playwright supports Chromium, Firefox, and WebKit. Chromium is used for browsers such as Chrome and Edge, while WebKit is used for Safari-like browser testing.**

### 3\. What is `page` in Playwright?

> **`page` represents a browser tab or page. We use it to perform actions such as navigating to a URL, locating elements, clicking, filling fields, and interacting with the web page.**

### 4\. What is `expect()` used for in Playwright?

> **`expect()` is used to perform assertions or validations in Playwright. For example, we can verify the page title, URL, visibility of an element, or its text.**

Example:

```
await expect(page).toHaveTitle("Automation Testing Practice");
```

### 5\. Why do we use `async/await` in Playwright?

> **Most Playwright actions are asynchronous and return Promises. We use `await` to wait for an asynchronous operation to complete before moving to the next statement. The test or function containing `await` is marked as `async`.**

### 6\. What is a Browser Context in Playwright?

> **A Browser Context is an isolated browser session. It has its own cookies, local storage, session storage, and authentication state, which helps keep tests independent from each other.**

### 7\. What is the difference between Browser, Browser Context, and Page?

> **Browser represents the browser instance, such as Chromium, Firefox, or WebKit. Browser Context represents an isolated browser session, and Page represents a browser tab within that context.**

### 8\. Why is Playwright preferred for automation testing?

> **Playwright provides features such as auto-waiting, cross-browser testing, parallel execution, multiple tab and window handling, iframe support, API testing, and built-in test reporting. These features make it suitable for modern end-to-end automation.**
