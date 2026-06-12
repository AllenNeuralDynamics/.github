# <Workflow Title>

**Name:** `<Workflow Display Name>`

**Workflow File:** `<workflow-file-name>.yml`

**Description:**

<Description of what this workflow does, when it runs, and what it produces.>

**Created By:** <Author or Team Name>

## Parameters

**Inputs:**

- `<input-name>` (required|optional): <Description>
  - default: `<default-value>`

**Secrets:**

- `<secret-name>` (required|optional): <Description>

**Outputs:**

- `<output-name>`: <Description>

## Example

**workflow.yml**
```yml
name: <Workflow Name>

on:
  push:
    branches:
      - main

jobs:
  <job-name>:
    uses: your-org/.github/.github/workflows/<workflow-file-name>.yml@main
    with:
      <input-name>: <value>
    secrets:
      repo-token: ${{ secrets.GITHUB_TOKEN }}
```

**Results:**

- <Description of what the workflow does when triggered.>
