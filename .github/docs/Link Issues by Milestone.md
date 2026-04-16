# Link Issue to Cross-Repo Milestone Parent

**Name:** `Link issue to cross-repo milestone parent`

**Workflow File:** `util-link-issues-by-milestone.yml`

**Description:**

This workflow links a milestoned issue as a sub-issue of a parent issue in a target repository. It is designed to be triggered via `workflow_call` from a per-repo workflow that listens for the `issues: [milestoned]` event. The parent issue is identified by parsing the milestone's description, which is expected to contain a URL ending in `/issues/<number>`.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:**

- `issue-number` (required): Issue number to link as a sub-issue
- `issue-id` (required): Issue node ID to link as a sub-issue
- `milestone-description` (required): Milestone description containing the parent issue URL
- `target-owner` (optional): Owner of the repository containing the parent issue
  - default: `AllenNeuralDynamics`
- `target-repo` (optional): Repository name containing the parent issue
  - default: `aind-scientific-computing`

**Secrets:**

- `service-token` (required): Token with permission to read and link issues across repositories

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: Link Issue to Milestone Parent

on:
  issues:
    types: [milestoned]

jobs:
  link-issue:
    if: github.event.issue.milestone != null
    uses: AllenNeuralDynamics/.github/.github/workflows/util-link-issues-by-milestone.yml@main
    with:
      issue-number: ${{ github.event.issue.number }}
      issue-id: ${{ github.event.issue.id }}
      milestone-description: ${{ github.event.issue.milestone.description }}
    secrets:
      service-token: ${{ secrets.SERVICE_TOKEN }}
