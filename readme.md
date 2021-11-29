# composite-azure-deploy-functionapp

This a composite action wrapping the following functionality

1. Render environment specific variables with jnus/json-variables
2. Substitute variables in configuration with microsoft/substitute-variables
3. Deploy artifacts to an Azure function app with Azure/functions-action@v1

## Usage

Reference the composite action with a full sha like the following to avoid breaking changes in the repo:
```yaml
segesdk/composite-azure-deploy-functionapp@<INSERT SHA>
```
or like the following, to always use latest version by referencing @main

```yaml
segesdk/composite-azure-deploy-functionapp@main
```

## Parameters

| Input variable | Description |
|----------|----------|
| publishPath | Publishing path of the build artifacts |
| environment | Target environment to deploy to. Used for rendering environment variables and deployment to Azure
|variableFile| Full path to json file with variables for rendering with jnus/json-variables|
|secrets|Serialized json string for github secrets, used to render environment variables. Syntax: `'${{ toJson(secrets) }}'`|
|configurationFile|Full path to configuration file to perform variable substitution|
|functionAppName|Name of the target Azure function app|
|azureCredentials|Service principal credentials|

## Usage example
```yaml
- id: Deployment
       uses: segesdk/composite-azure-deploy-functionapp@main
       with:
         publishPath: ${{env.PUBLISH_PATH}}
         environment: Dev
         variableFile: '${{env.PUBLISH_PATH}}/variables.json'
         secrets: '${{ toJson(secrets) }}'
         configurationFile: '${{env.PUBLISH_PATH}}/appsettings.json'
         functionAppName:  ${{env.PROJECT_NAME}}-dev-api-wa
         azureCredentials: ${{ secrets.AZURE_SEGES_SYSUDV_UDV_SECRET }}
```


