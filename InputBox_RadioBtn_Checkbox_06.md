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

Input Boxes, Radio Buttons & Checkboxes --- VVIP Interview Questions
==================================================================

### 1\. How do you enter text into an input field in Playwright?

> I use the `fill()` method. It clears the existing value and enters the specified text.

await page.getByLabel("Username").fill("Aniket");

Playwright recommends `fill()` as the straightforward way to fill text inputs.

* * * * *

### 2\. What is the difference between `fill()` and `inputValue()`?

> `fill()` is used to enter a value into an input field, while `inputValue()` is used to read the current value from the input field.

await username.fill("Aniket");

console.log(await username.inputValue());

* * * * *

### 3\. How do you verify the value entered in an input field?

> I use the `toHaveValue()` assertion.

await expect(username).toHaveValue("Aniket");

This is preferable when I want to make an assertion about the value rather than simply retrieve it.

* * * * *

### 4\. How do you verify whether an input field is enabled or disabled?

> I use `toBeEnabled()` or `toBeDisabled()`.

await expect(username).toBeEnabled();

or:

await expect(username).toBeDisabled();

* * * * *

### 5\. How do you select a radio button in Playwright?

> I use the `check()` method.

await page.getByLabel("Male").check();

Playwright's documentation specifically supports `check()` for both radio buttons and checkboxes.

* * * * *

### 6\. How do you verify that a radio button is selected?

> I use the `toBeChecked()` assertion.

await expect(page.getByLabel("Male")).toBeChecked();

* * * * *

### 7\. What is the difference between `check()` and `isChecked()`?

**Very VVIP.**

> `check()` is an action that makes the checkbox or radio button checked. `isChecked()` reads the current state and returns `true` or `false`.

Example:

await radio.check();

const status = await radio.isChecked();

console.log(status);

`check()` ensures the desired checked state, while `isChecked()` returns the current state.

* * * * *

### 8\. What is the difference between `isChecked()` and `toBeChecked()`?

**Extremely VVIP.**

> `isChecked()` returns a boolean, so I use it when I need to make a decision in my code. `toBeChecked()` is an assertion that verifies the expected checked state.

Example:

if (await checkbox.isChecked()) {

    // do something

}

versus:

await expect(checkbox).toBeChecked();

Playwright recommends the assertion when the purpose is to verify the checked state.

* * * * *

### 9\. What is the difference between `check()` and `click()` for a checkbox?

**VVIP.**

> `check()` is specifically intended to make a checkbox or radio button checked and verifies that the element reaches the checked state. `click()` is a general mouse-click action.

So if my requirement is:

> "Select this checkbox."

I would use:

await checkbox.check();

rather than:

await checkbox.click();

Playwright's `check()` also returns immediately if the checkbox is already checked.

* * * * *

### 10\. How do you uncheck a checkbox?

> I use `uncheck()`.

await checkbox.uncheck();

Then I can verify:

await expect(checkbox).not.toBeChecked();

* * * * *

### 11\. How do you handle multiple checkboxes?

> I first create a locator that identifies all the checkboxes, get their count, and then use `nth()` inside a loop.

Example:

const checkboxes = page.locator("input[type='checkbox']");

const count = await checkboxes.count();

for (let i = 0; i < count; i++) {

    await checkboxes.nth(i).check();

}

* * * * *

### 12\. How would you make sure all checkboxes are checked?

There are two ways depending on the requirement.

If I simply want all of them checked:

for (let i = 0; i < count; i++) {

    await checkboxes.nth(i).check();

}

I don't need to call `isChecked()` first because `check()` already ensures the checked state.

If I need different logic based on the current state, then I can use:

if (await checkbox.isChecked()) {

    // already checked

} else {

    await checkbox.check();

}

**This is a good interview distinction.**

* * * * *

### 13\. How do you select a specific checkbox from multiple checkboxes?

> I can use `nth()` because Playwright uses zero-based indexing.

For the third checkbox:

await checkboxes.nth(2).check();

So:

nth(0) = first

nth(1) = second

nth(2) = third

* * * * *

### 14\. How would you select a checkbox using its label?

**VVIP Playwright question.**

If the checkbox has an associated label:

await page.getByLabel("Subscribe").check();

This is generally preferable to creating a CSS/XPath locator directly against the checkbox because it represents how the user identifies the control. Playwright documents `getByLabel()` and `getByRole()` as suitable ways to locate form controls.

* * * * *

### 15\. What would you do if the checkbox is already checked?

> If my requirement is simply to make sure it is checked, I can directly use `check()`. Playwright will leave it checked if it is already in the desired state.

await checkbox.check();

I don't necessarily need:

if (!(await checkbox.isChecked())) {

    await checkbox.check();

}

This is an important practical point.

* * * * *

### 16\. How would you toggle a checkbox?

> If I want to reverse its current state, I can use `isChecked()` and then call `check()` or `uncheck()` accordingly.

if (await checkbox.isChecked()) {

    await checkbox.uncheck();

} else {

    await checkbox.check();

}

This is different from simply wanting the checkbox to be checked.

* * * * *

### 17\. How do you verify that a checkbox is not selected?

await expect(checkbox).not.toBeChecked();

> I use the `not` assertion because I want to verify the opposite state.

* * * * *

### 18\. Can `check()` be used for both checkboxes and radio buttons?

> Yes. Playwright's `check()` method can be used for both checkbox and radio input elements.

await checkbox.check();

await radioButton.check();

* * * * *

### 19\. What locator would you prefer for a checkbox in Playwright?

> If the checkbox has a meaningful label, I would prefer `getByLabel()`. I can also use `getByRole('checkbox', { name: ... })`. If those aren't suitable, I would use a stable CSS or other locator.

Example:

await page.getByRole("checkbox", {

    name: "Subscribe"

}).check();

Playwright recommends using the accessible role together with the accessible name when locating elements by role.

* * * * *

### 20\. Suppose an interviewer asks: "How would you automate a form containing text fields, radio buttons and checkboxes?"

A good practical answer:

> First, I would locate each control using a stable Playwright locator. I would use `fill()` for text fields, `check()` for radio buttons and checkboxes, and then use Playwright assertions such as `toHaveValue()` and `toBeChecked()` to verify the results.

Example:

await page.getByLabel("Name").fill("Aniket");

await page.getByLabel("Male").check();

await page.getByLabel("Subscribe").check();

await expect(page.getByLabel("Name"))

    .toHaveValue("Aniket");

await expect(page.getByLabel("Male"))

    .toBeChecked();

await expect(page.getByLabel("Subscribe"))

    .toBeChecked();
