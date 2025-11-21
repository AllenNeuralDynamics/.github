# Tag

**Name:** `Tag`

**Workflow File:** `release-tag.yml`

**Description:**

This workflow extracts the current version from a Python package's `__init__.py` file (as defined in `pyproject.toml`), creates a Git tag with that version, and pushes it to the repository. It is designed to be triggered via `workflow_call` and supports customizable default branches.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:**

- `default_branch` (optional): Default branch name  
  - default: `main`
- `working-directory` (optional): The working directory to run the job in
  - default: `.`

**Secrets:**

- `repo-token` (required): GitHub token with permissions to push tags

**Outputs:**

- `new_version`: The new version extracted from the package's `__init__.py` file

## Example

**workflow.yml**
```yml
name: Tag Release

on:
  push:
    branches:
      - main

jobs:
  tag:
    uses: your-org/.github/.github/workflows/release-tag.yml@main
    with:
      default_branch: main
    secrets:
      repo-token: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- Extracts the version from the package's `__init__.py` file, creates a Git tag with the format `v{version}`, and pushes it to the repository.
