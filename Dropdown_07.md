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
