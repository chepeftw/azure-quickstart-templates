# Snips On Premise Infrastructure

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fchepeftw%2Fazure-quickstart-templates%2Fmaster%2F007-snips-on-premise-template%2FmainTemplate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fchepeftw%2Fazure-quickstart-templates%2Fmaster%2F007-snips-on-premise-template%2FmainTemplate.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

## Test the mainTemplae.json

To validate or dry-run:

```bash
az group deployment validate --resource-group templateDeployments --template-file mainTemplate.json  --parameters @azuredeploy.parameters.json
```

To run:

```bash
az group create --name TestGroup201906171649 --location "West US"
az group deployment create --name TestDeployment201906171649 --resource-group templateDeployments --template-file mainTemplate.json  --parameters @azuredeploy.parameters.json
```
