# Version Bump

**Name:** `Version Bump`

**Workflow File:** `util-bump-version.yml`

**Description:**

This workflow automatically bumps the project version using Commitizen, commits the change, and outputs the new version. It is designed to be triggered via `workflow_call` and supports specifying the branch to bump.

**Created By:** [Your Name or Team]

## Parameters

**Inputs:**

- `default-branch` (optional): Branch to bump (usually `main`)  
  - default: `main`

**Secrets:**

- `repo-token` (required): Token with write permission to push

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: Version Bump

on:
  workflow_dispatch:

jobs:
  bump:
    uses: your-org/.github/.github/workflows/util-bump-version.yml@main
    with:
      default-branch: main
    secrets:
      repo-token: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- Bumps the version on the specified branch using Commitizen and outputs the new version.
