# Lint (Ruff)

**Name:** ``Lint (Ruff)``

**Workflow File:** ``test-lint-ruff-python.yml``

**Description:** 

This workflow runs ``ruff check`` on the project with ``UV``.

**Created By:** Jessy Liao (SIPE)

## Parameters

**Inputs:** 

- ``runner-label`` (optional): runner to use - default: to "eng-tools"

**Outputs:** N/A

## Examples

### Example 1: Ruff Check

**workflow.yml**
```yml
name: Lint

on:
  pull_request:
    branches:
      - main
      - dev 

jobs: 
  lint: 
    uses: AllenNeuralDynamics/.github/.github/workflows/test-lint-ruff-python.yml@main
```

**Results:**

- runs ``ruff check`` on the project whenever a PR is created to merge into dev or main

