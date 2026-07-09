# Python Library Template Publish (v0.0.1)

**Name:** `Python Library Template Publish (v0.0.1)`

**Workflow File:** `python_library_template_publish_v0_0_1.yml`

**Description:**

This workflow builds a Python package using `uv build`, validates the distribution with `twine check`, and publishes it to PyPI using [Trusted Publishing](https://docs.pypi.org/trusted-publishers/) (OIDC). No PyPI token or password is required. It is designed to be triggered via `workflow_call` and supports configurable branch, Python version, and working directory.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:**

- `default-branch` (optional): Default branch name, used when pulling latest changes before building
  - default: `main`
- `python-version` (optional): Python version to use for building and publishing
  - default: `3.12`
- `working-directory` (optional): Working directory for the build and publish tasks
  - default: `.`

**Secrets:** N/A — publishing uses Trusted Publishing (OIDC); no PyPI token is required

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
  tag:
    uses: your-org/.github/.github/workflows/python_library_template_tag_v0_0_1.yml@main
    with:
      app-id: ${{ vars.SEMANTIC_RELEASE_BOT_APP_ID }}
    secrets:
      app-private-key: ${{ secrets.SEMANTIC_RELEASE_BOT_PRIVATE_KEY }}

  publish:
    needs: tag
    uses: your-org/.github/.github/workflows/python_library_template_publish_v0_0_1.yml@main
    with:
      default-branch: main
      python-version: "3.12"
```

**Results:**

- Pulls the latest changes (including the version bump commit from the tag job), builds the package with `uv build`, validates the distribution artifacts with `twine check`, and publishes to PyPI using Trusted Publishing.

## Prerequisites

Trusted Publishing must be configured on PyPI for the repository before this workflow can publish. See the [PyPI Trusted Publishers documentation](https://docs.pypi.org/trusted-publishers/adding-a-publisher/) for setup instructions.
