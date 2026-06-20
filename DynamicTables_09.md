# Playwright - Dynamic Web Table

## What is a Dynamic Web Table?

A **Dynamic Web Table** is a table where:

* Row values change dynamically.
* Column positions may also change after page refresh.
* New rows/columns may be added or removed.
* Hardcoding column indexes is **not recommended**.

Example:

Refresh 1:

| Browser | CPU (%) | Memory |
| ------- | ------- | ------ |
| Chrome  | 3.7     | 450    |

Refresh 2:

| Memory | Browser | CPU (%) |
| ------ | ------- | ------- |
| 450    | Chrome  | 3.7     |

Notice that **CPU (%)** is no longer the second column.

---

# Why Hardcoding Fails

❌ Bad Practice

```ts
table.locator("td").nth(1);
```

This assumes **CPU (%)** is always the second column.

If the column order changes, the script will fail.

---

# Recommended Approach

1. Locate the table.
2. Count rows and columns.
3. Find the required column dynamically using the header text.
4. Find the required row.
5. Read the required cell value.
6. Compare or validate the value.

---

# Locate the Table

```ts
let table = page.locator("table#taskTable");
```

---

# Count Rows

```ts
let rowCount = await table.locator("tr").count();
```

---

# Count Columns

```ts
let columnCount = await table.locator("tr th").count();
```

---

# Find the "CPU (%)" Column Dynamically

```ts
for(let i = 0; i < columnCount; i++){

    let headerText = await table
        .locator("tr")
        .first()
        .locator("th")
        .nth(i)
        .innerText();

    if(headerText === "CPU (%)"){

        // CPU column found

        break;
    }
}
```

---

# Find Chrome Row

```ts
for(let j = 1; j < rowCount; j++){

    let browser = await table
        .locator("tr")
        .nth(j)
        .locator("td")
        .nth(0)
        .innerText();

    if(browser === "Chrome"){

        // Chrome row found

        break;
    }
}
```

---

# Read CPU Value

```ts
let cpuLoad = await table
    .locator("tr")
    .nth(j)
    .locator("td")
    .nth(i)
    .innerText();
```

---

# Compare With Another UI Value

```ts
let outerCpu = await page
    .locator(".chrome-cpu:nth-child(1)")
    .innerText();

expect(outerCpu).toBe(cpuLoad);
```

---

# Complete Flow

```text
Locate Table
      │
      ▼
Count Rows & Columns
      │
      ▼
Find CPU (%) Header
      │
      ▼
Store CPU Column Index
      │
      ▼
Find Chrome Row
      │
      ▼
Read CPU Value
      │
      ▼
Compare with Displayed CPU Value
```

---

# Frequently Used Methods

| Method            | Purpose                    |
| ----------------- | -------------------------- |
| `count()`         | Count rows or columns      |
| `locator()`       | Create chained locators    |
| `nth()`           | Access specific row/column |
| `innerText()`     | Read visible text          |
| `expect().toBe()` | Compare values             |

---

# Interview Tips

* Never hardcode column indexes in a dynamic table.
* First identify the required column using the header text.
* Then locate the required row.
* Finally, read the cell value using the dynamic column index.
* Use nested loops when both rows and columns are dynamic.

---

# Frequently Asked Interview Questions

1. Find the CPU (%) value for Chrome.
2. Find the Memory value for Firefox.
3. Verify the CPU value shown outside the table matches the table value.
4. Find a row based on Browser name.
5. Find the highest CPU usage.
6. Print all Browser names.
7. Print values of a specific dynamic column.
8. Handle a table where column order changes after every refresh.

---

# Difference: Static vs Dynamic Table

| Static Table                            | Dynamic Table                                 |
| --------------------------------------- | --------------------------------------------- |
| Fixed column order                      | Column order may change                       |
| Hardcoded column indexes are acceptable | Column indexes should be found dynamically    |
| Easier to automate                      | Requires loops and dynamic logic              |
| Simpler interview questions             | More common in real-world interview scenarios |
