# Playwright Codegen Notes

## What is Codegen?

**Codegen** is Playwright's built-in recorder that generates automation scripts by recording your interactions with a web application.

It is useful for:

* Learning Playwright syntax
* Quickly generating test scripts
* Finding robust locators
* Creating assertions
* Inspecting elements
* Debugging locator issues

> **Note:** Codegen is a productivity tool. In real projects, generated code is usually cleaned up and refactored.

---

# Launching Codegen

Basic command:

```bash
npx playwright codegen
```

Open a specific website:

```bash
npx playwright codegen https://testautomationpractice.blogspot.com/
```

Open in Chromium:

```bash
npx playwright codegen --browser chromium
```

Open in Firefox:

```bash
npx playwright codegen --browser firefox
```

Open in WebKit:

```bash
npx playwright codegen --browser webkit
```

Save generated code directly:

```bash
npx playwright codegen --output tests/demo.spec.ts
```

Generate code in JavaScript:

```bash
npx playwright codegen --target javascript
```

Generate code in TypeScript (default):

```bash
npx playwright codegen --target playwright-test
```

---

# Codegen Window

When Codegen starts, two windows appear:

### 1. Browser Window

Used to interact with the application.

Example actions:

* Click
* Fill
* Select dropdown
* Check checkbox
* Navigate pages

Every action is automatically recorded.

---

### 2. Inspector (Codegen) Window

Displays:

* Generated Playwright code
* Toolbar buttons
* Locator picker
* Assertion tools

---

# Codegen Toolbar Features

---

## 1. Record

**Purpose**

Starts or resumes recording user actions.

While recording:

* Clicks
* Typing
* Dropdown selection
* Checkbox selection
* Navigation

are automatically converted into Playwright code.

Example generated code:

```ts
await page.getByRole('button', { name: 'Submit' }).click();
```

---

## 2. Pause Recording

Stops recording temporarily.

Useful when:

* Performing unwanted actions
* Logging into the application manually
* Preparing test data

Recording can be resumed later.

---

## 3. Pick Locator

Shortcut:

Click the **Pick Locator** button.

Purpose:

Inspect any element on the page.

When hovering over elements, Codegen displays:

* Best locator
* Locator priority
* Generated Playwright locator

Example:

```ts
page.getByRole('button', { name: 'Submit' })
```

instead of

```ts
page.locator("#submit")
```

Useful for learning Playwright locator strategies.

---

## 4. Assert Visibility

Purpose:

Generate visibility assertions.

Steps:

1. Click **Assert Visibility**
2. Select an element

Generated code:

```ts
await expect(locator).toBeVisible();
```

Use case:

Verify elements are displayed.

---

## 5. Assert Text

Purpose:

Verify displayed text.

Steps:

1. Click **Assert Text**
2. Select an element

Generated code:

```ts
await expect(locator).toHaveText("Expected Text");
```

Useful for:

* Labels
* Headers
* Messages
* Buttons

---

## 6. Assert Value

Purpose:

Verify textbox or input values.

Example:

```ts
await expect(locator).toHaveValue("Aniket");
```

Useful for:

* Input fields
* Textareas
* Hidden inputs

---

## 7. Assert Checked

Purpose:

Verify checkbox or radio button state.

Generated code:

```ts
await expect(locator).toBeChecked();
```

Useful for:

* Checkboxes
* Radio buttons

---

## 8. Assert Screenshot

Purpose:

Capture a screenshot assertion.

Generated code:

```ts
await expect(page).toHaveScreenshot();
```

Useful for:

* Visual regression testing
* UI comparison

---

## 9. Copy Locator

After selecting an element,

Codegen lets you copy the generated locator.

Example:

```ts
page.getByLabel("Name")
```

Useful for quickly creating locators without recording actions.

---

## 10. Clear Code

Removes all generated code from the Inspector.

Useful when starting a new recording session.

---

# Locator Generation Priority

Codegen tries to generate the most reliable locator.

Preferred order:

1. `getByRole()`
2. `getByLabel()`
3. `getByPlaceholder()`
4. `getByText()`
5. `getByAltText()`
6. `getByTitle()`
7. `getByTestId()`
8. CSS locator
9. XPath (last preference)

Example:

Preferred:

```ts
page.getByRole('button', { name: 'Submit' })
```

Instead of:

```ts
page.locator("//button[text()='Submit']")
```

---

# Debugging Using Codegen

Useful for:

* Checking whether locators identify the correct element
* Understanding generated Playwright syntax
* Exploring dynamic web pages
* Learning locator strategies

---

# Common Workflow

1. Start Codegen

```bash
npx playwright codegen
```

2. Perform actions

* Click
* Type
* Select dropdown
* Check checkbox

3. Copy generated code.

4. Refactor:

* Create reusable locators
* Remove unnecessary waits
* Add assertions
* Improve readability

---

# Advantages

* Fast script generation
* Excellent learning tool
* Generates reliable locators
* Helps beginners understand Playwright APIs
* Useful for debugging locator issues

---

# Limitations

* Generated code is not always production-ready.
* May contain repeated locators.
* Does not automatically create reusable Page Object Models.
* Requires manual cleanup and optimization.

---

# Best Practices

* Use Codegen to generate the initial script.
* Replace duplicate locators with variables.
* Add meaningful assertions.
* Remove unnecessary generated code.
* Prefer semantic locators (`getByRole`, `getByLabel`) when possible.
* Refactor into Page Object Model for larger projects.

---

# Interview Questions

### What is Codegen?

A Playwright tool that records browser interactions and automatically generates Playwright test code.

---

### Why is Codegen useful?

* Script generation
* Locator generation
* Learning Playwright
* Creating assertions
* Debugging locators

---

### Does Codegen generate production-ready scripts?

No.

It provides a good starting point, but the generated code should be reviewed and refactored.

---

### Which locator types does Codegen prefer?

1. getByRole()
2. getByLabel()
3. getByPlaceholder()
4. getByText()
5. getByAltText()
6. getByTitle()
7. getByTestId()
8. CSS
9. XPath

---

### Can Codegen generate assertions?

Yes.

It can generate:

* `toBeVisible()`
* `toHaveText()`
* `toHaveValue()`
* `toBeChecked()`
* `toHaveScreenshot()`

---

### Is Codegen useful in interviews?

Yes.

Many interviewers expect candidates to know:

* How to launch Codegen
* How to inspect locators
* How to generate assertions

---

## Debugging with Codegen

Codegen can also be used to **debug existing Playwright tests** interactively.

### Debug Command

Run a specific test in debug mode:

```bash
npx playwright test tests/example.spec.ts --debug
```

Or simply:

```bash
PWDEBUG=1 npx playwright test
```

This opens the **Playwright Inspector**, where you can debug your test step by step.

---

## Features Available in Debug Mode

### ▶ Resume

Continues execution until the next breakpoint or the end of the test.

---

### ⏭ Step Over

Executes the current statement and moves to the next one.

Useful for understanding the execution flow line by line.

---

### ⏩ Step Into

If the current line calls another function, enters that function and pauses at its first line.

Useful when debugging helper methods or Page Object Model (POM) methods.

---

### ⏪ Step Out

Completes execution of the current function and returns to the caller.

Useful when you no longer want to debug inside a function.

---

### ⏸ Pause

Temporarily pauses the execution.

You can inspect:

* Current page
* Locators
* Variables
* Browser state

---

### Locator Highlighting

When paused, selecting a locator in the Inspector highlights the corresponding element in the browser.

This helps verify that the locator points to the correct element.

---

### Live Locator Editing

You can modify a locator directly in the Inspector and immediately see whether it matches the desired element.

This is very useful for troubleshooting locator issues.

---

## When to Use Debug Mode

Use debug mode when:

* A locator is failing.
* An assertion is failing.
* The page is loading slowly.
* You want to understand the test execution flow.
* You need to inspect element states before or after an action.

---

## Difference Between Codegen and Debug Mode

| Codegen                                                | Debug Mode                                                                      |
| ------------------------------------------------------ | ------------------------------------------------------------------------------- |
| Records browser actions and generates Playwright code. | Executes an existing Playwright test step by step.                              |
| Mainly used to create scripts and discover locators.   | Mainly used to troubleshoot and inspect test execution.                         |
| Started using `npx playwright codegen`.                | Started using `npx playwright test --debug` or `PWDEBUG=1 npx playwright test`. |

---

# Reference Video

For a complete walkthrough of Playwright Codegen and Inspector features, watch:

**YouTube:** https://youtu.be/HF3Og7o_xSA?si=fXyAArgY0qUaHWMV

* How to use it for rapid test creation
* Its advantages and limitations
