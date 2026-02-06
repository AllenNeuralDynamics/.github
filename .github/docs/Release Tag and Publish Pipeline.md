# Tag and Publish Pipeline


**Name:** `Tag and Publish Pipelines`

**Workflow File:** `tag-and-publish-pipelines.yml`

**Description:**

This workflow uses [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) to determine how to automatically bump the semantic version of a pipeline repository

| Commit Prefix | Version Bump | Example |
|---------------|--------------|---------|
| `fix:` | Patch | `0.1.0` → `0.1.1` |
| `feat:` | Minor | `0.1.0` → `0.2.0` |
| `feat!:` or `BREAKING CHANGE` | Major | `0.1.0` → `1.0.0` |
| `chore:`, `docs:`, `style:` | No bump | — |

After the version has been bumped, the workflow updates `pipeline/nextflow.config` with the version in `PIPELINE_VERSION`, the pipeline name in `PIPELINE_NAME` and the GitHub URL in `PIPELINE_URL`. These environment variables can be queried wherever the `processing.json` is created (add them to keys `PIPELINE_NAME`, `PIPELINE_VERSION` and `PIPELINE_URL`).

## Parameters

**Trigger Events:**

- `push` to `main` branch: Automatically triggers version bump based on conventional commits
- `workflow_dispatch`: Manual trigger with optional bump type override

**Inputs (workflow_dispatch only):**

| Input | Required | Type | Default | Description |
|-------|----------|------|---------|-------------|
| `bump_type` | No | choice | `''` (auto-detect) | Version bump type. Options: `patch`, `minor`, `major`, or empty for auto-detection from commits |

**Secrets:**

| Secret | Required | Description |
|--------|----------|-------------|
| `GITHUB_TOKEN` | Yes | Automatically provided by GitHub. Used to push commits, create tags, and publish releases |

**Outputs:**

This workflow does not produce reusable outputs. Instead, it performs the following actions:

- Updates `pipeline/nextflow.config` with `PIPELINE_VERSION` and `PIPELINE_URL`
- Commits the config changes with `[skip ci]` flag
- Creates and pushes a Git tag (`v{version}`)
- Creates a GitHub Release with auto-generated release notes

## Example

This workflow runs automatically when you push to `main`. Simply copy `tag-and-publish-pipelines.yml` to your repository's `.github/workflows/` directory.

**Automatic trigger (push to main):**

When you merge a PR or push directly to `main`, the workflow:

1. Analyzes commit messages to determine the version bump type
2. Updates `pipeline/nextflow.config` with the new version
3. Commits, tags, and creates a GitHub Release

```bash
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    uses: aind/.github/.github/workflows/release-tag-and-publish-pipeline.yml@main
```

**Results:**

- Calculates the new version based on conventional commits (or manual selection)
- Updates `pipeline/nextflow.config` with `PIPELINE_NAME`, `PIPELINE_VERSION` and `PIPELINE_URL`
- Commits the changes with message `ci: bump version to {version} [skip ci]`
- Creates and pushes a Git tag with format `v{version}`
- Creates a GitHub Release with auto-generated release notes