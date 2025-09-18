# Publish Python Package

**Name:** `Publish Python Package`

**Workflow File:** `release-publish-python.yml`

**Description:**

This workflow builds a Python package, creates a GitHub release from a tag, and optionally publishes the package to PyPI. It uses `uv` for building and publishing, and supports configurable Python versions.

**Created By:** [Your Name or Team]

## Parameters

**Inputs:**

- `python-version` (optional): Python version for build  
  - default: `3.13`
- `tag-name` (required): Git tag to release
- `publish-to-pypi` (optional): Whether to publish to PyPI (`true`/`false`)  
  - default: `false`

**Secrets:**

- `repo-token` (required): GitHub token for release creation
- `pypi-token` (optional): PyPI API token

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: Publish Python

on:
  push:
    tags:
      - "v*.*.*"

jobs:
  publish:
    uses: your-org/.github/.github/workflows/release-publish-python.yml@main
    with:
      tag-name: ${{ github.ref_name }}
      publish-to-pypi: true
    secrets:
      repo-token: ${{ secrets.GITHUB_TOKEN }}
      pypi-token: ${{ secrets.PYPI_TOKEN }}
```

**Results:**

- Builds the Python package, creates a GitHub release for the pushed tag, and publishes the package to PyPI if a token is provided.
