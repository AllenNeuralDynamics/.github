# Run CodeOcean Integration

**Name:** `Run CodeOcean Integration`

**Workflow File:** `run-co-integration.yml`

**Description:**

This workflow submits a batch of runs to a Code Ocean capsule, where each dataset is submitted to a separate run.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:**

| Input | Required | Type | Default | Description |
|-------|----------|------|---------|-------------|
| `test_commit` | Yes | string | None | The commit hash for this repo to run the test code against |
| `co_capsule_id` | Yes | string | None | The ID of the CodeOcean capsule to run |
| `co_data_asset_ids` | No | list | [ ] |  A list of CodeOcean data asset IDs that will be submitted with the run. Each data asset is sent to its own run |
| `cancel_on_failure` | No | bool | True | Cancels all jobs in the batch when one run fails |
| `log_file` | No | string | "output" | File where the CodeOcean capsule logs errors. The tail of this file will be printed in the workflow output for debugging failures

**Secrets:**

| Input | Required | Type | Default | Description |
|-------|----------|------|---------|-------------|
| `co_token` | Yes | string | None | CodeOcean API token to submit jobs to the capsule |

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: Run CodeOcean Integration

on:
  push:
    branches:
      - main

jobs:
  code-ocean:
    uses: AllenNeuralDynamics/.github/.github/workflows/run-co-integration.yml@v1
    with:
      test_commit: ${{ github.event.pull_request.head.sha }}
      co_capsule_id: 64c28ee3-68cc-4033-beb6-94317455e2ac
      co_data_asset_ids: '["75671bb6-2c58-41f9-a53f-8de40cd285df", "1fa84619-ad5c-46b4-919a-a11e3f283450"]'
      cancel_on_failure: true
      log_file: error.log
    secrets:
      co_token: ${{ secrets.CODEOCEAN_TOKEN }}
```

**Results:**

- One run for each data asset id submitted to the CodeOcean capsule
- After the runs complete, a summary is printed with details on which runs passed/failed
- Errors include last lines logged to the output file
