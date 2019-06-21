# EJERCICIO 5

Para una posterior aplicación empresarial es necesario desplegar una aplicación web
en la que los usuarios deben poder subir imágenes y la aplicación debe mostrarlas y
permitir su redimensionamiento. El código de esta aplicación lo puedes encontrar en
GitHub y tu labor consiste en implementar la infraestructura necesaria para que la
aplicación funcione en su totalidad (https://github.com/Azure-Samples/storage-blobupload-
from-webapp).

## Creación de cuenta de almacenamiento

Primero procederemos a crear una cuenta de almacenamiento en la cual guardaremos las imágenes subidas a la aplicación web.

```Bash
blobStorageAccount=sawebmat02t38w03

az storage account create --name $blobStorageAccount \
--location eastus --resource-group RG-WebApp \
--sku Standard_LRS --kind blobstorage --access-tier hot
```

Obtendremos la siguiente salida:

```Bash
{
  "accessTier": "Hot",
  "creationTime": "2019-06-21T16:07:55.153013+00:00",
  "customDomain": null,
  "enableAzureFilesAadIntegration": null,
  "enableHttpsTrafficOnly": false,
  "encryption": {
    "keySource": "Microsoft.Storage",
    "keyVaultProperties": null,
    "services": {
      "blob": {
        "enabled": true,
        "lastEnabledTime": "2019-06-21T16:07:55.324843+00:00"
      },
      "file": {
        "enabled": true,
        "lastEnabledTime": "2019-06-21T16:07:55.324843+00:00"
      },
      "queue": null,
      "table": null
    }
  },
  "failoverInProgress": null,
  "geoReplicationStats": null,
  "id": "/subscriptions/****************************/resourceGroups/RG-WebApp/providers/Microsoft.Storage/storageAccounts/sawebmat02t38w03",
  "identity": null,
  "isHnsEnabled": null,
  "kind": "BlobStorage",
  "lastGeoFailoverTime": null,
  "location": "eastus",
  "name": "sawebmat02t38w03",
  "networkRuleSet": {
    "bypass": "AzureServices",
    "defaultAction": "Allow",
    "ipRules": [],
    "virtualNetworkRules": []
  },
  "primaryEndpoints": {
    "blob": "https://sawebmat02t38w03.blob.core.windows.net/",
    "dfs": "https://sawebmat02t38w03.dfs.core.windows.net/",
    "file": null,
    "queue": null,
    "table": "https://sawebmat02t38w03.table.core.windows.net/",
    "web": null
  },
  "primaryLocation": "eastus",
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-WebApp",
  "secondaryEndpoints": null,
  "secondaryLocation": null,
  "sku": {
    "capabilities": null,
    "kind": null,
    "locations": null,
    "name": "Standard_LRS",
    "resourceType": null,
    "restrictions": null,
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": null,
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
```
