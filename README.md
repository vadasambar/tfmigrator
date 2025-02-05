# tfmigrator

[![Build Status](https://github.com/suzuki-shunsuke/tfmigrator/workflows/test/badge.svg)](https://github.com/suzuki-shunsuke/tfmigrator/actions)
[![Go Report Card](https://goreportcard.com/badge/github.com/suzuki-shunsuke/tfmigrator)](https://goreportcard.com/report/github.com/suzuki-shunsuke/tfmigrator)
[![GitHub last commit](https://img.shields.io/github/last-commit/suzuki-shunsuke/tfmigrator.svg)](https://github.com/suzuki-shunsuke/tfmigrator)
[![License](http://img.shields.io/badge/license-mit-blue.svg?style=flat-square)](https://raw.githubusercontent.com/suzuki-shunsuke/tfmigrator/main/LICENSE)

CLI tool to migrate Terraform configuration and State

## Check tfmigrator/tfmigrator

https://github.com/tfmigrator/tfmigrator is a Go framework for migration.
Please see https://github.com/tfmigrator/tfmigrator#compared-with-suzuki-shunsuketfmigrator .

## Blog written in Japanese

[terraformer で雑に生成した tf ファイル と state を分割したくてツールを書いた](https://techblog.szksh.cloud/tfmigrator/)

## Overview

`tfmigrator` is a CLI tool to migrate Terraform configuration and State into multiple states.
`tfmigrator` configures rules to classify resources and migrate resources to other states via `terraform state mv` and [hcledit](https://github.com/minamijoyo/hcledit).

## Requirement

* Terraform CLI
* [hcledit](https://github.com/minamijoyo/hcledit)

## Install

Download a binary from the [replease page](https://github.com/suzuki-shunsuke/tfmigrator/releases).

## How to use

```
$ vi tfmigrator.yaml
$ cat *.tf | tfmigrator run [-skip-state]
```

## Configuration file

[CONFIGURATION.md](docs/CONFIGURATION.md)

example of tfmigrator.yaml

```yaml
items:
- rule: |
    "name" not in Values
  exclude: true
- rule: |
    Values.name contains "foo"
  state_out: foo/terraform.tfstate
  resource_name: "{{.Values.name}}"
  tf_path: foo/resource.tf
- rule: |
    Values.name contains "bar"
  state_out: bar/terraform.tfstate
  resource_name: "{{.Values.name}}"
  tf_path: bar/resource.tf
```

## Restriction

tfmigrator migrates Terraform configuration with hcledit and doesn't support to expand the expression.
For example, if Terraform configuration refers local values, the migrated configuration may be broken.

## LICENSE

[MIT](LICENSE)
