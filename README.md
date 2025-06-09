# dataliquid GitHub Actions

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

This repository contains reusable GitHub Actions workflows for dataliquid projects.

## Quick Start

To quickly install the Gitflow workflows in your project, run:

```bash
curl -s https://raw.githubusercontent.com/dataliquid/github-actions/main/install-gitflow-workflows.sh | bash
```

This will download and install both gitflow-release and gitflow-hotfix workflows in your project.

## Available Workflows

### Gitflow Workflows

#### Gitflow Release Workflow

Manages the gitflow release process with automatic branch detection.

**File**: `.github/workflows/gitflow-release.yml`

**Features**:
- Start a new release branch
- Finish a release with automatic merge to master and develop
- Auto-detection of single release branch for finish action
- Configurable Java version and distribution

#### Gitflow Hotfix Workflow

Manages the gitflow hotfix process for emergency fixes.

**File**: `.github/workflows/gitflow-hotfix.yml`

**Features**:
- Start a new hotfix branch from master
- Finish a hotfix with automatic merge to master and develop
- Configurable Java version and distribution

## Usage

To use these workflows in your project, create a workflow file in your repository that calls the reusable workflow:

### Example: Gitflow Release

Create `.github/workflows/gitflow-release.yml` in your repository:

```yaml
name: Gitflow Release

on:
  workflow_dispatch:
    inputs:
      action:
        description: 'Gitflow action to perform'
        required: true
        type: choice
        options:
          - release-start
          - release-finish
      version:
        description: 'Release version (optional for auto-detection)'
        required: false
        type: string

jobs:
  release:
    uses: dataliquid/github-actions/.github/workflows/gitflow-release.yml@main
    with:
      action: ${{ inputs.action }}
      version: ${{ inputs.version }}
```

### Example: Gitflow Hotfix

Create `.github/workflows/gitflow-hotfix.yml` in your repository:

```yaml
name: Gitflow Hotfix

on:
  workflow_dispatch:
    inputs:
      action:
        description: 'Gitflow action to perform'
        required: true
        type: choice
        options:
          - hotfix-start
          - hotfix-finish
      version:
        description: 'Hotfix version (e.g., 1.17.1)'
        required: true
        type: string

jobs:
  hotfix:
    uses: dataliquid/github-actions/.github/workflows/gitflow-hotfix.yml@main
    with:
      action: ${{ inputs.action }}
      version: ${{ inputs.version }}
```

## Workflow Parameters

### Common Parameters

All workflows support the following optional parameters:

- `java-version`: Java version to use (default: '8')
- `java-distribution`: Java distribution to use (default: 'temurin')
- `runs-on`: Runner OS (default: 'ubuntu-latest')

### Gitflow Release Parameters

- `action`: Required. Either 'release-start' or 'release-finish'
- `version`: Optional. Release version (e.g., '1.18.0'). If not provided for release-finish, will auto-detect if only one release branch exists

### Gitflow Hotfix Parameters

- `action`: Required. Either 'hotfix-start' or 'hotfix-finish'
- `version`: Required. Hotfix version (e.g., '1.17.1')

## Requirements

Your project must:
- Use Maven with the gitflow plugin configured
- Have proper Git configuration
- Have appropriate permissions for the workflow to push to protected branches

## Examples

See the `examples/` directory for complete workflow examples that you can copy to your project.

## Contributing

To add new reusable workflows:
1. Create the workflow file in `.github/workflows/`
2. Use `workflow_call` as the trigger
3. Document the workflow in this README
4. Add an example in the `examples/` directory