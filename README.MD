# 🚀 GitHub Actions — Concepts & Notes

A simple and clear guide to understand the **core concepts of GitHub Actions** — **Workflows**, **Jobs**, and **Steps**.

---

## 🧩 Overview

GitHub Actions automates tasks like CI/CD, testing, deployments, and code scanning.

Workflows are defined as YAML files under:
.github/workflows/*.yml

Each workflow runs based on specific triggers (push, PR, schedule, or manually via workflow dispatch).

---

## ⚙️ 1. Workflow

A **workflow** is an automated process defined in a YAML file.  
It defines **when** to run and **what** to execute.

### 🔹 Key points:
- Each workflow can contain multiple jobs.
- Triggered by events (push, pull_request, schedule, etc.).
- Can have environment variables, permissions, concurrency, and defaults.

### 🧾 Example:
```yaml
name: CI Workflow

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 2 * * *'  # Runs daily at 2 AM UTC
```

##  🧱 2. Job

A job is a group of steps that run on a single runner (machine).
Jobs in a workflow run in parallel by default, but you can control order using needs.

### 🔹 Key options:

- **runs-on**: defines the OS/environment (like ubuntu-latest).

- **needs**: defines dependencies between jobs.

- **strategy.matrix**: run same job with different parameters.

- **if**: conditionally run the job.

- **outputs**: share data with other jobs.


### 🧾 Example:
``` yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build project
        run: make build
```

### 🧩 Job Dependencies:
``` yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Linting complete"

  test:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - run: echo "Testing after lint"
```

Here, test runs after lint.