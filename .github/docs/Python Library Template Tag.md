# Python Library Template Tag (v0.0.1)

**Name:** `Python Library Template Tag (v0.0.1)`

**Workflow File:** `python_library_template_tag_v0_0_1.yml`

**Description:**

This workflow automatically bumps the project version using semantic versioning, updates version references in the package's `pyproject.toml`, `uv.lock`, and `CITATION.cff` (if present), commits the changes, creates a Git tag with the new version, and pushes it to the repository. It is designed to be triggered via `workflow_call` and supports customizable default branches.

This workflow generates a GitHub App token internally using the provided `app-id` and `app-private-key`, removing the need for a pre-generated `repo-token`. 

It is assumed that the version is managed in the `pyproject.toml` file. The workflow will use `uv version` and `uv lock` to update the version in the `pyproject.toml` and `uv.lock` files.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:**

- `default_branch` (optional): Default branch name
  - default: `main`
- `working-directory` (optional): The working directory to run the job in
  - default: `.`
- `app-id` (required): GitHub App client ID used to generate an installation token

**Secrets:**

- `app-private-key` (required): Private key of the GitHub App used to generate an installation token

**Outputs:**

- `new_version`: The new version string after the bump

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
      default_branch: main
      app-id: ${{ vars.SEMANTIC_RELEASE_BOT_APP_ID }}
    secrets:
      app-private-key: ${{ secrets.SEMANTIC_RELEASE_BOT_PRIVATE_KEY }}
```

**Results:**

- Generates a GitHub App installation token, checks out the repository, calculates the new version (default: patch bump), updates the version in `pyproject.toml`, `uv.lock`, and `CITATION.cff` (if present), commits the changes, creates a Git tag with the format `v{version}`, and pushes both the commit and tag to the repository.
