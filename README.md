# Github Actions

This repository contains all the reusable workflows and workflow templates for the AIND Organization. Make sure to read the [standards](#reusable-workflow-standards) below before adding any new reusable workflows or templates. 

## Reusable Workflow Standards

### Naming Convention

The structure for naming reusable workflows should follow the following format:

```
[category]-[function]-[tooling scope]-[language or platform]
```

Examples:

- ``build-pkg-uv-python``: Build a whl for a python package using UV.
- ``release-engtools-python``: Releases a distributable (from previous build step) to eng-tools
- ``test-lint-ruff-python``: Lints python project using ruff

```
.github
├── .github/workflows <===================== REUSABLE WORKFLOWS HERE
│   ├── test-lint-ruff-python.yml
│   ├── build-pkg-uv-python.yml
│   ├── release-engtools-python.yml
│   ├── ...
├── workflow-templates <==================== WORKFLOW TEMPLATES HERE
│   ├── build-pkg-uv-python.yml
│   ├── release-engtools-python.yml
├── actions <=============================== COMPOSITE ACTIONS HERE
└── README.md
```

## Workflow Templates Standards

Workflow templates should encompass an entire CI workflow. It should contain all the necessary reusable workflows to accomplish a CI workflow for a specific type of project. 

In the ``properties.json`` file, ensure that the description clearly describes each step in the CI workflow and what kind of project this template was designed to integrate into.

### Naming Convention

The structure for naming workflow templates should follow the following format:

```
[Team] [Project Type] Workflow
```

Example: 

- SIPE Python Desktop Application CI
- SIPE Python Package CI
- AIND Python Library CI
