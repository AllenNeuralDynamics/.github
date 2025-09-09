# Lint (Ruff)

**Name:** ``Lint (Ruff)``

**Workflow File:** ``test-lint-ruff-python.yml``

**Description:** 

This workflow runs ``ruff check`` on the project with ``UV``.

**Created By:** Jessy Liao (SIPE)

## Parameters

**Inputs:** 

- ``runner-label`` (optional): runner to use 
  - default: to "eng-tools"
- ``ruff-commands`` (optional): commands to run with ruff
  - default: ``ruff check``

**Outputs:** N/A

## Examples

### Example 1: Ruff Check Default

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

### Example 2: Ruff Check Commands

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
    with: 
      ruff-commands: check --fix
```

**Results:**

- runs ``ruff check --fix`` on the project whenever a PR is created to merge into dev or main


