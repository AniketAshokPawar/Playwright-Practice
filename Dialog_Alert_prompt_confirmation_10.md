# Playwright - JavaScript Dialogs (Alert, Confirmation & Prompt)

## What are JavaScript Dialogs?

JavaScript dialogs are browser pop-ups that interrupt user interaction until they are handled.

Playwright handles dialogs using the `page.on("dialog")` event.

---

# Types of JavaScript Dialogs

1. Simple Alert
2. Confirmation Dialog
3. Prompt Dialog

---

# 1. Simple Alert

## Description

* Displays a message.
* Has only one button (**OK**).
* Can only be accepted.

### Methods Used

* `dialog.message()`
* `dialog.type()`
* `dialog.accept()`

### Example

```ts
page.on("dialog", async(dialog)=>{

    console.log(dialog.message());
    expect(dialog.message()).toBe("I am an alert box!");

    console.log(dialog.type());
    expect(dialog.type()).toBe("alert");

    await dialog.accept();
});

await page.locator("//button[text()='Simple Alert']").click();
```

---

# 2. Confirmation Dialog

## Description

* Displays a confirmation message.
* Has **OK** and **Cancel** buttons.
* Can be accepted or dismissed.

### Methods Used

* `dialog.message()`
* `dialog.type()`
* `dialog.accept()`
* `dialog.dismiss()`

### Accept Example

```ts
page.on("dialog", async(dialog)=>{

    await dialog.accept();

});
```

Expected output:

```text
You pressed OK!
```

---

### Dismiss Example

```ts
page.on("dialog", async(dialog)=>{

    await dialog.dismiss();

});

await page.locator("//button[text()='Confirmation Alert']").click();

let output = await page.locator("#demo").innerText();

expect(output).toBe("You pressed Cancel!");
```

---

# 3. Prompt Dialog

## Description

* Prompts the user to enter text.
* Has **OK** and **Cancel** buttons.
* Supports a default value.

### Methods Used

* `dialog.message()`
* `dialog.type()`
* `dialog.defaultValue()`
* `dialog.accept("text")`
* `dialog.dismiss()`

### Example

```ts
page.on("dialog", async(dialog)=>{

    expect(dialog.type()).toBe("prompt");

    expect(dialog.message()).toBe("Please enter your name:");

    expect(dialog.defaultValue()).toBe("Harry Potter");

    await dialog.accept("Aniket");

});

await page.locator("//button[text()='Prompt Alert']").click();

let output = await page.locator("#demo").innerText();

expect(output).toBe("Hello Aniket! How are you today?");
```

---

# Dialog Event Syntax

```ts
page.on("dialog", async(dialog)=>{

    // Handle dialog here

});
```

---

# Common Dialog Methods

| Method                  | Description                                                             |
| ----------------------- | ----------------------------------------------------------------------- |
| `dialog.message()`      | Returns the dialog message.                                             |
| `dialog.type()`         | Returns the dialog type (`alert`, `confirm`, `prompt`, `beforeunload`). |
| `dialog.defaultValue()` | Returns the default text in a prompt dialog.                            |
| `dialog.accept()`       | Clicks **OK**.                                                          |
| `dialog.accept("text")` | Enters text into a prompt and clicks **OK**.                            |
| `dialog.dismiss()`      | Clicks **Cancel**.                                                      |

---

# Dialog Types

| Dialog Type  | Buttons     | User Input |
| ------------ | ----------- | ---------- |
| Alert        | OK          | ❌ No       |
| Confirmation | OK / Cancel | ❌ No       |
| Prompt       | OK / Cancel | ✅ Yes      |

---

# Best Practices

* Register the dialog handler **before** clicking the button that opens the dialog.
* Verify the dialog message using `dialog.message()`.
* Verify the dialog type using `dialog.type()`.
* Use `dialog.accept("text")` only for prompt dialogs.
* Verify the result displayed on the page after handling the dialog.

---

# Interview Questions

### Q1. How do you handle JavaScript dialogs in Playwright?

Using:

```ts
page.on("dialog", async(dialog)=>{
    await dialog.accept();
});
```

---

### Q2. Difference between `accept()` and `dismiss()`?

* `accept()` → Clicks **OK**.
* `dismiss()` → Clicks **Cancel**.

---

### Q3. How do you enter text into a Prompt dialog?

```ts
await dialog.accept("Aniket");
```

---

### Q4. How do you verify the dialog message?

```ts
expect(dialog.message()).toBe("Expected Message");
```

---

### Q5. How do you verify the dialog type?

```ts
expect(dialog.type()).toBe("confirm");
```

Possible dialog types:

* `alert`
* `confirm`
* `prompt`
* `beforeunload`

---

# Summary

* **Alert** → OK only → `accept()`
* **Confirmation** → OK / Cancel → `accept()` or `dismiss()`
* **Prompt** → Accepts text input → `accept("text")`
* Always register `page.on("dialog")` **before** triggering the dialog.
* Validate dialog message and type before accepting or dismissing.
