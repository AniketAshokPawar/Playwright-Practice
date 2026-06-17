# Playwright - Handling Input Boxes, Radio Buttons and Checkboxes

## Input Box

### Common Validations

#### Verify Visibility

```ts
await expect(inputName).toBeVisible();
```

Checks whether the input box is visible on the page.

---

#### Verify Enabled State

```ts
await expect(inputName).toBeEnabled();
```

Checks whether the input box is enabled for user interaction.

---

#### Verify Attribute Value

```ts
let maxLength = await inputName.getAttribute("maxlength");

expect(maxLength).toBe("15");
```

Checks the value of an HTML attribute.

---

#### Enter Text

```ts
await inputName.fill("Aniket");
```

Enters text into the input field.

---

#### Read Entered Value

```ts
console.log(await inputName.inputValue());
```

Returns the current value present inside the input box.

---

#### Verify Input Value

```ts
await expect(inputName).toHaveValue("Aniket");
```

Checks whether the entered value matches the expected value.

---

# Radio Buttons

## Common Validations

### Verify Visibility

```ts
await expect(radiobtn).toBeVisible();
```

---

### Verify Enabled State

```ts
await expect(radiobtn).toBeEnabled();
```

---

### Verify Initial State

```ts
expect(await radiobtn.isChecked()).toBe(false);
```

Checks whether the radio button is initially unchecked.

---

### Select Radio Button

```ts
await radiobtn.check();
```

Selects the radio button.

---

### Verify Selected State

```ts
await expect(radiobtn).toBeChecked();
```

Verifies that the radio button is selected.

---

# Checkboxes

## Single Checkbox

### Check Checkbox

```ts
await checkbox1.check();
```

---

### Verify Checkbox Checked

```ts
await expect(checkbox1).toBeChecked();
```

---

### Uncheck Checkbox

```ts
await checkbox1.uncheck();
```

---

### Verify Checkbox Unchecked

```ts
await expect(checkbox1).not.toBeChecked();
```

---

# Multiple Checkboxes

## Locate All Checkboxes

```ts
let allCheckboxes =
page.locator(".form-check-input[type='checkbox']");
```

---

## Count Total Checkboxes

```ts
let count = await allCheckboxes.count();

console.log(
    "Total checkboxes are: " + count
);
```

---

## Check All Checkboxes

```ts
for(let i=0;i<count;i++){

    await allCheckboxes.nth(i).check();

    await expect(
        allCheckboxes.nth(i)
    ).toBeChecked();
}
```

### Methods Used

```ts
locator.nth(index)
```

Used to access a specific element from a locator collection.

Examples:

```ts
locator.nth(0); // First element
locator.nth(1); // Second element
locator.nth(2); // Third element
```

---

# Uncheck Last 3 Checkboxes

```ts
for(let i=count-1;i>count-4;i--){

    await allCheckboxes.nth(i).uncheck();

    await expect(
        allCheckboxes.nth(i)
    ).not.toBeChecked();
}
```

### Example

If count = 7

```text
6
5
4
```

Last three checkboxes will be unchecked.

---

# Toggle Checkbox State

## Logic

- Checked → Uncheck
- Unchecked → Check

```ts
for(let i=0;i<count;i++){

    if(await allCheckboxes.nth(i).isChecked()){

        await allCheckboxes.nth(i).uncheck();

    }else{

        await allCheckboxes.nth(i).check();
    }
}
```

---

## isChecked()

Returns:

```ts
true
```

if checkbox is selected.

Returns:

```ts
false
```

if checkbox is not selected.

Example:

```ts
await checkbox.isChecked();
```

---

# Common Assertions

## Visible

```ts
await expect(locator).toBeVisible();
```

---

## Enabled

```ts
await expect(locator).toBeEnabled();
```

---

## Checked

```ts
await expect(locator).toBeChecked();
```

---

## Not Checked

```ts
await expect(locator).not.toBeChecked();
```

---

## Value Verification

```ts
await expect(locator).toHaveValue("Aniket");
```

---

## Attribute Verification

```ts
await expect(locator)
    .toHaveAttribute(
        "maxlength",
        "15"
    );
```

---

# Useful Methods Summary

| Method | Purpose |
|----------|----------|
| fill() | Enter text |
| inputValue() | Get input value |
| getAttribute() | Read attribute value |
| check() | Select radio/checkbox |
| uncheck() | Unselect checkbox |
| isChecked() | Returns checked state |
| count() | Returns total matching elements |
| nth() | Access specific element |
| toBeVisible() | Verify visibility |
| toBeEnabled() | Verify enabled state |
| toBeChecked() | Verify checked state |
| toHaveValue() | Verify input value |

---

# Interview Questions

### Difference between fill() and inputValue()

```text
fill() is used to enter data.

inputValue() is used to read data from an input field.
```

---

### Difference between check() and isChecked()

```text
check() selects a checkbox or radio button.

isChecked() returns whether it is selected or not.
```

---

### How do you count multiple elements?

```ts
await locator.count();
```

---

### How do you access the third checkbox?

```ts
locator.nth(2);
```

Because Playwright uses 0-based indexing.

---

### How do you verify an input field value?

```ts
await expect(locator)
    .toHaveValue("Aniket");
```
