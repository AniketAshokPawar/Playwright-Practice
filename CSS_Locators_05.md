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
