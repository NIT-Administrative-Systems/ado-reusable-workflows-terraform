# ado-reusable-workflows-terraform

Repository for storing and sharing the reusable OpenTofu workflow produced for ADOES.

## OpenTofu

This reusable workflow combines all the steps for running your OpenTofu IAC in a single step.

## What's New

See the [CHANGELOG.md](./CHANGELOG.md) file.

## Usage

### Pre-requisites

Create a workflow `.yml` file in your repositories `.github/workflows` directory. An [example workflow](#example-workflow) is available below. For more information, reference the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).

### Inputs

* `run-apply` - (optional) Whether or not to run `tofu apply` as part of the process. Defaults to `false`
* `tofu-version` - (required) Version of OpenTofu to use (e.g., `1.10.5`)
* `iac-path` - (optional) Path to your IAC module folder. Defaults to 'iac/dev'
* `cache-key-suffix` - (optional) Suffix for PR comment cache key. Defaults to empty string

### Secrets

* `AWS_ACCESS_KEY_ID` - (required) AWS Access Key (Stored in Org Secrets), by GitHub design this has to be passed.  
* `AWS_SECRET_ACCESS_KEY` - (required) AWS Secret Access Key (Stored in Org Secrets.)
* `TF_LOCAL_STATE_ENCRYPTION_KEY` - (required) Local state encryption key
* `TF_SHARED_RESOURCES_STATE_ENCRYPTION_KEY` - (required) Shared resources state encryption key
* `TF_SECRETS:` -  (optional) JSON formatted array of secrets (name, value) to be injected as environment variables

### Outputs

* `tofu-outputs` - JSON formatted outputs from `tofu apply`

## Example Workflow

```yaml
name: OpenTofu Workflow

on: 
  pull_request:

jobs:      
  open-tofu:
    uses: nit-administrative-systems/ado-reusable-workflows-terraform/.github/workflows/tofu-reusable.yml@main
    with:
      iac-path: 'iac/dev'
      run-apply: false
      tofu-version: '1.10.5'
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.TF_KEY_ADO_NONPROD }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.TF_SECRET_ADO_NONPROD }}
      TF_LOCAL_STATE_ENCRYPTION_KEY: ${{ secrets.TF_LOCAL_STATE_ENCRYPTION_KEY_2025_07 }}
      TF_SHARED_RESOURCES_STATE_ENCRYPTION_KEY: ${{ secrets.TF_SHARED_RESOURCES_STATE_ENCRYPTION_KEY_2025_07 }}
      TF_SECRETS: >-
        [
           { 
             \"name\" : \"EXAMPLE_NAME\",
             \"value\" : \"${{ secrets.EXP_SECRET }}\"
            }
         ]
```

## Features

* Format check: validates code formatting
* Init: Initializes the backend and providers
* Validate: Validates the configuration syntax
* Plan: Shows planned infrastructure changes
* Apply: Applies changes (when `run-apply: true`)
* PR Comments: Automatically posts plan results to pull requests
