# Bump And Tag

**Name:** `Bump And Tag`

**Workflow File:** `release-bump-version.yml`

**Description:**

This workflow automatically bumps the project version using semantic versioning, updates version references in the package's `__init__.py` file and `CITATION.cff` (if present), commits the changes, creates a Git tag with the new version, and pushes it to the repository. It is designed to be triggered via `workflow_call` and supports customizable default branches.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:**

- `default_branch` (optional): Default branch name  
  - default: `main`
- `working-directory` (optional): The working directory to run the job in
  - default: `.`

**Secrets:**

- `repo-token` (required): GitHub token with permissions to push commits and tags

**Outputs:**

- `new_version`: The new version string after the bump

## Example

**workflow.yml**
```yml
name: Bump And Tag

on:
  push:
    branches:
      - main

jobs:
  bump-and-tag:
    uses: your-org/.github/.github/workflows/release-bump-version.yml@main
    with:
      default_branch: main
    secrets:
      repo-token: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- Calculates the new version (default: patch bump), updates version in the package's `__init__.py` file and `CITATION.cff` (if present), commits the changes, creates a Git tag with the format `v{version}`, and pushes both the commit and tag to the repository.
