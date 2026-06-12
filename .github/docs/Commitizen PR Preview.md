# Commitizen PR Preview

**Name:** `version bump preview`

**Workflow File:** `commitizen-pr-preview.yml`

**Description:**

This workflow runs `cz bump --dry-run` against the PR head and posts a comment on the pull request showing the expected version bump and CHANGELOG section that will be produced when the PR is merged. It is designed to be triggered via `workflow_call` from a per-repo workflow listening for `pull_request` events. The comment is only posted on non-draft PRs opened from the same repository (not forks).

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:** N/A

**Secrets:**

- `GITHUB_TOKEN` (required): Automatically provided by GitHub. Used to post the version bump preview comment on the pull request.

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: PR Version Preview

on:
  pull_request:
    branches:
      - main
    types:
      - opened
      - synchronize
      - reopened
      - ready_for_review

jobs:
  preview:
    uses: AllenNeuralDynamics/.github/.github/workflows/commitizen-pr-preview.yml@main
    secrets: inherit
```

**Results:**

- Runs `cz bump --dry-run` on the PR head and posts a comment on the pull request showing the expected version bump. If no commits are eligible for a bump, the comment states so. If the dry-run fails, the comment includes the error output with an exit code warning.
