# ssm-parameter CfHighlander component

## Parameters

| Name | Use | Default | Global | Type | Allowed Values |
| ---- | --- | ------- | ------ | ---- | -------------- |
| EnvironmentName | Tagging | dev | true | string
| EnvironmentType | Tagging | development | true | string | ['development','production']
## Outputs/Exports

| Name | Value | Exported |
| ---- | ----- | -------- |
| DnsDomainZoneId | The hosted zone ID that was created, if it was created | false

## Included Components

<none>

## Example Configuration
### Highlander
```
    Component name: 'ssm', template: 'ssm-parameter' do
      parameter name: 'DBAddress', value: FnImportValue(FnSub('${EnvironmentName}-rdspostgres-DBEndpoint'))
      parameter name: 'DBPort', value: FnImportValue(FnSub('${EnvironmentName}-rdspostgres-DBPort'))
      parameter name: 'DBUser', value: FindInMap('AccountId', Ref('AWS::AccountId'), 'DbUser')
      parameter name: 'DBName', value: FindInMap('AccountId', Ref('AWS::AccountId'), 'DbName')
    end
```

### SSM Configuration
```
ssm_parameters:
  DbUser:
    name: "/app1/${EnvironmentType}/${EnvironmentName}/DATABASE_USER"
    value: ${DBUser}
    description: "The description for this parameter"
  DbAddress:
    name: "/app1/${EnvironmentType}/${EnvironmentName}/DATABASE_ADDRESS"
    value: ${DBAddress}
  DbPort:
    name: "/app1/${EnvironmentType}/${EnvironmentName}/DATABASE_PORT"
    value: ${DBPort}
  DbName:
    name: "/app1/${EnvironmentType}/${EnvironmentName}/DATABASE_NAME"
    value: ${DBName}
```

## Cfhighlander Setup

install cfhighlander [gem](https://github.com/theonestack/cfhighlander)

```bash
gem install cfhighlander
```

or via docker

```bash
docker pull theonestack/cfhighlander
```
## Testing Components

Running the tests

```bash
cfhighlander cftest ssm-parameter
```