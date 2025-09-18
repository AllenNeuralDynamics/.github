# Tag

**Name:** `Tag`

**Workflow File:** `release-tag.yml`

**Description:**

This workflow automatically bumps the project version, updates version references in source and metadata files, commits the changes, and creates a new Git tag. It is designed to be triggered via `workflow_call` and supports customizable default branches.

**Created By:** [Your Name or Team]

## Parameters

**Inputs:**

- `default_branch` (optional): Default branch name  
  - default: `main`

**Secrets:**

- `SERVICE_TOKEN` (required): GitHub token with permissions to push commits and tags

**Outputs:**

- `new_version`: The new version string after the bump

## Example

**workflow.yml**
```yml
name: Tag Release

on:
  workflow_dispatch:

jobs:
  tag:
    uses: your-org/.github/.github/workflows/release-tag.yml@main
    with:
      default_branch: main
    secrets:
      SERVICE_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- Bumps the version (default: patch), updates version in source and `CITATION.cff` (if present), commits the changes, and pushes a new Git tag to the repository.
