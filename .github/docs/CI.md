# CI

**Name:** `CI`

**Workflow File:** `test-ci.yml`

**Description:**

This workflow runs continuous integration (CI) tasks for a Python project. It installs dependencies, runs linting with flake8, checks docstring coverage with interrogate, and executes unit tests with coverage reporting. Designed to be triggered via `workflow_call` with a configurable Python version.

**Created By:** [Your Name or Team]

## Parameters

**Inputs:**

- `python-version` (required): Python version to use in the CI

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: CI

on:
  pull_request:
    branches:
      - main
      - dev

jobs:
  ci:
    uses: your-org/.github/.github/workflows/test-ci.yml@main
    with:
      python-version: "3.11"
```

**Results:**

- Installs dependencies, runs flake8 for linting, checks docstring coverage with interrogate, and runs unit tests with coverage reporting for the specified Python version.
