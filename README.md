# TerraformBasicTemplate

Store ID of resource group in groupId variable:
```bash
groupId=$(az group show \
  --name <resource-group-name> \
  --query id --output tsv)
```

Create RBAC Service Principal:
```bash
az ad sp create-for-rbac \
  --scope $groupId \
  --role Contributor \
  --sdk-auth
```
Copy JSON from output to use it later in. Use *clientId* from output in role assignment command.

Store ID of container registry in registryId variable:
```bash
registryId=$(az acr show \
  --name <registry-name> \
  --resource-group <resource-group-name> \
  --query id --output tsv)
```

Assign AcrPush role to service principal which gives pull and push access to the registry (substitute <client-id> with clientId of service principal):
```bash
az role assignment create \
  --assignee <client-id> \
  --scope $registryId \
  --role AcrPush
```
  
In GitHub UI go to your repository and then **Security > Secrets and variables > Actions**.  
By clicking on **New repository secret** create secret for each of these below:
| Secret        | Value           |
| ------------- |:------------- |
| AZURE_CREDENTIALS     | The entire `JSON output` from the service principal creation step |
| REGISTRY_LOGIN_SERVER      | The login server name of your registry (all lowercase). Example: *myregistry.azurecr.io*      |
| REGISTRY_USERNAME | The `clientId` from the JSON output from the service principal creation      |
| REGISTRY_PASSWORD | The `clientSecret` from the JSON output from the service principal creation     |
| RESOURCE_GROUP | 	The name of the resource group you used to scope the service principal      |


