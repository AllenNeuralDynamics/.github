# Test (PyTest)

**Name:** ``Test (PyTest)``

**Workflow File:** ``test-pytest-python.yml``

**Description:** 

This workflow runs ``pytest tests/`` on the project with ``UV``.

**Created By:** Jessy Liao (SIPE)

## Parameters

**Inputs:**

- ``runner-label`` (optional): runner to use 
  - default: to "eng-tools"
- ``pytest-commands`` (optional): pytest commands to run
  - default: ``pytest tests/``

**Outputs:** N/A

## Examples

### Example 1: Test

**workflow.yml**
```yml
name: Test

on:
  pull_request:
    branches:
      - main
      - dev 

jobs: 
  test: 
    uses: AllenNeuralDynamics/.github/.github/workflows/test-pytest-python.yml@main
```

**Results:**

- runs ``pytest tests/`` on the project whenever a PR is created to merge into dev or main

### Example 1: Test Custom

**workflow.yml**
```yml
name: Test

on:
  pull_request:
    branches:
      - main
      - dev 

jobs: 
  test: 
    uses: AllenNeuralDynamics/.github/.github/workflows/test-pytest-python.yml@main
    with:
      pytest-commands: tests/test_unit_1.py
```

**Results:**

- runs ``pytest tests/test_unit_1.py`` on the project whenever a PR is created to merge into dev or main

