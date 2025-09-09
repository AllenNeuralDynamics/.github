# Type (mypy)

**Name:** ``Type (mypy)``

**Workflow File:** ``test-type-mypy-python.yml``

**Description:** 

This workflow runs ``mypy`` on the project with ``UV``.

**Created By:** Jessy Liao (SIPE)

## Parameters

**Inputs:**

- ``runner-label`` (optional): runner to use - default: to "eng-tools"

**Outputs:** N/A

## Examples

### Example 1: Type Check

**workflow.yml**
```yml
name: Type Check

on:
  pull_request:
    branches:
      - main
      - dev 

jobs: 
  type: 
    uses: AllenNeuralDynamics/.github/.github/workflows/test-type-mypy-python.yml@main
```

**Results:**

- runs ``mypy`` on the project whenever a PR is created to merge into dev or main

