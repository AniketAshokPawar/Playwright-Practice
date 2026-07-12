# 📊 Playwright Reports (HTML Report & Allure Report)

## Why do we need reports?

Reports help us:
- View test execution results.
- Identify passed, failed, skipped, flaky tests.
- Analyze screenshots, videos, traces, logs, and execution time.
- Share execution results with team members.

---

# 1. HTML Report (Built-in Playwright Report)

## What is HTML Report?

- Built-in reporting feature of Playwright.
- No additional installation required.
- Generated after test execution.
- Opens in browser with interactive UI.

---

## Configure HTML Reporter

```ts
reporter: [['html']]
```

or

```ts
reporter: [
    ['html']
]
```

---

## Open HTML Report

If report is already generated:

```bash
npx playwright show-report
```

---

## HTML Report contains

- ✅ Passed Tests
- ❌ Failed Tests
- ⏭️ Skipped Tests
- 🔄 Retried Tests
- 📷 Screenshots (if enabled)
- 🎥 Videos (if enabled)
- 📄 Trace (if enabled)
- Execution time
- Error stack trace

---

## HTML Report Folder

```
playwright-report/
```

---

# HTML Report Auto Open

Configure:

```ts
reporter: [
    ['html', { open: 'always' }]
]
```

Available options:

```ts
open: 'always'
```

- Opens after every execution.

```ts
open: 'on-failure'
```

- Opens only when any test fails.
- Most commonly used.

```ts
open: 'never'
```

- Never opens automatically.
- Open manually using:

```bash
npx playwright show-report
```

---

# 2. Allure Report

## What is Allure?

Allure is an advanced reporting framework that provides more detailed and visually rich reports than the default HTML report.

It supports:

- Test history
- Categories
- Attachments
- Screenshots
- Videos
- Trace files
- Test steps
- Environment information
- Labels
- Severity
- Suites
- Trend analysis

---

# Installation

## Step 1 Install Playwright Reporter

```bash
npm install -D allure-playwright
```

Verify installation:

```bash
npm list allure-playwright
```

---

## Step 2 Install Allure Command Line

```bash
npm install -g allure-commandline
```

Verify installation:

```bash
allure --version
```

Example:

```
2.43.0
```

---

# Configure Reporter

In `playwright.config.ts`

```ts
reporter: [
    ['html'],
    ['allure-playwright']
]
```

---

# Configure Screenshot & Video

```ts
screenshot: 'only-on-failure',
video: 'retain-on-failure'
```

### Screenshot Options

```ts
'off'
```

No screenshots.

```ts
'on'
```

Screenshot for every test.

```ts
'only-on-failure'
```

Screenshot only when test fails.

```ts
'on-first-failure'
```

Screenshot only for the first failed retry.

---

### Video Options

```ts
'off'
```

No video.

```ts
'on'
```

Video for every test.

```ts
'retain-on-failure'
```

Video recorded for every test but saved only for failed tests.

```ts
'on-first-retry'
```

Video recorded only during the first retry.

---

# Run Tests

```bash
npx playwright test
```

or

```bash
npx playwright test tests/Reporters_25.spec.ts --project=Chromium
```

After execution:

```
allure-results/
```

folder is automatically created.

---

# Generate Allure Report

```bash
allure generate allure-results --clean -o allure-report
```

### Explanation

| Option | Meaning |
|----------|----------|
| allure generate | Generate HTML report |
| allure-results | Input folder |
| --clean | Delete previous report |
| -o allure-report | Output folder |

---

# Open Allure Report

```bash
allure open allure-report
```

---

# Folder Structure

```
Project
│
├── allure-results
│
├── allure-report
│
├── playwright-report
│
├── tests
│
└── playwright.config.ts
```

---

# Execution Flow

```
Run Test
      │
      ▼
Playwright executes tests
      │
      ▼
allure-results generated
      │
      ▼
Generate HTML files
      │
      ▼
allure-report generated
      │
      ▼
Open report in browser
```

---

# HTML Report vs Allure Report

| Feature | HTML Report | Allure Report |
|----------|-------------|---------------|
| Built into Playwright | ✅ | ❌ |
| Extra Installation | ❌ | ✅ |
| Easy Setup | ✅ | Moderate |
| Screenshots | ✅ | ✅ |
| Videos | ✅ | ✅ |
| Trace | ✅ | ✅ |
| Retry Information | ✅ | ✅ |
| Test History | ❌ | ✅ |
| Categories | ❌ | ✅ |
| Attachments | Limited | ✅ |
| Environment Info | ❌ | ✅ |
| Trend Analysis | ❌ | ✅ |
| Rich Dashboard | Basic | Excellent |

---

# Interview Questions

## Q1. Difference between HTML Report and Allure Report?

**HTML Report**
- Built into Playwright.
- Easy to configure.
- Good for local execution.

**Allure Report**
- External reporting framework.
- Rich dashboard.
- Supports history, categories, attachments, trends, severity, environment, and advanced reporting.
- Commonly used in enterprise automation frameworks.

---

## Q2. Which report is commonly used in companies?

- HTML Report is commonly used during development.
- Allure Report is preferred in enterprise projects because it provides detailed execution insights and better visualization.

---

# Commands to Remember ⭐

## HTML Report

```bash
npx playwright show-report
```

---

## Allure Report

Install reporter

```bash
npm install -D allure-playwright
```

Install CLI

```bash
npm install -g allure-commandline
```

Verify CLI

```bash
allure --version
```

Generate report

```bash
allure generate allure-results --clean -o allure-report
```

Open report

```bash
allure open allure-report
```

---

# Quick Revision

✅ HTML Report → Built-in Playwright report.

✅ Allure Report → Advanced third-party reporting framework.

✅ `allure-playwright` → Generates `allure-results`.

✅ `allure-commandline` → Converts `allure-results` into HTML report.

✅ HTML report command:

```bash
npx playwright show-report
```

✅ Allure commands:

```bash
allure generate allure-results --clean -o allure-report
```

```bash
allure open allure-report
```
