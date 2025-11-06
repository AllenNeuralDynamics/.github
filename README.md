# Github Actions

This repository contains reusable templates and components to help build a CI/CD pipeline. These components can be used in repositories throughout the **AllenInstituteNeuralDynamics** organization. Included in this repositories are: 

- **Workflow Templates**: Templates of Github Actions workflows which run an entire CI/CD pipeline. 
- **Reusable Workflows**: Modular workflows designed to be reused across other projects. Think of these as *jobs* that can be injected to an existing github action workflow.
- **Composite Actions**: Custom Github actions that can be reused. Think of these as steps that can be injected into an existing job in an existing github action workflow. 
- **(🚧) Issue Templates**: WIP
- **(🚧) Pull-Request Templates**: WIP

## Project Structure

```
.github
├── .github
│   ├── workflows <========================= REUSABLE WORKFLOWS HERE
|   │   ├── test-lint-ruff-python.yml
|   │   ├── test-lint-ruff-python.yml
|   │   ├── build-pkg-uv-python.yml
|   │   ├── release-engtools-python.yml
|   │   ├── ...
|   ├── docs <============================== DOCS HERE
│   ├── actions <=========================== COMPOSITE ACTIONS HERE
|   │   ├── extract_info
|   │   |   ├── action.yml
├── workflow-templates <==================== WORKFLOW TEMPLATES HERE
│   ├── build-pkg-uv-python.yml
│   ├── release-engtools-python.yml
|   ├── ...
└── README.md
```

## Documentation

Documentation for each reusable workflow and composite action can be found in the [wiki](https://github.com/AllenNeuralDynamics/.github/wiki). 

The documentation should outline what the workflow/action does and what parameters you can write/read to it.

# Usage

Here are some helpful topics to go over before integrating the various CI/CD components in this repository. 

- [Building a CI/CD Pipeline](https://github.blog/enterprise-software/ci-cd/build-ci-cd-pipeline-github-actions-four-steps/)
- [Github Actions](https://docs.github.com/en/actions/get-started/quickstart#using-workflow-templates)
- [Github Workflows](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows)

## Workflow Templates

Below are the steps to use a workflow template. 

1. Click ``Actions`` on the top nav bar for your repository. If ``Actions`` is missing you might not have the correct permissions.
2. On the left hand side click the ``New workflow`` button. 
3. From here you should see a list of workflow templates in this repository. Click configure on the ones you want to add to your repository. 

## Reusable Workflows

To use a reusable workflow, simply add the following code snippet to your existing workflow. 

```yml
name: Your Custom Workflow

on:
  pull_request:
    branches:
      - main

jobs:
  call-reusable-workflow:
    uses: AllenNeuralDynamics/.github/.github/workflows/<workflow_name>@<branch> 
```

This will add a step in your CI/CD pipeline that runs ``<workflow_name>`` which is a corresponding reusable workflow found in this repository. 

The ``@<branch>`` part is optional. This can be added to choose a reusable workflow that can be found in a specific branch of this repository.

## Composite Actions

To use a composite action, simple add the following code snippet to an exist **job** in your workflow

```yml
jobs:
  example-job: 
    runs-on: 'ubuntu-latest'
    steps:
      # ... other job steps
      - name: Run reusable composite action
        uses: AllenNeuralDynamics/.github/.github/actions/<action_name>@<branch>
      # ... other job steps
```

``<action_name>`` corresponds to a composite action found in this repository. 

The ``@<branch>`` is optional, just like in the reusable workflow example.

# Contribution

Make sure to read the [standards](#reusable-workflow-standards) first before adding any new reusable workflows or templates. 

Also before creating your CI/CD components its important to note what are the different responsibilities of workflows, templates and actions. 

- **Templates:** entire CI/CD pipeline. A user should be able to integrate this to a relevant project and it should work with little edits needed on the users end.
- **Workflows:** these are "jobs" that users will run. Workflows **do not persist data between each other**. If you have a CI/CD step that generates some sort of data that will later be used in a different step, make this a composite action instead.
- **Composite Actions:** reusable "steps" within a job. 



## Adding new workflows/templates/actions

1. Create a new branch and add your workflows/templates/actions in the corresponding locations: 

    - ``.github/workflows/<workflow_name>`` for new reusable workflow
    - ``.github/workflow_templates/<workflow_template_name>`` for new workflow template name
    - ``.github/actions/<action_name>/action.yml`` for new composite actions


2. Make sure your new components adhere to the naming convention outlined in the [standards](#reusable-workflow-standards) section below. 

3. Create documentation for any new reusable workflows or actions. Use copier to use pre-existing documentation template: 

    ```
    uvx copier copy copier_template .
    ```

4. Create PR request to merge to main

## Testing

Reusable workflows and composite actions can be tested in a different repository by specifying the branch name when referencing them. 

Example:

```yml
jobs:
  test-reusable-workflow:
    uses: AllenNeuralDynamics/.github/.github/workflows/<new_workflow_name>@<custom_branch> 
```

# Standards

Majority of these standards are based off of this [article](https://wellarchitected.github.com/library/collaboration/recommendations/scaling-actions-reusability/?utm_source=chatgpt.com#3-define-contribution-guidelines-and-best-practices-for-adding-new-workflows-and-actions)

## Reusable Workflow Standards

Reusable workflows are entire "jobs" that runs in an existing github action workflow. 

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
