# Playwright - Static Web Table

## What is a Static Web Table?

A **Static Web Table** is a table where:

- Number of columns remains fixed.
- Column order remains fixed.
- Row structure remains fixed.
- Only data inside rows may change.

Example:

| Book Name | Author | Subject | Price |
|-----------|---------|---------|-------|
| Learn Java | Mukesh | Java | 500 |

---

# Locate the Table

```ts
let table = page.locator("table[name='BookTable']");
```

---

# Verify Number of Rows

### Using Assertion

```ts
await expect(table.locator("tr")).toHaveCount(7);
```

### Using count()

```ts
let rowCount = await table.locator("tr").count();
expect(rowCount).toBe(7);
```

---

# Print All Rows

```ts
for(let i = 0; i < rowCount; i++){

    let rowText = await table.locator("tr").nth(i).innerText();
    console.log(rowText);
}
```

---

# Verify Number of Columns

```ts
let columnCount = await table.locator("tr th").count();

await expect(table.locator("tr th")).toHaveCount(4);

expect(columnCount).toBe(4);
```

---

# Verify Specific Row

Example: Verify 3rd row.

```ts
let secondRow = table.locator("tr:nth-child(3)");

await expect(secondRow).toHaveText(
    "Learn Java      Mukesh  Java    500"
);
```

---

# Print Rows Except Header

```ts
for(let i = 1; i < rowCount; i++){

    let rowText = await table.locator("tr").nth(i).innerText();

    console.log(rowText);
}
```

---

# Read Cell Data

Syntax:

```ts
table.locator("tr").nth(rowIndex)
     .locator("td").nth(columnIndex)
```

Example:

```ts
let author = await table
    .locator("tr")
    .nth(2)
    .locator("td")
    .nth(1)
    .innerText();
```

---

# Find Book Name by Author

```ts
for(let i = 1; i < rowCount; i++){

    let author = await table
        .locator("tr")
        .nth(i)
        .locator("td")
        .nth(1)
        .innerText();

    if(author === "Amit"){

        let bookName = await table
            .locator("tr")
            .nth(i)
            .locator("td")
            .nth(0)
            .innerText();

        console.log(bookName);
    }
}
```

---

# Calculate Total Price

```ts
let totalPrice = 0;

for(let i = 1; i < rowCount; i++){

    let price = await table
        .locator("tr")
        .nth(i)
        .locator("td")
        .nth(3)
        .innerText();

    totalPrice += parseInt(price.trim());
}

console.log(totalPrice);
```

---

# Frequently Used Methods

| Method | Purpose |
|---------|---------|
| `count()` | Count rows/columns |
| `nth()` | Select specific row/column (0-based) |
| `innerText()` | Get visible text |
| `locator()` | Chain locators |
| `toHaveCount()` | Verify row/column count |
| `toHaveText()` | Verify row/cell text |

---

# Important Notes

- `nth()` uses **0-based indexing**.
- `nth-child()` uses **1-based indexing**.
- Header row generally contains `<th>`.
- Data rows generally contain `<td>`.
- Start loop from `1` to skip the header row.
- Static tables have fixed column positions, so hardcoding column indexes is acceptable.

---

# Interview Questions

1. Count rows in a table.
2. Count columns in a table.
3. Print all rows.
4. Print all values from a specific column.
5. Find the book written by a specific author.
6. Calculate the total price of all books.
7. Verify a particular row exists.
8. Verify a specific cell value.
9. Print all book names.
10. Find the highest/lowest price in the table.
