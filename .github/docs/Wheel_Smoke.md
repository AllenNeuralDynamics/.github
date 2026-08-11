# Wheel Smoke Test

**Name:** `Wheel Smoke Test`

**Workflow File:** `wheel-smoke.yml`

**Description:**

This workflow builds the project wheel with `uv build --wheel`, installs it into an isolated virtual environment, and runs the project's test suite **against the installed wheel** (not against the source tree). It catches the class of packaging bugs — missing subpackages, missing `package_data`, incorrect src-layout configuration, missing type stubs — that source-tree-only tests silently pass. Designed to be triggered via `workflow_call` and paired with `test.yml`.

**Created By:** AIND Scientific Computing

## Parameters

**Inputs:**

- `runner_label` (optional): Runner to use
  - default: `ubuntu-latest`
- `python-version` (optional): Python version to build and smoke-test against
  - default: `3.12`
- `smoke-test-paths` (optional): Space-separated list of files or directories copied into the smoke-test sandbox alongside the built wheel. Typically `tests` so pytest has a suite to run against the installed wheel.
  - default: `tests`

**Secrets:** N/A

**Outputs:** N/A

## Example

**workflow.yml**
```yml
name: Wheel Smoke

on:
  pull_request:
    branches: [ main ]
  workflow_dispatch:

jobs:
  smoke:
    uses: AllenNeuralDynamics/.github/.github/workflows/wheel-smoke.yml@main
    with:
      python-version: '3.12'
      smoke-test-paths: 'tests'
```

**Results:**

- The wheel is built into `dist/`.
- A throwaway sandbox is created under `${{ runner.temp }}/wheeltest/`; `smoke-test-paths` (e.g. `tests`) plus the wheel are copied in.
- A fresh `uv venv` is created inside the sandbox, `pytest` and the wheel are installed, and pytest runs against the **installed** package — so the source tree cannot satisfy imports.
- If the wheel is missing a subpackage, data file, or entry point, pytest imports fail and the job fails.

**Why this is worth running:**

Ordinary `pytest` run against the source tree passes even when the built wheel is broken, because pytest finds code via `src/` / sys.path rather than via the installed distribution. This workflow is the cheap way to catch:

- Missing subpackages (forgot to list them in `[tool.setuptools.packages.find]` or `pyproject.toml [tool.hatch.build.targets.wheel]`).
- Missing `package_data` / resource files.
- Wrong src-layout (`src/foo/` not being picked up by the backend).
- Missing py.typed or .pyi stubs.
- Entry-point typos that only surface post-install.
