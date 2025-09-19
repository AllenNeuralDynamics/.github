# CI (Test with uv)

**Name:** `CI`

**Workflow File:** `test-ci-uv.yml`

**Description:**

This workflow runs continuous integration (CI) checks for a Python project using `uv`. It installs dependencies, runs linting, formatting, typing, docstring coverage, typo checks, unit tests, builds the package, and performs a smoke test on the built wheel. It is designed to be triggered via `workflow_call` and supports configurable OS, Python version, and package name.

**Created By:** [Your Name or Team]

## Parameters

**Inputs:**

- `os` (required): Runner OS (e.g., `ubuntu-latest`, `windows-latest`)
- `python-version` (required): Python version to use
- `package-name` (required): The Python package name to test/cover
- `smoke-test-paths` (optional): Space-separated list of files or directories to copy into the wheel smoke test  
  - default: `tests`
- `run-mypy` (optional): Whether to run mypy for typing checks (`true`/`false`)  
  - default: `false`

**Secrets:**

- `repo-token` (required): GitHub token for checkout/PR operations

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
  test:
    uses: your-org/.github/.github/workflows/test-ci-uv.yml@main
    with:
      os: ubuntu-latest
      python-version: "3.12"
      package-name: my_package
      run-mypy: true
    secrets:
      repo-token: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- Runs linting, formatting, typing, docstring coverage, typo checks, unit tests, builds the package, and performs a smoke test on the built wheel for the specified Python version and OS.
