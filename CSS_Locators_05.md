# Playwright CSS Locators

## What are CSS Locators?

CSS Selectors are used to locate HTML elements based on:

* Tag Name
* ID
* Class
* Attributes
* Parent-Child Relationships

CSS locators are generally:

* Faster than XPath
* Easier to read
* Widely used in automation

---

# Types of CSS Locators

1. Relative CSS Locator
2. Absolute CSS Locator

---

# 1. Relative CSS Locator

## Definition

A Relative CSS Locator identifies an element using its attributes without specifying the complete path from the root.

### Advantages

* Short
* Easy to maintain
* Recommended in automation projects

---

# 1.1 tag#id

## Syntax

```css
tagname#id
```

## Example

```css
div#crosscol
```

Matches:

```html
<div id="crosscol">
```

## Playwright Example

```ts
let tagId = page.locator("div#crosscol");

console.log(await tagId.allInnerTexts());
```

---

# 1.2 tag.class

## Syntax

```css
tagname.classname
```

## Example

```css
div.form-group
```

Matches:

```html
<div class="form-group">
```

## Playwright Example

```ts
let tagClass = page.locator("div.form-group");

console.log(await tagClass.allInnerTexts());
```

---

# 1.3 tag[attr='value']

## Syntax

```css
tagname[attribute='value']
```

## Example

```css
button[name='start']
```

Matches:

```html
<button name="start">
```

## Playwright Example

```ts
let tagAttr = page.locator(
    "button[name='start']"
);

await tagAttr.click();

console.log(
    "Clicked on the button with name='start'"
);
```

---

# Absolute CSS Locator

## Definition

An Absolute CSS Locator starts from the root element and follows the complete hierarchy to reach the target element.

## Syntax

```css
parent > child > child > child
```

## Example

```css
body > div:nth-child(4) > div:nth-child(2)
```

## Playwright Example

```ts
let absoluteCss = page.locator(
    "body>div:nth-child(4)>div:nth-child(2)"
);

console.log(
    await absoluteCss.allInnerTexts()
);
```

---

# nth-child()

## Definition

Used to select a specific child element based on its position.

## Syntax

```css
tag:nth-child(index)
```

## Example

```css
div:nth-child(2)
```

Selects:

```text
Second child element
```

---

## Example HTML

```html
<div>First</div>
<div>Second</div>
<div>Third</div>
```

Locator:

```css
div:nth-child(2)
```

Output:

```text
Second
```

---

# CSS Combinators

## Direct Child (>)

### Syntax

```css
parent > child
```

### Example

```css
body > div
```

Meaning:

```text
Select direct child div elements of body
```

---

# Common CSS Patterns

## By ID

```css
div#crosscol
```

---

## By Class

```css
div.form-group
```

---

## By Attribute

```css
button[name='start']
```

---

## Direct Child

```css
body > div
```

---

## nth-child

```css
div:nth-child(2)
```

---

# Relative vs Absolute CSS

| Relative CSS            | Absolute CSS           |
| ----------------------- | ---------------------- |
| Short                   | Long                   |
| Easy to maintain        | Difficult to maintain  |
| Preferred in automation | Rarely used            |
| Uses attributes         | Uses complete DOM path |

### Relative Example

```css
button[name='start']
```

### Absolute Example

```css
body > div:nth-child(4) > div:nth-child(2)
```

---

# Common Playwright Methods Used

## Create Locator

```ts
page.locator("div.form-group");
```

---

## Click Element

```ts
await locator.click();
```

---

## Get Inner Text

### Single Element

```ts
await locator.innerText();
```

---

### Multiple Elements

```ts
await locator.allInnerTexts();
```

---

# Interview Questions

### What are CSS Locators?

CSS Locators are selectors used to locate HTML elements using:

* Tag
* ID
* Class
* Attribute
* DOM hierarchy

---

### What is the syntax for locating an element by ID?

```css
tagname#id
```

Example:

```css
div#crosscol
```

---

### What is the syntax for locating an element by Class?

```css
tagname.classname
```

Example:

```css
div.form-group
```

---

### What is the syntax for locating an element by Attribute?

```css
tagname[attribute='value']
```

Example:

```css
button[name='start']
```

---

### What is nth-child()?

Used to locate an element based on its position among siblings.

Example:

```css
div:nth-child(2)
```

---

### Which CSS Locator is preferred in automation?

```text
Relative CSS Locator
```

because it is:

* Short
* Readable
* Easy to maintain

---

### Which CSS Locator should generally be avoided?

```text
Absolute CSS Locator
```

because small DOM changes can break the locator.

CSS Locators --- VVIP Interview Questions + Answers
=================================================

### 1\. What are CSS Locators in Playwright?

> CSS locators are used to identify elements using CSS selectors based on things like ID, class, attributes, and relationships between elements. In Playwright, we use them with `page.locator()`.

Example:

const loginButton = page.locator("button#login");

* * * * *

### 2\. How do you locate an element by ID using CSS?

> We use `#` followed by the ID.

Example:

#username

In Playwright:

page.locator("#username");

We can also use:

input#username

* * * * *

### 3\. How do you locate an element by class?

> We use `.` followed by the class name.

Example:

.form-control

In Playwright:

page.locator(".form-control");

If we know the tag as well:

input.form-control

* * * * *

### 4\. How do you locate an element using an attribute?

> We use the attribute inside square brackets.

Example:

button[name='login']

In Playwright:

page.locator("button[name='login']");

This is especially useful when the element has a stable attribute but no unique ID.

* * * * *

### 5\. What is the difference between `>` and a space in CSS selectors?

**VVIP question.**

> `>` selects only a **direct child**, while a space selects an element that can be anywhere inside the parent, including nested elements.

Example:

div.user > button

means the button must be a direct child of `div.user`.

Whereas:

div.user button

can find a button nested deeper inside `div.user`.

* * * * *

### 6\. What is `nth-child()` in CSS?

> `nth-child()` is used to select an element based on its position among its siblings.

Example:

div.colors span:nth-child(2)

This selects the second child if it is a `span`.

**Important:** `nth-child()` is **1-based**, so:

nth-child(1) = first

nth-child(2) = second

nth-child(3) = third

* * * * *

### 7\. What is the difference between CSS `nth-child()` and Playwright `nth()`?

**Very important for Playwright.**

> CSS `:nth-child()` is a CSS selector based on the element's position among its siblings, while Playwright's `.nth()` selects an element from the locator's matching elements using a zero-based index.

Example:

div:nth-child(2)

means the element is the **second child**.

But:

page.locator("div").nth(1);

selects the **second matching `div`**, because Playwright indexing starts from `0`.

So:

CSS nth-child(2) → second child

Playwright nth(1) → second matching element

* * * * *

### 8\. What is the difference between Relative CSS and Absolute CSS?

> Relative CSS identifies an element using useful attributes or relationships without depending on the complete DOM hierarchy. Absolute CSS follows the complete DOM hierarchy from the root.

Example of relative CSS:

button#login

Example of absolute CSS:

body > div:nth-child(3) > div:nth-child(2) > button

**I prefer relative CSS because it is generally shorter and easier to maintain.**

* * * * *

### 9\. Why should you avoid Absolute CSS selectors?

> Because they depend heavily on the DOM structure. If a parent element is added, removed, or moved, the selector can break.

For example:

body > div:nth-child(3) > div:nth-child(2) > button

is much more fragile than:

button#login

* * * * *

### 10\. How would you create a CSS locator if an element doesn't have an ID?

> I would look for another stable attribute such as a class, `name`, `data-testid`, or another unique attribute. I can also combine multiple attributes if necessary.

Example:

<input class="form-control" name="username">

I could use:

input[name='username']

or:

input.form-control[name='username']

* * * * *

### 11\. Can you combine multiple CSS conditions?

**Yes.**

Example:

input#email.form-control

This means:

> Find an `input` whose ID is `email` and which has the `form-control` class.

You can also combine attributes:

input[type='text'][name='username']

* * * * *

### 12\. CSS Locator vs XPath --- which one do you prefer?

> If both can reliably identify the element, I generally prefer CSS because it is concise and easy to read. However, in Playwright I would first check whether a built-in locator such as `getByRole()`, `getByLabel()`, or `getByTestId()` is available. I would use XPath when I need more complex DOM relationships that are easier to express with XPath.

This is a **very good interview answer** because you're showing that you understand Playwright rather than just CSS.

* * * * *

### 13\. CSS Locator vs Playwright Built-in Locator --- which do you prefer?

**VVIP.**

> I prefer Playwright's built-in locators when they provide a stable and unique way to identify the element, such as `getByRole()`, `getByLabel()`, or `getByTestId()`. I use CSS when a stable CSS attribute provides a better or simpler locator.

Example:

page.getByRole("button", { name: "Login" });

would generally be preferred over:

page.locator("button#login");

if the role locator is unique and stable.

Playwright's current documentation recommends prioritizing user-facing locators and explicit contracts such as test IDs over selectors tied closely to DOM structure.

* * * * *

### 14\. Can CSS selectors locate elements using text?

> Standard CSS selectors do not provide XPath-style `text()` functionality. In Playwright, if I need to locate an element based on its visible text, I would generally use `getByText()` or another appropriate Playwright locator.

For example:

page.getByText("Login");

rather than trying to create an XPath-style CSS selector.

* * * * *

### 15\. What would you do if your CSS locator matches multiple elements?

> First, I would try to make the locator more specific by using a stable attribute or combining conditions. If multiple matching elements are actually expected, I can use Playwright methods such as `.nth()`, `.first()`, or `.last()`.

Example:

page.locator(".form-control").nth(1);

But I would avoid using `.nth()` just to hide a bad locator if a unique and stable locator can be created.
