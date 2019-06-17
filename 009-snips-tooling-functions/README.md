# Snips Azure Functions Testing

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
