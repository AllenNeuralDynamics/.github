# Python Library Template Update Badges (v0.0.1)

**Name:** `Python Library Template Update Badges (v0.0.1)`

**Workflow File:** `python_library_template_update_badges_v0_0_1.yml`

**Description:**

This workflow updates status badges (Python version, docstring coverage, and test coverage) in the `README.md` file based on the latest project state. It is designed to be triggered via `workflow_call` and supports configurable default branch and Python version.

This workflow generates a GitHub App token internally using the provided `app-id` and `app-private-key`, removing the need for a pre-generated `repo-token`. It uses `uv` for dependency management and test execution instead of `pip`.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:**

- `default-branch` (optional): Default branch name
  - default: `main`
- `python-version` (optional): Python version to use for badge updates
  - default: `3.12`
- `working-directory` (optional): Working directory for the CI tasks
  - default: `.`
- `app-id` (required): GitHub App client ID used to generate an installation token

**Secrets:**

- `app-private-key` (required): Private key of the GitHub App used to generate an installation token

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: Tag and Publish

on:
  push:
    branches:
      - main

jobs:
  update-badges:
    uses: your-org/.github/.github/workflows/python_library_template_update_badges_v0_0_1.yml@main
    with:
      default-branch: main
      python-version: "3.12"
      app-id: ${{ vars.SEMANTIC_RELEASE_BOT_APP_ID }}
    secrets:
      app-private-key: ${{ secrets.SEMANTIC_RELEASE_BOT_PRIVATE_KEY }}
```

**Results:**

- Generates a GitHub App installation token, checks out the repository, runs `interrogate` and `pytest` via `uv`, then updates the Python version, docstring coverage, and test coverage badges in `README.md` and commits the changes to the specified branch.
