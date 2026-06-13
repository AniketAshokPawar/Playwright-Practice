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
