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

🔥 Playwright Dialogs --- VVIP Interview Questions
------------------------------------------------

### 1\. ⭐⭐⭐ How do you handle JavaScript dialogs in Playwright?

**Answer:**

> We handle JavaScript dialogs using the `page.on('dialog')` event. Inside the handler, we can accept or dismiss the dialog.

page.on("dialog", async dialog => {

    await dialog.accept();

});

await page.locator("#alertBtn").click();

* * * * *

### 2\. ⭐⭐⭐ What types of JavaScript dialogs can Playwright handle?

**Answer:**

> Playwright can handle `alert`, `confirm`, `prompt`, and `beforeunload` dialogs.

* * * * *

### 3\. ⭐⭐⭐ What is the difference between `accept()` and `dismiss()`?

**Answer:**

> `accept()` is equivalent to clicking **OK**, while `dismiss()` is equivalent to clicking **Cancel**.

await dialog.accept();   // OK

await dialog.dismiss();  // Cancel

* * * * *

### 4\. ⭐⭐⭐ How do you enter text into a Prompt dialog?

**Answer:**

> We pass the text to `dialog.accept()`.

await dialog.accept("Aniket");

This enters `Aniket` into the prompt and clicks OK.

* * * * *

### 5\. ⭐⭐⭐ How do you verify the message of a dialog?

expect(dialog.message()).toBe("I am an alert box!");

**Interview answer:**

> We use `dialog.message()` to get the text displayed in the dialog.

* * * * *

### 6\. ⭐⭐⭐ How do you identify whether a dialog is alert, confirm or prompt?

Use:

dialog.type()

Example:

expect(dialog.type()).toBe("confirm");

Possible types include:

alert

confirm

prompt

beforeunload

* * * * *

### 7\. 🔥🔥 Why should `page.on("dialog")` be written before clicking the button?

**Answer:**

> Because the click triggers the dialog. So I register the handler first, so Playwright is ready to handle the dialog when it appears.

page.on("dialog", async dialog => {

    await dialog.accept();

});

await page.locator("#alertBtn").click();

This **handler-before-action pattern** is one of the most important things to remember.

* * * * *

### 8\. 🔥🔥 What happens if you register `page.on("dialog")` but don't accept or dismiss the dialog?

**Answer:**

> The dialog remains open and blocks the page, so the action that triggered it can hang.

For example, this is wrong:

page.on("dialog", async dialog => {

    console.log(dialog.message());

});

await page.locator("#alertBtn").click();

The handler must eventually call:

await dialog.accept();

or:

await dialog.dismiss();

Playwright's documentation specifically warns about this behavior.

* * * * *

### 9\. ⭐⭐⭐ Does Playwright automatically handle dialogs?

**Answer:**

> Yes. If we don't register a dialog handler, Playwright automatically dismisses dialogs by default. If we register a `page.on("dialog")` listener, then our handler must accept or dismiss the dialog.

This is a **very good interview question** because many candidates don't know this.

* * * * *

### 10\. ⭐⭐ How do you handle a confirmation dialog and click Cancel?

page.on("dialog", async dialog => {

    await dialog.dismiss();

});

await page.locator("#confirmBtn").click();

**Answer:**

> For Cancel, I use `dialog.dismiss()`.

* * * * *

### 11\. ⭐⭐ How do you get the default value of a Prompt?

dialog.defaultValue()

**Answer:**

> `defaultValue()` returns the default text displayed in a prompt dialog.

* * * * *

### 12\. 🔥🔥 Interview scenario

**Interviewer:**

> "The application displays a confirmation dialog when clicking Delete. You need to verify its message, click Cancel, and verify that the record was not deleted. How will you automate it?"

Your answer should describe:

Register dialog handler

        ↓

Verify dialog message/type

        ↓

dismiss()

        ↓

Click Delete

        ↓

Verify record still exists

This type of **real-world scenario question** is more valuable than simply memorizing `accept()`/`dismiss()`.

* * * * *

### 13\. 🔥🔥 What is the difference between a JavaScript dialog and a normal HTML popup/modal?

This is worth preparing because interviewers may ask it as a follow-up.

**Simple answer:**

> A JavaScript dialog is generated by browser functions such as `alert()`, `confirm()` and `prompt()`, and Playwright handles it through the `dialog` event. An HTML modal is part of the webpage DOM, so we normally locate and interact with it like any other web element.
