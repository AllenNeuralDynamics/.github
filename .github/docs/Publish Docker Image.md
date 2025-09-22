# Publish Docker Image

**Name:** `Publish Docker Image`

**Workflow File:** `release-publish-docker-image.yml`

**Description:**

This workflow builds and publishes a Docker image to GitHub Container Registry (GHCR) whenever triggered via `workflow_call`. It supports custom Docker tags, branch-based tagging, and configurable Docker build contexts.

**Created By:** Yosef Bedaso

## Parameters

**Inputs:**

- `default-branch` (optional): Default branch name  
  - default: `main`
- `working-directory` (optional): Working directory for the CI tasks
  - default: `.`
- `docker-tag` (required): Docker image tag to use
- `branch-tag` (optional): Branch tag to use for Docker image  
  - default: `latest`

**Secrets:**

- `repo-token` (required): GitHub token for release creation

**Outputs:** N/A

## Examples

### Example 1: Publish with Default Settings

**workflow.yml**
```yml
name: Publish Docker

on:
  push:
    branches:
      - main

jobs:
  publish-docker:
    uses: your-org/.github/.github/workflows/release-publish-docker-image.yml@main
    with:
      docker-tag: v1.0.0
    secrets:
      repo-token: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- Builds and pushes the Docker image with tag `v1.0.0` and branch tag `latest` to GHCR when pushing to `main`.

---

### Example 2: Custom Docker Context and Branch Tag

**workflow.yml**
```yml
name: Publish Docker

on:
  push:
    branches:
      - dev

jobs:
  publish-docker:
    uses: your-org/.github/.github/workflows/release-publish-docker-image.yml@main
    with:
      docker-tag: dev-2024-09-18
      working-directory: docker
      branch-tag: dev
    secrets:
      repo-token: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- Builds and pushes the Docker image from the `./docker` directory with tag `dev-2024-09-18` and branch tag `dev` to GHCR when pushing to `dev`.
