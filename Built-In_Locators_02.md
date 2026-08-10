# Playwright Built-in Locators

## 1. getByAltText()

### Definition

`getByAltText()` is used to locate elements based on their **alt attribute**.

It is commonly used for locating images.

### Syntax

```ts
page.getByAltText("alt text")
```

### Example

```ts
import { test, expect } from "@playwright/test";

test("Verify getByAltText()", async ({ page }) => {

    await page.goto("https://webflow.com/made-in-webflow/demo");

    let logo = page.getByAltText("author avatar").first();

    await expect(logo).toBeVisible();

    console.log("Logo is visible on the page");
});
```

### HTML Example

```html
<img alt="author avatar">
```

---

## 2. getByText()

### Definition

`getByText()` is used to locate elements based on their visible text content.

Useful for:

* Headers
* Links
* Buttons
* Paragraphs

### Syntax

```ts
page.getByText("Visible Text")
```

### Example

```ts
import { test, expect } from "@playwright/test";

test("Verify getByText()", async ({ page }) => {

    await page.goto("https://webflow.com/made-in-webflow/demo");

    let header = page.getByText("built by the Webflow community").first();

    await expect(header).toBeVisible();

    console.log("Header is visible on the page");
});
```

### HTML Example

```html
<h1>built by the Webflow community</h1>
```

---

## 3. getByRole()

### Definition

`getByRole()` is used to locate elements based on their accessibility role.

Common roles:

* link
* button
* heading
* textbox
* checkbox
* radio

### Syntax

```ts
page.getByRole(role, { name: "Accessible Name" })
```

### Example

```ts
import { test, expect } from "@playwright/test";

test("Verify getByRole()", async ({ page }) => {

    await page.goto("https://testautomationpractice.blogspot.com/");

    let role = page.getByRole("link", {
        name: "Udemy Courses"
    });

    await expect(role).toBeVisible();

    await role.click();

    console.log("Role is clicked");

    let role2 = page.getByRole("heading", {
        name: "Udemy Courses"
    });

    await expect(role2).toBeVisible();

    console.log("Heading is visible");
});
```

### HTML Example

```html
<a href="#">Udemy Courses</a>

<h2>Udemy Courses</h2>
```

---

## 4. getByLabel()

### Definition

`getByLabel()` is used to locate form controls using their associated label text.

Useful for:

* Textboxes
* Textareas
* Checkboxes
* Radio Buttons
* Dropdowns

### Syntax

```ts
page.getByLabel("Label Text")
```

### Example

```ts
import { test, expect } from "@playwright/test";

test("Verify getByLabel()", async ({ page }) => {

    await page.goto("https://testautomationpractice.blogspot.com/");

    let address = page.getByLabel("Address:");

    await address.fill("Aniket");

    await expect(address).toHaveValue("Aniket");

    console.log("Address value verified");
});
```

### HTML Example

```html
<label for="address">Address:</label>

<textarea id="address"></textarea>
```

### Important Note

For `getByLabel()` to work:

```html
for="address"
```

must match:

```html
id="address"
```

---

## 5. getByPlaceholder()

### Definition

`getByPlaceholder()` is used to locate input fields using their placeholder text.

### Syntax

```ts
page.getByPlaceholder("Placeholder Text")
```

### Example

```ts
import { test, expect } from "@playwright/test";

test("Verify getByPlaceholder()", async ({ page }) => {

    await page.goto("https://testautomationpractice.blogspot.com/");

    let name = page.getByPlaceholder("Enter Name");

    await name.fill("Aniket");

    console.log("Name is filled");

    let email = page.getByPlaceholder("Enter EMail");

    await email.fill("aniketpawar@gmail.com");

    console.log("Email is filled");
});
```

### HTML Example

```html
<input placeholder="Enter Name">

<input placeholder="Enter EMail">
```

---

# Summary

| Locator            | Used For                                 |
| ------------------ | ---------------------------------------- |
| getByAltText()     | Locate elements using alt attribute      |
| getByText()        | Locate elements using visible text       |
| getByRole()        | Locate elements using accessibility role |
| getByLabel()       | Locate form controls using label text    |
| getByPlaceholder() | Locate inputs using placeholder text     |

---

# Interview Questions

### What is getByAltText() used for?

Used to locate elements based on their alt attribute, commonly images.

### What is getByText() used for?

Used to locate elements using visible text.

### What is getByRole() used for?

Used to locate elements using accessibility roles such as link, button, heading, textbox, etc.

### What is getByLabel() used for?

Used to locate form controls using associated label text.

### What is getByPlaceholder() used for?

Used to locate input elements using placeholder text.

Playwright Built-in Locators --- Interview Questions + Answers
============================================================

### 1\. What are built-in locators in Playwright?

**Answer:**

> Playwright provides built-in locator methods such as `getByRole()`, `getByText()`, `getByLabel()`, `getByPlaceholder()`, and `getByAltText()` to locate elements using user-facing or accessibility-related information. They are generally more readable and maintainable than relying heavily on CSS or XPath.

* * * * *

### 2\. What is `getByRole()`?

**Answer:**

> `getByRole()` locates an element based on its accessibility role, such as button, link, textbox, checkbox, or heading. We can also specify the accessible name to make the locator more specific.

Example:

```
page.getByRole("button", { name: "Login" });
```

* * * * *

### 3\. Why do you prefer `getByRole()`?

**Answer:**

> I prefer `getByRole()` when possible because it represents how a user or assistive technology identifies the element. It also makes the locator more readable and usually more stable than depending on CSS classes or XPath.

* * * * *

### 4\. What is `getByText()`?

**Answer:**

> `getByText()` locates an element based on its visible text.

Example:

```
page.getByText("Welcome to Dashboard");
```

I would use it when the visible text itself is a good way to identify the element.

* * * * *

### 5\. What is `getByLabel()`?

**Answer:**

> `getByLabel()` locates form controls using their associated label text. It is useful for inputs, checkboxes, radio buttons, and other form controls.

Example:

```
page.getByLabel("Username");
```

For example, if `"Username"` is associated with an input, Playwright can locate that input using the label.

* * * * *

### 6\. What is `getByPlaceholder()`?

**Answer:**

> `getByPlaceholder()` locates an input using its placeholder text.

Example:

```
page.getByPlaceholder("Enter Email");
```

I would use it when the input has a meaningful and stable placeholder.

* * * * *

### 7\. What is `getByAltText()`?

**Answer:**

> `getByAltText()` locates elements, commonly images, using their `alt` attribute.

Example:

```
page.getByAltText("Company Logo");
```

* * * * *

### 8\. What is the difference between `getByText()` and `getByRole()`?

**Answer:**

> `getByText()` identifies an element using its visible text, whereas `getByRole()` identifies it using its accessibility role and optionally its accessible name.

For example:

```
page.getByText("Login");
```

versus:

```
page.getByRole("button", { name: "Login" });
```

If I know the element is a button, I would generally prefer `getByRole()` because it is more specific.

* * * * *

### 9\. What is the difference between `getByLabel()` and `getByPlaceholder()`?

**Answer:**

> `getByLabel()` uses the associated label of a form control, while `getByPlaceholder()` uses the placeholder text inside the input.

For example:

```
page.getByLabel("Email");
```

uses the label, while:

```
page.getByPlaceholder("Enter Email");
```

uses the placeholder.

* * * * *

### 10\. Which locator would you use for a Login button?

Suppose we have:

```
<button>Login</button>
```

**Answer:**

```
page.getByRole("button", { name: "Login" });
```

I would prefer this because the element's role and accessible name clearly identify it.

* * * * *

### 11\. What if multiple elements have the same text?

For example:

```
<button>Submit</button>
<a>Submit</a>
```

**Answer:**

> I would make the locator more specific by using the element's role.

```
page.getByRole("button", { name: "Submit" });
```

This specifically targets the button instead of simply searching for the text `"Submit"`.

* * * * *

### 12\. Which Playwright locator would you prefer?

**Answer:**

> I generally prefer Playwright's user-facing locators such as `getByRole()`, `getByLabel()`, `getByText()`, and `getByPlaceholder()` when they provide a unique and stable way to identify the element. If they are not suitable, I can use CSS, XPath, or other locator strategies depending on the application.

* * * * *

### 13\. Which locator would you use for an input with a label?

Given:

```
<label for="username">Username</label>
<input id="username">
```

**Answer:**

```
page.getByLabel("Username");
```

* * * * *

### 14\. Which locator would you use for an input with a placeholder?

Given:

```
<input placeholder="Enter Name">
```

**Answer:**

```
page.getByPlaceholder("Enter Name");
```

* * * * *

### 15\. Which locator would you use for an image?

Given:

```
<img alt="Company Logo">
```

**Answer:**

```
page.getByAltText("Company Logo");
```
