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
  
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
