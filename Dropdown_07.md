# Playwright - Handling Dropdowns

Playwright provides the `selectOption()` method to interact with HTML `<select>` dropdowns.

---

# 1. Single Select Dropdown

A single select dropdown allows selecting only one option at a time.

### Syntax

```ts
await locator.selectOption("Canada");
```

or

```ts
await locator.selectOption({ value: "canada" });
```

or

```ts
await locator.selectOption({ label: "Canada" });
```

or

```ts
await locator.selectOption({ index: 2 });
```

---

## Example

```ts
let dropdown = page.locator("#country");

await dropdown.selectOption("Canada");

await expect(dropdown).toHaveValue("canada");
```

---

# 2. Selecting Options

### Select by Label (Visible Text)

```ts
await dropdown.selectOption("Canada");
```

or

```ts
await dropdown.selectOption({
    label: "Canada"
});
```

---

### Select by Value

```ts
await dropdown.selectOption({
    value: "france"
});
```

---

### Select by Index

```ts
await dropdown.selectOption({
    index: 3
});
```

---

# 3. Verify Selected Option

```ts
await expect(dropdown).toHaveValue("canada");
```

Read selected value

```ts
console.log(await dropdown.inputValue());
```

---

# 4. Count Dropdown Options

```ts
let options = dropdown.locator("option");

let count = await options.count();

console.log(count);
```

---

# 5. Print All Dropdown Options

```ts
for(let i = 0; i < count; i++){

    let text = await options.nth(i).textContent();

    console.log(text?.trim());
}
```

---

# 6. Verify Option Exists

```ts
let found = false;

for(let i = 0; i < count; i++){

    let text = await options.nth(i).textContent();

    if(text?.trim() === "India"){

        found = true;
        break;
    }
}

if(found){
    console.log("India is present");
}
else{
    console.log("India is not present");
}
```

---

# 7. Multi Select Dropdown

A multi-select dropdown allows selecting multiple options simultaneously.

### Syntax

```ts
await dropdown.selectOption([
    "red",
    "blue",
    "green"
]);
```

---

## Example

```ts
let dropdown = page.locator("#colors");

await dropdown.selectOption([
    "red",
    "blue",
    "green"
]);
```

---

# 8. Count Total Options

```ts
let count = await dropdown
    .locator("option")
    .count();

expect(count).toBe(7);
```

---

# 9. Print All Options

```ts
for(let i = 0; i < count; i++){

    let text = await dropdown
        .locator("option")
        .nth(i)
        .textContent();

    console.log(text?.trim());
}
```

---

# 10. Verify Option Exists in Multi Select Dropdown

```ts
let found = false;

for(let i = 0; i < count; i++){

    let text = await dropdown
        .locator("option")
        .nth(i)
        .textContent();

    if(text?.trim() === "Yellow"){

        found = true;
        break;
    }
}

if(found){
    console.log("Yellow is present");
}
else{
    console.log("Yellow is not present");
}
```

---

# 11. Count Selected Options

```ts
let selectedOptions = dropdown.locator("option:checked");

let selectedCount = await selectedOptions.count();

console.log(selectedCount);
```

---

# 12. Print Selected Options

```ts
let selectedOptions = dropdown.locator("option:checked");

let count = await selectedOptions.count();

for(let i = 0; i < count; i++){

    console.log(
        await selectedOptions
            .nth(i)
            .textContent()
    );
}
```

---

# 13. Check Duplicate Options

```ts
let options = dropdown.locator("option");

let count = await options.count();

let texts: string[] = [];

for(let i = 0; i < count; i++){

    let text = await options.nth(i).textContent();

    texts.push(text!.trim());
}

let uniqueOptions = new Set(texts);

if(uniqueOptions.size === texts.length){
    console.log("No duplicate options");
}
else{
    console.log("Duplicate options found");
}
```

---

# Frequently Used Methods

| Method | Purpose |
|---------|----------|
| `selectOption()` | Select dropdown option(s) |
| `toHaveValue()` | Verify selected value (single select) |
| `inputValue()` | Read selected value |
| `locator("option")` | Locate all options |
| `count()` | Count options |
| `textContent()` | Read option text |
| `option:checked` | Locate selected option(s) |
| `Set` | Detect duplicate options |

---

# Interview Tips

- Use `selectOption()` for HTML `<select>` dropdowns only.
- Select options by **label**, **value**, or **index**.
- Use `toHaveValue()` to verify the selected value in a single-select dropdown.
- Use `option:checked` to work with selected options in a multi-select dropdown.
- Use `locator("option")` to count or print all dropdown options.
- Use a `Set` to check for duplicate dropdown values.

VVIP Dropdown Interview Questions + Answers
-------------------------------------------

### 1\. How do you handle a dropdown in Playwright?

For a native HTML `<select>` dropdown, I use `selectOption()`.

await page.locator("#country").selectOption({ label: "India" });

It allows selection by **label, value, or index**.

* * * * *

### 2\. How can you select an option from a dropdown?

We can select it in three common ways:

// By label

await dropdown.selectOption({ label: "India" });

// By value

await dropdown.selectOption({ value: "india" });

// By index

await dropdown.selectOption({ index: 2 });

* * * * *

### 3\. What is the difference between selecting by label and value?

Consider:

<option value="ind">India</option>

Here:

-   `India` → visible **label**
-   `ind` → HTML **value**

So:

await dropdown.selectOption({ label: "India" });

selects using visible text, while:

await dropdown.selectOption({ value: "ind" });

selects using the `value` attribute.

* * * * *

### 4\. Can Playwright handle multi-select dropdowns?

**Yes.**

If the `<select>` supports multiple selections, we can pass multiple values:

await dropdown.selectOption([

    "red",

    "green",

    "blue"

]);

* * * * *

### 5\. How do you verify that an option is selected?

For a single-select dropdown:

await expect(dropdown).toHaveValue("india");

For a multi-select dropdown:

await expect(dropdown).toHaveValues([

    "red",

    "green"

]);

* * * * *

### 6\. How do you count the options in a dropdown?

I locate the `option` elements and use `count()`:

let options = dropdown.locator("option");

let count = await options.count();

Important: `dropdown.count()` counts the dropdown itself, **not its options**.

* * * * *

### 7\. How do you get all options from a dropdown?

I locate all `option` elements and iterate through them:

let options = dropdown.locator("option");

let count = await options.count();

for(let i = 0; i < count; i++) {

    console.log(await options.nth(i).innerText());

}

* * * * *

### 8\. How do you select the third option from a dropdown?

await dropdown.selectOption({ index: 2 });

Playwright uses **0-based indexing**, so index `2` represents the third option.

* * * * *

⭐ Very Important Interview Questions
------------------------------------

### 9\. What is the difference between a native dropdown and a custom dropdown?

A **native dropdown** uses the HTML `<select>` element:

<select>

    <option>India</option>

</select>

For this, I use:

selectOption()

A **custom dropdown** may use elements like `div`, `button`, and `li`. For that, I use normal Playwright locators and click actions.

For example:

await page.getByRole("button", { name: "Select Country" }).click();

await page.getByText("India").click();

* * * * *

### 10\. Can we use `selectOption()` for a custom dropdown?

**No.**

`selectOption()` is intended for native HTML `<select>` elements.

For a custom dropdown, I interact with the dropdown using normal locators, for example:

await dropdown.click();

await page.getByText("India").click();

* * * * *

### 11\. Which method would you prefer: selecting by index or value?

I prefer **value or label** over index because index depends on the order of options.

For example:

await dropdown.selectOption({ value: "india" });

is generally more stable than:

await dropdown.selectOption({ index: 2 });

If the developer changes the option order, an index-based locator can select the wrong option.

* * * * *

### 12\. How do you verify whether a particular option exists?

I would locate all `option` elements, iterate through them, and compare their text.

let options = dropdown.locator("option");

let count = await options.count();

let found = false;

for(let i = 0; i < count; i++) {

    let text = (await options.nth(i).innerText()).trim();

    if(text === "India") {

        found = true;

        break;

    }

}

expect(found).toBe(true);

* * * * *

### 13\. How do you get the selected value from a dropdown?

For a native dropdown:

let value = await dropdown.inputValue();

For example, if India is selected and its value is `india`, it returns:

india

* * * * *

### 14\. How do you verify multiple selected options?

For a multi-select dropdown:

await expect(dropdown).toHaveValues([

    "red",

    "green",

    "blue"

]);

This verifies the selected option values.

* * * * *

### 15\. How would you handle a dropdown whose options are dynamically loaded?

First, I would interact with the dropdown and then locate the required option after it becomes available.

For example, for a custom dropdown:

await dropdown.click();

await page.getByText("India").click();

If necessary, I can also use Playwright's built-in waiting through locators rather than adding unnecessary hard waits.

* * * * *

🔥 One scenario-based question you should definitely prepare
------------------------------------------------------------

### 16\. Interviewer: "Suppose there are 10 options in a dropdown and tomorrow the developer changes their order. How would you make your test stable?"

**Answer:**

> I would avoid selecting the option by index because the order can change. I would preferably select it using a stable value or label.

Example:

await dropdown.selectOption({ value: "india" });
