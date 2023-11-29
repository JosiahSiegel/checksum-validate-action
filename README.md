# Checksum Validate Action

[![Test Action](https://github.com/JosiahSiegel/checksum-validate-action/actions/workflows/test_action.yml/badge.svg)](https://github.com/JosiahSiegel/checksum-validate-action/actions/workflows/test_action.yml)

## Synopsis

Output the following:
1. valid
   * `true` if checksums with matching keys are identical

## Usage

```yml
jobs:
  generate-checksums:
    name: Generate checksum
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4.1.1

      - name: Generate checksum of string
        uses: ./
        with:
          key: test string
          input: hello world

      - name: Generate checksum of command output
        uses: ./
        with:
          key: test command
          input: cat action.yml

  validate-checksums:
    name: Validate checksum
    needs:
      - generate-checksums
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4.1.1

      - name: Validate checksum of valid string
        id: valid-string
        uses: ./
        with:
          key: test string
          validate: true
          fail-invalid: true
          input: hello world

      - name: Validate checksum of valid command output
        id: valid-command
        uses: ./
        with:
          key: test command
          validate: true
          fail-invalid: true
          input: cat action.yml

      - name: Get outputs
        run: |
          echo ${{ steps.valid-string.outputs.valid }}
          echo ${{ steps.valid-command.outputs.valid }}
```

## Workflow summary

### :construction: Terraform Stats :construction:

* change-count: 2
* change-percent: 100
* resource-changes:
```json
[
  {
    "address": "docker_container.nginx",
    "changes": [
      "create"
    ]
  },
  {
    "address": "docker_image.nginx",
    "changes": [
      "create"
    ]
  }
]
```

## Inputs

```yml
inputs:
  terraform-directory:
    description: Terraform commands will run in this location.
    required: true
    default: "./terraform"
  include-no-op:
    description: "\"no-op\" refers to the before and after Terraform changes are identical as a value will only be known after apply."
    required: true
    default: false
  add-args:
    description: Pass additional arguments to Terraform plan.
    required: true
    default: ""
  upload-plan:
    description: Upload plan file. true or false
    required: true
    default: false
  upload-retention-days:
    description: Number of days to keep uploaded plan.
    required: true
    default: 7
  plan-file:
    description: Name of plan file.
    required: true
    default: tf__stats__plan.bin
  terraform-version:
    description: Specify a specific version of Terraform
    required: true
    default: latest
```

## Outputs
```yml
outputs:
  terraform-version:
    description: 'Terraform version'
  drift-count:
    description: 'Count of drifts'
  resource-drifts:
    description: 'JSON output of resource drifts'
  change-count:
    description: 'Count of changes'
  change-percent:
    description: 'Percentage of changes to total resources'
  resource-changes:
    description: 'JSON output of resource changes'
```