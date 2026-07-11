# Data-Driven Testing (DDT) in Playwright

## 1. What is Data-Driven Testing (DDT)?

**Data-Driven Testing (DDT)** is a testing approach where test data is separated from the test script.

Instead of writing multiple test cases for different input values, we create a single test script and execute it multiple times using different sets of data.

### Without DDT

```
Test 1 → Search Laptop
Test 2 → Search Camera
Test 3 → Search Smartphone
```

### With DDT

```
             Test Script
                  |
                  |
        ---------------------
        |         |         |
     Laptop    Camera   Smartphone
```

The same test logic runs with different test data.

---

# Advantages of DDT

- ✅ Reduces duplicate test scripts
- ✅ Easy maintenance
- ✅ Improves test coverage
- ✅ Add new test data without changing test code
- ✅ Generates separate test reports for each data set

---

# DDT Approaches in Playwright

In Playwright, DDT can be implemented mainly in two ways:

1. Using data from the same test file
2. Using data from an external JSON file

---

# 1. DDT Using Data from Same File

In this approach, test data is stored inside the Playwright test file.

## Example

```typescript
import { test, expect } from "@playwright/test";


let searchItems = [
    "Laptop",
    "Camera",
    "Smartphone"
];


for(let item of searchItems){

    test(`Search for item: ${item}`, async({page})=>{

        await page.goto("https://demowebshop.tricentis.com/");


        await page
        .locator("#small-searchterms")
        .fill(item);


        await page
        .locator("//input[@type='submit']")
        .click();


        await expect(
            page.locator("h2 a").nth(0)
        )
        .toContainText(item,{ignoreCase:true});


        console.log(
            await page.locator("h2 a").nth(0).textContent()
        );

    });

}
```

---

## Explanation

### Step 1: Create Test Data

```typescript
let searchItems = [
    "Laptop",
    "Camera",
    "Smartphone"
];
```

An array stores multiple test values.

---

### Step 2: Iterate Through Data

```typescript
for(let item of searchItems)
```

The loop picks one value at a time.

Example:

```
Iteration 1 → Laptop

Iteration 2 → Camera

Iteration 3 → Smartphone
```

---

### Step 3: Create Dynamic Tests

```typescript
test(`Search for item: ${item}`)
```

Playwright creates separate tests:

```
✓ Search for item: Laptop

✓ Search for item: Camera

✓ Search for item: Smartphone
```

Each test appears separately in Playwright HTML Report.

---

# 2. DDT Using External JSON File

In real automation projects, test data is usually maintained separately.

Common test data sources:

- JSON
- Excel
- CSV
- Database
- API Response

Here we use JSON.

---

# Project Structure

```
Playwright_Project

│
├── tests
│     |
│     ├── Registration.spec.ts
│     |
│     └── DDT.json
│
```

---

# JSON Test Data File

## DDT.json

```json
[
    {
        "name":"John",
        "email":"john@gmail.com",
        "password":"12345",
        "gender":"Male",
        "country":"India"
    },

    {
        "name":"Smith",
        "email":"smith@gmail.com",
        "password":"56789",
        "gender":"Female",
        "country":"USA"
    }
]
```

---

# Import JSON Data in Playwright

```typescript
import {test,expect} from "@playwright/test";

import DDT from "./DDT.json";
```

Here:

- `DDT` becomes an array of JSON objects.
- Each object represents one test data set.

Example:

```typescript
DDT[0]
```

Output:

```json
{
    "name":"John",
    "email":"john@gmail.com",
    "password":"12345"
}
```

---

# Complete JSON DDT Example

```typescript
import {test,expect} from "@playwright/test";

import DDT from "./DDT.json";


for(let data of DDT){


test(`Registration Test for ${data.name}`, async({page})=>{


    await page.goto(
        "file:///C:/Users/z00575dy/Downloads/preview.html"
    );


    await page.locator("#name")
    .fill(data.name);


    await page.locator("#email")
    .fill(data.email);


    await page.locator("#password")
    .fill(data.password);



    if(data.gender === "Male"){

        await page.locator("#male").check();

    }
    else{

        await page.locator("#female").check();

    }



    await page.locator("#country")
    .selectOption(data.country);



    await page.locator("#agree")
    .check();



    await page.locator("#register")
    .click();



    let expectedMsg =
    `Registration Successful for ${data.name}`;



    await expect(
        page.locator("#message")
    )
    .toHaveText(expectedMsg);



    console.log(
        await page.locator("#message").textContent()
    );


});

}
```

---

# JSON DDT Execution Flow

```
             DDT.json

                 |
                 |
                 ↓

        [
          User 1,
          User 2,
          User 3
        ]

                 |
                 |
                 ↓

          for loop execution

                 |
        ------------------
        |        |        |
      Test1    Test2    Test3

```

---

# Example Execution

## First Iteration

```
data = User 1

Name     → John
Email    → john@gmail.com
Password → 12345
```

Creates:

```
Registration Test for John
```

---

## Second Iteration

```
data = User 2

Name     → Smith
Email    → smith@gmail.com
Password → 56789
```

Creates:

```
Registration Test for Smith
```

---

# Playwright Report

After execution:

```
✓ Registration Test for John

✓ Registration Test for Smith
```

Each JSON object becomes an independent test case.

---

# Difference Between Same File Data and JSON Data

| Same File Data | External JSON Data |
|---|---|
| Data stored inside `.spec.ts` | Data stored separately |
| Suitable for small tests | Suitable for real projects |
| Code and data are mixed | Code and data are separated |
| Less reusable | More reusable |
| Easy initial setup | Easy maintenance |

---

# Interview Answer

**Q: How do you implement Data Driven Testing in Playwright?**

**Answer:**

> In Playwright, I implement Data Driven Testing by separating test data from test logic. I maintain test data in arrays or external JSON files and use loops to execute the same test with different data sets. For every data object, Playwright dynamically creates an independent test case, which improves maintainability and provides better test coverage.

---

# Real-Time Framework Approach

In enterprise automation frameworks, DDT flow looks like:

```
        Test Data
            |
            |
            ↓

    JSON / Excel / API

            |
            |
            ↓

       Test File

            |
            |
            ↓

     Page Object Model

            |
            |
            ↓

    Playwright Execution

            |
            |
            ↓

 HTML / Allure Report
```

---

## Key Points to Remember

- DDT separates **test data** and **test logic**.
- Each data record creates a separate Playwright test.
- JSON is commonly used for maintaining automation test data.
- DDT works well with Page Object Model frameworks.
- It helps achieve scalable and maintainable automation frameworks.
