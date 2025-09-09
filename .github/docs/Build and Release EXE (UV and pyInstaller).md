# Build and Release EXE (UV and pyInstaller)

**Name:** ``Build and Release EXE (UV and pyInstaller)``

**Workflow File:** ``release-exe-uv_pyinstaller-python.yml``

**Description:** 

This workflow performs the following steps:

- Builds executable project with pyinstaller. The commands for pyinstaller can be passed in as an input variable 
- Auto zips the distributable from the pyinstaller step. The name of the zip can be passed in as an input variable
- Publishes the zip to Github Releases. If a tag name isn't provided, it will use the current version (set in the ``pyproject.toml``) as the tag name (with the "v" prefix e.g. v1.0.0).
- Publishes the zip to [eng-tools/release](http://eng-tools/release). If the project name and project version isn't given, it will pull it from the ``pyproject.toml``. Below is an example of how the project name and project version is used to deploy to [eng-tools/release](http://eng-tools/release):
  - project-name: "test_name"
  - project-version: "1.0.0.custom"
  - zip-name: "test_project"
  - Creates the following: http://eng-tools/test_name/1.0.0.custom/test_project.zip

**Created By:** Jessy Liao (SIPE)

## Parameters

**Inputs:** 

- ``runner-label`` (optional): runner to use 
  - default: to "eng-tools"
- ``project-name`` (optional): project name to use (used to set eng-tools/release directory).
  - default: uses project name from ``pyproject.toml`` if no value given
- ``project-version`` (optional): project version to use (used to set eng-tools/release subdirectory). 
  - default: uses project version from ``pyproject.toml`` if no value given
- ``tag-name`` (optional): tag name to upload release to. 
  - default: uses project version from ``pyproject.toml`` if no value given.
- ``zip-name`` (optional): what to name zip file. 
  - default: uses project name from ``pyproject.toml`` if no value given
- ``exe-name`` (optional): what to name executable.
  - default: uses project name from ``pyproject.toml`` if no value given
- ``pyinstaller-commands`` (optional): custom pyinstaller commands to run. 
  - default: runs ``pyinstaller --name <exe-name> src/<project-name>/main.py``

**Outputs:** N/A

## Examples

### Example 1: Publish on new tag

**workflow.yml**
```yml
name: Tag and Publish

on:
  push:
    tags:
      - '*'         # Triggers on any new tag 
      # - 'v*'      # Triggers on tags starting with 'v' 
      # - 'v*.*.*'  # Triggers on tags matching a specific version pattern 
jobs: 
  publish: 
    needs: tag
    uses: AllenNeuralDynamics/.github/.github/workflows/release-exe-uv_pyinstaller-python.yml@main
    with: 
      tag-name: ${{ github.ref }}
```

**Results:**

- Builds executable and zips it (uses project name from ``pyproject.toml`` as default for executable name and zip name)
- Uploads zip file to the new tag that triggered this job
- Calls API to get eng-tools to download assets from latest tag (this new one)

### Example 2: Tag + Publish

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
    uses: AllenNeuralDynamics/.github/.github/workflows/release-tag-uv-python.yml@main
  publish: 
    needs: tag
    uses: AllenNeuralDynamics/.github/.github/workflows/release-exe-uv_pyinstaller-python.yml@main
    with: 
      project-version: ${{ needs.auto-tag.outputs.version }} 
      tag-name: v${{ needs.auto-tag.outputs.version }}
```

**Results:**

- Auto tags, build, and publish whenever a push is made to main or dev. 
- Because publish is running in the same workflow as tag, the version must be passed into publish (if it isn't the publish workflow will reference the previous version).  


