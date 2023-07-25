# pagopa-ecommerce-dashboard
This project contains GHA for generate dashboards and relative alerts starting from openapi definition stored remotely.

Dashboard generation is performed using [Opex dashboard project](https://github.com/pagopa/opex-dashboard)

## Github actions

### Create Dashboad

This is the action that start dashboard creation process through Opex and publish dashboard by the meaning of terraform.

This action can be run both manually and using webhook.

#### Parameters

| Parameter name  | Description                                                                                                                                                                | Mandatory | 
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|
| api-name        | The name of the API for which create dashboards and alerts. This name is used to point to the right `.opex` subfolder which contains required configurations for opex tool | yes       |
| environment     | The environment for which create dashboards and alert.                                                                                                                     | yes       |
| config-options  | JSON stringify object used to parameterize opex config.yaml file                                                                                                           | no        | 

#### Config.yaml parameterization

Since this project contains dashboards creation code that point to remote ref (such as infra repo) some config parameters need parameterization.

This is done using the `config-options` workflow string parameters.

All key-value parameters will be set as env variables that are used for `envsubst` command run against config.yaml file

For example remote url branch can be parameterized using:

`config.yaml file`

oa3_spec: https://raw.githubusercontent.com/pagopa/pagopa-infra/${branch_ref}/src/domains/ecommerce-app/api/.../_openapi.json.tpl 

And then the wanted value is parameterized with 

config-option value
```json
{"branch_ref": "main"}
```