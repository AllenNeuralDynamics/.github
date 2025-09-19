# Update Badges

**Name:** `Update Badges`

**Workflow File:** `util-update-badges.yml`

**Description:**

This workflow updates status badges (Python version, docstring coverage, and test coverage) in the `README.md` file based on the latest project state. It is designed to be triggered via `workflow_call` and supports configurable default branch and Python version.

**Created By:** Yosef Bedaso

## Parameters

**Inputs:**

- `default-branch` (optional): Default branch name  
  - default: `main`
- `python-version` (optional): Python version to use for badge updates  
  - default: `3.10`

**Secrets:**

- `SERVICE_TOKEN` (required): Token with write permission to push changes

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: Update Badges

on:
  workflow_dispatch:

jobs:
  update-badges:
    uses: your-org/.github/.github/workflows/util-update-badges.yml@main
    with:
      default-branch: main
      python-version: "3.10"
    secrets:
      SERVICE_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- Updates the Python version, docstring coverage, and test coverage badges in `README.md` and commits the changes to the specified branch.
