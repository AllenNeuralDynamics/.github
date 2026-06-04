# Commitizen Release and Publish

**Name:** `Bump/tag/publish with commitizen`

**Workflow File:** `commitizen-release-publish.yml`

**Description:**

This workflow uses [commitizen](https://commitizen-tools.github.io/commitizen/) to automatically bump the project version, generate a changelog, create a Git tag, and publish a GitHub Release. It is designed to be triggered via `workflow_call` and skips execution if the triggering commit is already a commitizen bump commit (i.e. the commit message starts with `bump:`).
Configuration details are set by the repository's `pyproject.toml`, `.cz.toml`, or other commitizen config file.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:** N/A

**Secrets:**

- `GITHUB_TOKEN` (required): Automatically provided by GitHub. Used to push the bump commit, create a tag, and publish the release.

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    uses: AllenNeuralDynamics/.github/.github/workflows/commitizen-release-publish.yml@main
    secrets: inherit
```

**Results:**

- Analyzes commits since the last tag to determine the version bump, updates `pyproject.toml` (or whichever file commitizen is configured to manage), generates an incremental changelog, commits the bump, creates a Git tag with the format `v{version}`, and publishes a GitHub Release with the changelog as the release body.
