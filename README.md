# ado-reusable-workflows-terraform
Repository for storing and sharing reusable workflows produced for OpenTofu and Terraform in ADO EACD.

## OpenTofu and Terraform
This reusable workflow combines all the steps for running your OpenTofu (recommended) or legacy Terraform IAC in a single step.

## What's New
- **OpenTofu support** with state encryption for enhanced security
- **Terraform legacy support** for projects not yet migrated

## Usage

### Pre-requisites
Create a workflow `.yml` file in your repositories `.github/workflows` directory. An [example workflow](#example-workflow) is available below. For more information, reference the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).


### Inputs

* `run-apply` - (optional) Whether or not to run terraform apply as part of the process. Defaults to `false`
* `tofu-version` - (required for OpenTofu) Version of OpenTofu to use (e.g., `1.10.5`)
* `terraform-version` - (required for Terraform) Version of Terraform to use
* `iac-path` - (optional) Path to your IAC module folder. Defaults to 'iac/dev'
* `cache-key-suffix` - (optional) Suffix for PR comment cache key. Defaults to empty string

### Secrets

* `AWS_ACCESS_KEY_ID` - (required) AWS Access Key (Stored in Org Secrets), by GitHub design this has to be passed.  
* `AWS_SECRET_ACCESS_KEY` - (required) AWS Secret Access Key (Stored in Org Secrets.)
* `TF_LOCAL_STATE_ENCRYPTION_KEY` - (required by OpenTofu) Local state encryption key
* `TF_SHARED_RESOURCES_STATE_ENCRYPTION_KEY` - (required by OpenTofu) Shared resources state encryption key
* `TF_SECRETS:` -  (optional) JSON formatted array of secrets (name, value) to be injected as environment variables

### Outputs

* `tofu-outputs` - (OpenTofu) JSON formatted outputs from `tofu apply`
* `terraform-outputs` - (Terraform) JSON formatted outputs from `terraform apply`

## Example Workflows

### OpenTofu Workflow (Recommended)

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

### Terraform Workflow (Legacy)

```yaml
name: Terraform Workflow

on: 
  pull_request:

jobs:      
  terraform:
    uses: nit-administrative-systems/ado-reusable-workflows-terraform/.github/workflows/terraform-reusable.yml@main
    with:
      iac-path: 'iac/dev'
      run-apply: false
      terraform-version: '1.10.5'
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.TF_KEY_ADO_NONPROD }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.TF_SECRET_ADO_NONPROD }}
      TF_SECRETS: >-
        [
           { 
             \"name\" : \"EXAMPLE_NAME\",
             \"value\" : \"${{ secrets.EXP_SECRET }}\"
            }
         ]    
```
## Migration from Terraform to OpenTofu
For projects migrating from Terraform to OpenTofu, please refer to the [ADOES OpenTofu Migration Guide](https://eacd.entapp.northwestern.edu/tech-stacks/opentofu/from-terraform.html).

## Features
* Format check: validates code formatting
* Init: Initializes the backend and providers
* Validate: Validates the configuration syntax
* Plan: Shows planned infrastructure changes
* Apply: Applies changes (when `run-apply: true`)
* PR Comments: Automatically posts plan results to pull requests
