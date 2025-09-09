# Tag Auto Bump (UV)

**Name:** ``Tag Auto Bump (UV)``

**Workflow File:** ``release-tag-uv-python.yml``

**Description:**

This workflow automatically bumps the version of a python project. It performs the following:

- Creates a new tag with the new version (adds 'v' prefix, e.g. v1.0.0)
- Updates the ``pyproject.toml`` file with the new tag
- Handles ``dev`` pre-release if action trigger from "dev" branch

**Created By:** Jessy Liao (SIPE)

## Parameters

**Inputs:** 

- ``runner-label`` (optional): runner to use 
  - defaults: "ubuntu-latest" 

**Outputs:** 

- ``version``: new version (``*.*.*.<(optional pre-release)>``)

## Examples

### Example 1: Auto Main and Dev Branches

**workflow.yml**
```yml
name: Auto Tag Workflow
 
on:
  push:
    branches:
      - main
      - dev

jobs: 
  tag: 
    uses: AllenNeuralDynamics/.github/.github/workflows/release-tag-uv-python.yml@dev
```

**Results:**

- Auto tags version whenever changes are pushed to main or dev

### Example 2: Auto Main - Manual Dev

**workflow.yml**
```yml
name: Auto Tag Workflow

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs: 
  tag: 
    uses: AllenNeuralDynamics/.github/.github/workflows/release-tag-uv-python.yml@dev
```

**Results:**

- Auto tags version whenever changes are pushed to main 
- Can trigger automatic tagging manually 

### Example 3: Tag + Publish

**workflow.yml**
```yml
name: Tag and Publish

on:
  push:
    branches:
      - main
      - dev

jobs: 
  tag: 
    uses: AllenNeuralDynamics/.github/.github/workflows/release-tag-uv-python.yml@dev
  publish: 
    needs: tag
    uses: AllenNeuralDynamics/.github/.github/workflows/release-exe-uv_pyinstaller-python.yml@dev
    with: 
      project-version: ${{ needs.auto-tag.outputs.version }} 
      tag-name: v${{ needs.auto-tag.outputs.version }}
```

**Results:**

- Auto tags, build, and publish whenever a push is made to main or dev. 
- Because publish is running in the same workflow as tag, the version must be passed into publish (if it isn't the publish workflow will reference the previous version).  
