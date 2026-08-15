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

### XPath Axes — VVIP Interview Questions + Answers
## 1. What are XPath Axes?

XPath Axes are used to navigate between related elements in the DOM. They help us locate an element based on its relationship with another element, such as its parent, child, ancestor, or descendant.

Example:

page.locator("//td[text()='Javascript']/parent::tr");

Here we find Javascript and move to its parent <tr>.

## 2. What is the difference between parent and ancestor?

parent selects only the immediate parent of an element, while ancestor can select the parent, grandparent, or any element above it.

Example:

//td[text()='Javascript']/parent::tr

gets the immediate <tr>.

//td[text()='Javascript']/ancestor::table

can move further up and get the <table>.

This is a very important question.

## 3. What is the difference between child and descendant?

child selects only the direct children, while descendant selects children at any level, including grandchildren and deeper elements.

Example:

//tr/child::td

gets direct <td> elements.

//table/descendant::td

gets <td> elements anywhere inside the table.

## 4. What is the self axis?

The self axis selects the current element itself.

Example:

//td[text()='Java']/self::td

It stays on the same <td> element.

Interview note: Know it, but don't overemphasize it. In real Playwright work, it's less commonly needed.

## 5. Why would you use XPath Axes in automation?

I use XPath axes when the target element does not have a reliable unique locator, but a related element does. I can locate the known element first and then navigate to the required element using its relationship.

For example:

<label>Username</label>
<input>

If the input doesn't have a useful ID, I can use the relationship between the label and input.

This is the real-world reason for using axes.

## 6. Give a real-world example of using parent axis.

Suppose a table contains:

<tr>
    <td>John</td>
    <td>Admin</td>
    <td>Active</td>
</tr>

If Admin is unique but the row doesn't have a unique attribute:

//td[text()='Admin']/parent::tr

This finds the complete row.

This is useful when working with tables where we identify a row using the text of one cell.

## 7. Give a real-world example of using ancestor.

Suppose:

```
<table>
    <tr>
        <td>John</td>
    </tr>
</table>
```
If John is easy to identify but the table isn't:

//td[text()='John']/ancestor::table

This moves from the cell upward and finds the table.

I would use ancestor when I know something inside a container but need to locate the container itself.

## 8. Can you combine XPath Axes?

Yes.

For example:

//td[text()='Javascript']/parent::tr/td[4]

This means:

Find Javascript → move to its parent row → select the fourth cell.

This is a very practical interview coding question.

## 9. How do you use XPath Axes in Playwright?

I use them inside page.locator().

Example:

const row = page.locator(
    "//td[text()='Javascript']/parent::tr"
);


await expect(row).toHaveText(
    "Learn JS Javascript Amit 500"
);
## 10. What is the difference between XPath Axes and normal XPath?

Normal XPath can locate an element using its own attributes or text. XPath Axes allow us to locate an element based on its relationship with another element.

For example:

Normal XPath:

//button[@id='login']

Using an axis:

//label[text()='Username']/following-sibling::input

The second one finds the input based on its relationship with the label.

## 11. Which XPath axes are most useful in real automation?

For your interview, I'd say:

The axes I commonly use or would use are parent, ancestor, child, descendant, and following-sibling. Among these, parent, ancestor, and following-sibling are especially useful when the target element itself doesn't have a good unique locator.

XPath technically has 13 axes, including following-sibling, preceding-sibling, following, and preceding, but you don't need to memorize all of them for your current preparation.

## 12. Is XPath Axes better than Playwright's built-in locators?
```
Not necessarily. I would first try Playwright's built-in locators such as getByRole(), getByLabel(), or getByTestId(). If those cannot uniquely identify the element, I may use XPath or XPath axes based on the DOM relationship.

Playwright specifically recommends prioritizing user-facing locators over XPath where possible because XPath tied to DOM structure can be less resilient.
```
