# Playwright XPath Axes

## What are XPath Axes?

XPath Axes are used to navigate between related elements in the DOM (Document Object Model).

They help locate elements based on relationships such as:

* Self
* Parent
* Child
* Ancestor
* Descendant

---

# 1. Self Axis

## Definition

The `self` axis selects the current node itself.

## Syntax

```xpath
//tagname/self::tagname
```

## Example

```xpath
//td[text()='Java']/self::td
```

## Playwright Example

```ts
let selfLoc = page.locator(
    "//td[text()='Java']/self::td"
);

console.log(await selfLoc.textContent());

await expect(selfLoc).toHaveText("Java");
```

## Output

```text
Java
```

---

# 2. Parent Axis

## Definition

The `parent` axis selects the immediate parent of the current node.

## Syntax

```xpath
//childElement/parent::parentElement
```

## Example

```xpath
(//td[text()='Javascript'])[1]/parent::tr
```

## Playwright Example

```ts
let parentLoc = page.locator(
    "(//td[text()='Javascript'])[1]/parent::tr"
);

console.log(await parentLoc.textContent());
```

## DOM Structure

```html
<tr>
    <td>Learn JS</td>
    <td>Javascript</td>
    <td>500</td>
</tr>
```

The locator selects the entire `<tr>` row.

---

# 3. Child Axis

## Definition

The `child` axis selects all direct children of an element.

## Syntax

```xpath
//parentElement/child::childElement
```

## Example

```xpath
//table[@name='BookTable']//tr[3]/child::td
```

## Playwright Example

```ts
let childLoc = page.locator(
    "//table[@name='BookTable']//tr[3]/child::td"
);

console.log(await childLoc.allTextContents());

await expect(childLoc).toHaveCount(4);
```

## Output Example

```text
[
  "Learn JS",
  "Javascript",
  "Amit",
  "500"
]
```

---

# 4. Ancestor Axis

## Definition

The `ancestor` axis selects all ancestors of an element.

Ancestors include:

* Parent
* Grandparent
* Great-grandparent

## Syntax

```xpath
//childElement/ancestor::ancestorElement
```

## Example

```xpath
(//td[text()='Selenium'])[1]/ancestor::table
```

## Playwright Example

```ts
let ancestorLoc = page.locator(
    "(//td[text()='Selenium'])[1]/ancestor::table"
);

console.log(await ancestorLoc.textContent());

await expect(ancestorLoc).toHaveAttribute(
    "name",
    "BookTable"
);
```

## Use Case

Locate a table using a value present inside one of its cells.

---

# 5. Descendant Axis

## Definition

The `descendant` axis selects all descendants of an element.

Descendants include:

* Children
* Grandchildren
* Great-grandchildren

## Syntax

```xpath
//parentElement/descendant::childElement
```

## Example

```xpath
//table[@name='BookTable']/descendant::td
```

## Playwright Example

```ts
let descendantLoc = page.locator(
    "//table[@name='BookTable']/descendant::td"
);

console.log(await descendantLoc.allTextContents());

await expect(descendantLoc).toHaveCount(24);
```

## Use Case

Retrieve all table cells from a table.

---

# textContent() vs allTextContents()

## textContent()

Used when the locator returns a single element.

### Example

```ts
let selfLoc = page.locator(
    "//td[text()='Java']/self::td"
);

console.log(await selfLoc.textContent());
```

### Output

```text
Java
```

---

## allTextContents()

Used when the locator returns multiple elements.

### Example

```ts
let childLoc = page.locator(
    "//table[@name='BookTable']//tr[3]/child::td"
);

console.log(await childLoc.allTextContents());
```

### Output

```text
[
  "Learn JS",
  "Javascript",
  "Amit",
  "500"
]
```

---

# Common Assertions Used

## Verify Text

```ts
await expect(locator).toHaveText("Java");
```

## Verify Count

```ts
await expect(locator).toHaveCount(4);
```

## Verify Attribute

```ts
await expect(locator).toHaveAttribute(
    "name",
    "BookTable"
);
```

---

# XPath Axes Summary

| Axis       | Description                              |
| ---------- | ---------------------------------------- |
| self       | Selects current node                     |
| parent     | Selects immediate parent                 |
| child      | Selects direct children                  |
| ancestor   | Selects parent, grandparent, etc.        |
| descendant | Selects all children and nested children |

---

# Interview Questions

### What is the Self Axis?

Selects the current node itself.

Example:

```xpath
//td[text()='Java']/self::td
```

---

### What is the Parent Axis?

Selects the immediate parent element.

Example:

```xpath
//td[text()='Javascript']/parent::tr
```

---

### What is the Child Axis?

Selects direct children of an element.

Example:

```xpath
//tr[3]/child::td
```

---

### What is the Ancestor Axis?

Selects parent, grandparent, and other upper-level elements.

Example:

```xpath
//td[text()='Selenium']/ancestor::table
```

---

### What is the Descendant Axis?

Selects all nested child elements.

Example:

```xpath
//table/descendant::td
```
