# Github Actions

This repository contains all the reusable workflows and workflow templates for the AIND Organization. Make sure to read the [standards](#reusable-workflow-standards) below before adding any new reusable workflows or templates. 

## Project Structure

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

## Reusable Workflow Standards

Majority of these standards are based off of this [article](https://wellarchitected.github.com/library/collaboration/recommendations/scaling-actions-reusability/?utm_source=chatgpt.com#3-define-contribution-guidelines-and-best-practices-for-adding-new-workflows-and-actions)

### Naming Convention

The structure for naming reusable workflows should follow the following format:

```
[category]-[function]-[tooling scope]-[language or platform]
```

**Categories:** 

| Prefix   | Purpose                          |
| -------- | -------------------------------- |
| sec-     | Security scanning and compliance |
| build-   | Build processes                  |
| test-    | Testing workflows                |
| deploy-  | Deployment processes             |
| release- | Release processes                |
| util     | Utility actions                  |

### Examples:

- ``build-pkg-uv-python``: Build a whl for a python package using UV.
- ``release-engtools-python``: Releases a distributable (from previous build step) to eng-tools
- ``test-lint-ruff-python``: Lints python project using ruff


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

## Contribution Guide

1. Create a new branch: 

    - ``workflow/<workflow_name>`` for new reusable workflow
    - ``workflow_tempalte/<workflow_template_name>`` for new workflow template name


2. Add reusable workflow or workflow template. Make sure to adhere to the naming conventions. 

3. Create documentation for the new workflow/template. Use copier to use pre-existing template: 

    ```
    uvx copier copy copier_template .
    ```

4. Fill out the rest of the information in the markdown file and create a pull request
