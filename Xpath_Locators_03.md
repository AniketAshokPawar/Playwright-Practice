# Playwright XPath Locators

## What is XPath?

XPath (XML Path Language) is used to locate elements in a webpage based on their HTML structure, attributes, and text.

There are two types of XPath:

1. Absolute XPath
2. Relative XPath

---

# 1. Absolute XPath

## Definition

Absolute XPath starts from the root element (`html`) and traverses the complete path to reach the target element.

## Syntax

```xpath
/html/body/div[1]/div[2]/input
```

## Example

```ts
let nameInput = page.locator(
    "xpath=/html/body/div[4]/div[2]/div[2]/div[2]/div[2]/div[2]/div[2]/div/div[4]/div/div/div/div/div/div/div/div/div/div[2]/div[1]/input[1]"
);

await nameInput.fill("Aniket");

await expect(nameInput).toHaveValue("Aniket");
```

## Advantages

* Unique locator
* Easy to generate using browser tools

## Disadvantages

* Very long
* Hard to maintain
* Breaks if page structure changes

---

# 2. Relative XPath

## Definition

Relative XPath starts from any node in the DOM and is more flexible than Absolute XPath.

## Syntax

```xpath
//tagname
```

Example:

```xpath
//input
//button
//label
```

---

# 3. Relative XPath - contains()

## Definition

Used when an attribute contains a partial value.

## Syntax

```xpath
//tagname[contains(@attribute,'value')]
```

## Example

```xpath
//input[contains(@class,'form')]
```

Matches:

```html
<input class="form-control">
```

## Playwright Example

```ts
let allInputs = page.locator(
    "//div/input[contains(@class,'form')]"
);

const count = await allInputs.count();

expect(count).toBe(12);

for(let i=0;i<count;i++){

    let value = await allInputs.nth(i).inputValue();

    console.log(value);
}
```

---

# 4. Relative XPath - starts-with()

## Definition

Used when an attribute starts with a specific value.

## Syntax

```xpath
//tagname[starts-with(@attribute,'value')]
```

## Example

```xpath
//input[starts-with(@class,'form')]
```

Matches:

```html
<input class="form-control">
```

Does NOT match:

```html
<input class="my-form-control">
```

## Playwright Example

```ts
let startWith = page.locator(
    "//div/input[starts-with(@class,'form')]"
);

const count = await startWith.count();

for(let i=0;i<count;i++){

    let value = await startWith.nth(i).inputValue();

    console.log(value);
}
```

---

# 5. Relative XPath - text()

## Definition

Used to locate elements based on exact visible text.

## Syntax

```xpath
//tagname[text()='Exact Text']
```

## Example

```xpath
//label[text()='Colors:']
```

## Playwright Example

```ts
let textColor = page.locator(
    "//label[text()='Colors:']"
);

await expect(textColor).toBeVisible();

await expect(textColor).toHaveText("Colors:");
```

---

# 6. XPath OR Operator

## Definition

Used when an element may contain one of multiple attributes or values.

## Syntax

```xpath
//tagname[@attr='value1' or @attr='value2']
```

## Example

```xpath
//button[@name='start' or @name='stop']
```

This locator works for:

```html
<button name="start">START</button>
```

and

```html
<button name="stop">STOP</button>
```

---

# Handling Dynamic Elements

## Definition

Dynamic elements change their:

* Text
* Attribute values
* State

during execution.

Example:

```text
START → STOP → START
```

## XPath

```xpath
//button[@name='start' or @name='stop']
```

## Playwright Example

```ts
let button = page.locator(
    "//button[@name='start' or @name='stop']"
);

await expect(button).toHaveText("START");

await button.click();

await expect(button).toHaveText("STOP");
```

---

# Common XPath Functions

| Function      | Syntax                                    |
| ------------- | ----------------------------------------- |
| contains()    | `//input[contains(@id,'name')]`           |
| starts-with() | `//input[starts-with(@id,'name')]`        |
| text()        | `//label[text()='Name']`                  |
| OR            | `//button[@name='start' or @name='stop']` |

---

# Common Playwright Methods Used

## Count Elements

```ts
const count = await locator.count();
```

## Get Nth Element

```ts
locator.nth(0);
```

## Get Input Value

```ts
await locator.inputValue();
```

## Fill Input

```ts
await locator.fill("Aniket");
```

## Click Element

```ts
await locator.click();
```

---

# Interview Questions

### What is the difference between Absolute XPath and Relative XPath?

**Absolute XPath**

* Starts from root (`html`)
* Long and fragile

**Relative XPath**

* Starts from any node
* Short and maintainable

---

### What is contains() used for?

Used when an attribute contains a partial value.

Example:

```xpath
//input[contains(@class,'form')]
```

---

### What is starts-with() used for?

Used when an attribute begins with a specific value.

Example:

```xpath
//input[starts-with(@class,'form')]
```

---

### What is text() used for?

Used to locate elements using exact visible text.

Example:

```xpath
//label[text()='Colors:']
```

---

### What is the OR operator used for?

Used when an element may have multiple possible attribute values.

Example:

```xpath
//button[@name='start' or @name='stop']
```

XPath Locators --- VVIP Interview Questions + Answers
===================================================

### 1\. What is XPath?

> XPath is a locator language used to identify elements in the DOM based on their tag, attributes, text, or relationships with other elements. In Playwright, we can use XPath with `page.locator()`.

Example:

page.locator("//input[@id='username']");

* * * * *

### 2\. What is the difference between Absolute XPath and Relative XPath?

> **Absolute XPath** starts from the root `html` element and follows the complete DOM hierarchy. It is long and fragile.\
> **Relative XPath** starts from a specific element or node and is generally shorter and more flexible.

Example:

Absolute:

/html/body/div[1]/div[2]/input

Relative:

//input[@id='username']

**Interview point:** Prefer relative XPath over absolute XPath.

* * * * *

### 3\. Why should we avoid Absolute XPath?

> Absolute XPath depends on the complete DOM structure. If a parent element is added, removed, or moved, the XPath can break. Therefore, it is difficult to maintain.

* * * * *

### 4\. What is `contains()` in XPath?

> `contains()` is used when we know only part of an attribute value and the complete value may be different.

Example:

//input[contains(@class,'form')]

If the class is:

form-control

the locator can still find it.

* * * * *

### 5\. What is `starts-with()` in XPath?

> `starts-with()` is used when we know how an attribute value begins, but the remaining part may be dynamic or different.

Example:

//input[starts-with(@id,'dynamic-input-')]

This can match:

dynamic-input-12345

dynamic-input-67890

* * * * *

### 6\. What is `text()` in XPath?

> `text()` is used to locate an element based on its text content.

Example:

//button[text()='Login']

This locates a button whose text is `Login`.

* * * * *

### 7\. What is the `OR` operator in XPath?

> The `or` operator is used when an element can have more than one possible value.

Example:

//button[@name='start' or @name='stop']

This can locate the button whether its name is `start` or `stop`.

* * * * *

### 8\. How do you handle dynamic elements using XPath?

> If an element has a dynamic attribute value, I avoid using the complete dynamic value. Instead, I identify the stable part using functions such as `contains()` or `starts-with()`.

For example, if the ID changes:

dynamic-input-12345

dynamic-input-67890

I can use:

//input[starts-with(@id,'dynamic-input-')]

* * * * *

### 9\. How do you use XPath in Playwright?

> We can use XPath with Playwright's `locator()` method.

Example:

const loginButton = page.locator("//button[text()='Login']");

await loginButton.click();

* * * * *

### 10\. What is the difference between XPath and Playwright's built-in locators?

> Playwright's built-in locators such as `getByRole()`, `getByLabel()`, and `getByText()` are generally preferred because they are more user-focused, readable, and maintainable. XPath is useful when built-in locators are not suitable, especially when we need to use complex DOM relationships or dynamic attributes.

**This is a VVIP question.**

* * * * *

### 11\. Which locator would you prefer: XPath or `getByRole()`?

> If `getByRole()` provides a unique and stable locator, I would prefer it. I would use XPath when the built-in locators cannot uniquely identify the element or when I need more specific DOM-based conditions.

For example:

page.getByRole("button", { name: "Login" });

would generally be preferred over:

page.locator("//button[text()='Login']");

* * * * *

### 12\. Can Playwright handle XPath without explicitly writing `xpath=`?

Yes.

You can write:

page.locator("//button[text()='Login']");

You don't have to write:

page.locator("xpath=//button[text()='Login']");

Playwright can recognize the XPath expression.
