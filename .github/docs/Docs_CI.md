# Docs CI (Sphinx)

**Name:** `Docs CI (Sphinx)`

**Workflow File:** `docs-ci.yml`

**Description:**

This workflow builds Sphinx documentation for a Python project using `uv`. It installs project dependencies (including an optional `docs` extra), optionally runs `sphinx-apidoc` to generate API stubs, builds the HTML with `-W` (warnings as errors) by default, optionally runs `sphinx-build -b linkcheck` as a non-blocking check, and uploads the built site as a workflow artifact. Designed to be triggered via `workflow_call`.

> **Scope:** This reusable is for **Sphinx**-based documentation (`docs/source/conf.py`, RST or MyST sources) as used by `aind-library-template` and existing production repos. It is not an MkDocs reusable workflow. The software-practices standard for scientific computing and SIPE specifies MkDocs + mkdocstrings for new documentation. Pick whichever matches your project.

## Parameters

**Inputs:**

- `runner_label` (optional): Runner to use
  - default: `ubuntu-latest`
- `python-version` (optional): Python version to use
  - default: `3.12`
- `docs-extra` (optional): Optional `[project.optional-dependencies]` extra to install (e.g. `docs`). Leave empty to skip.
  - default: `docs`
- `docs-path` (optional): Path to the Sphinx source dir
  - default: `docs`
- `output-dir` (optional): Where to write built HTML
  - default: `docs/_build/html`
- `fail-on-warnings` (optional): Treat Sphinx warnings as errors (`-W`)
  - default: `true`
- `linkcheck` (optional): Run `sphinx-build -b linkcheck` (non-blocking)
  - default: `false`
- `run-apidoc` (optional): Run `sphinx-apidoc` before build
  - default: `false`
- `apidoc-args` (optional): Args passed to `sphinx-apidoc`
  - default: `-o docs/api src`
- `artifact-name` (optional): Name for the uploaded site artifact
  - default: `site`

**Secrets:** N/A

**Outputs:** N/A (uploads a workflow artifact named per `artifact-name`)

## Example

**workflow.yml**
```yml
name: docs (CI)

on:
  pull_request:
    branches: [ main ]
  workflow_dispatch:

jobs:
  docs:
    uses: AllenNeuralDynamics/.github/.github/workflows/docs-ci.yml@main
    with:
      python-version: '3.12'
      docs-extra: 'docs'
      docs-path: 'docs/source'
      output-dir: 'docs/build/html'
      fail-on-warnings: true
      linkcheck: false
      run-apidoc: false
      # apidoc-args: '-o docs/api src/your_package -f'
```

**Results:**

- Sphinx builds the docs tree under `docs-path` into `output-dir`.
- Warnings fail the build by default (opt out via `fail-on-warnings: false`).
- Built HTML is uploaded as an artifact named per `artifact-name`, ready to be consumed by a Pages-deploy workflow or downloaded for review.

**Prerequisites:**

- `pyproject.toml` with Sphinx and any theme/extension deps, preferably under an optional `[project.optional-dependencies] docs = [...]` extra.
- A `uv.lock` is recommended for reproducible installs but not required; the workflow uses `uv sync` (not `--locked`), which will create a lockfile if missing.
