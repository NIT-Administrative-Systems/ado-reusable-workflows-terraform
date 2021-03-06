# ado-reusable-workflows-terraform
Repository for storing and sharing reusable workflows produced for ADO EACD



## Terraform
This reusable workflow combines all the steps for running you Terraform IAC in a single step.

## Documentation


## What's New
  Everything!

## Usage

### Pre-requisites
Create a workflow `.yml` file in your repositories `.github/workflows` directory. An [example workflow](#example-workflow) is available below. For more information, reference the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).


### Inputs

* `run-apply` - (optional) Whether or not to run terraform apply as part of the process. Defaults to false
* `terraform-version` - (required) Version of terraform to use. 
* `iac-path` - (optional) Path to your folder for of your terraform module. Defaults to 'iac/dev'

### Secrets

* `AWS_ACCESS_KEY_ID` - (required) AWS Access Key (Stored in Org Secrets), by GitHub design this has to be passed.  
* `AWS_SECRET_ACCESS_KEY` - (required) AWS Secret Access Key (Stored in Org Secrets.)
* `TF_SECRETS:` -  (optional) JSON formatted array of secrets (name, value) to be injected into terraform

### Outputs

* `terraform-outputs` - Json formatted outputs from terraform

### Example workflow

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
      terraform-version: '0.12.31'
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
