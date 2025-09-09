# Build and Publish DEVPI (UV)

**Name:** ``Build and Publish DEVPI (UV)``

**Workflow File:** ``release-pkg-devpi-python``

**Description:** 

This workflow builds a python package and uses uv to upload it to devpi (SIPE's internal python package repository).

**Created By:** Jessy Liao (SIPE)

## Parameters

**Inputs:**

- ``runner-label`` (optional): runner to use 
  - default: to "eng-tools"

**Outputs:** N/A

## Examples

### Example 1: Publish on new tag

**workflow.yml**
```yml
name: Publish DEVPI

on:
  push:
    branches:
      - main

jobs: 
  publish: 
    uses: AllenNeuralDynamics/.github/.github/workflows/release-pkg-devpi-python.yml@main
    with: 
      tag-name: ${{ github.ref }}
```

**Results:**

- Automatically build python package and uploads it to devpi whenever changes are pushed to main