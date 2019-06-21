# EJERCICIO 5

Para una posterior aplicación empresarial es necesario desplegar una aplicación web
en la que los usuarios deben poder subir imágenes y la aplicación debe mostrarlas y
permitir su redimensionamiento. El código de esta aplicación lo puedes encontrar en
GitHub y tu labor consiste en implementar la infraestructura necesaria para que la
aplicación funcione en su totalidad (https://github.com/Azure-Samples/storage-blobupload-
from-webapp).

## Creación de cuenta de almacenamiento

Primero procederemos a crear una cuenta de almacenamiento mediante Azure CLI, la cual asociaremos a la Web App posteriormente:

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
## Creación de los contenedores

La aplicación necesitará de dos contenedores para albergar las imgágenes que subamos a ella. Ejecutaremos el siguiente código para su creación:

```Bash
blobStorageAccountKey=$(az storage account keys list -g RG-WebApp \
-n $blobStorageAccount --query [0].value --output tsv)

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off

az storage container create -n thumbnails --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your Blob storage account key..."
echo $blobStorageAccountKey
```
Copiaremos la "Key" que nos genera para la configuración de la WebApp.

## Creación de un plan de App Service

A continuación crearemos el Service Plan para nuestra aplicación.

```Bash
az appservice plan create --name aspmat02t38w03 --resource-group RG-WebApp --sku Free
```

Obtendremos la siguiente salida:

```Bash
{
  "freeOfferExpirationTime": null,
  "geoRegion": "East US",
  "hostingEnvironmentProfile": null,
  "hyperV": false,
  "id": "/subscriptions/****************************/resourceGroups/RG-WebApp/providers/Microsoft.Web/serverfarms/aspmat02t38w03",
  "isSpot": false,
  "isXenon": false,
  "kind": "app",
  "location": "East US",
  "maximumElasticWorkerCount": 1,
  "maximumNumberOfWorkers": 1,
  "name": "aspmat02t38w03",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": false,
  "resourceGroup": "RG-WebApp",
  "sku": {
    "capabilities": null,
    "capacity": 0,
    "family": "F",
    "locations": null,
    "name": "F1",
    "size": "F1",
    "skuCapacity": null,
    "tier": "Free"
  },
  "spotExpirationTime": null,
  "status": "Ready",
  "subscription": "****************************",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

## Creación Web app

A continuación procederemos a crear la Web App:

```Bash
webapp=webmat02t38w03

az webapp create --name $webapp --resource-group RG-WebApp --plan aspmat02t38w03
```

Obtendremos la siguiente salida:

```Bash
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "webmat02t38w03.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "webmat02t38w03.azurewebsites.net",
    "webmat02t38w03.scm.azurewebsites.net"
  ],
  "ftpPublishingUrl": "ftp://waws-prod-blu-139.ftp.azurewebsites.windows.net/site/wwwroot",
  "geoDistributions": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "ipBasedSslResult": null,
      "ipBasedSslState": "NotConfigured",
      "name": "webmat02t38w03.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "toUpdateIpBasedSsl": null,
      "virtualIp": null
    },
    {
      "hostType": "Repository",
      "ipBasedSslResult": null,
      "ipBasedSslState": "NotConfigured",
      "name": "webmat02t38w03.scm.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "toUpdateIpBasedSsl": null,
      "virtualIp": null
    }
  ],
  "hostNames": [
    "webmat02t38w03.azurewebsites.net"
  ],
  "hostNamesDisabled": false,
  "hostingEnvironmentProfile": null,
  "httpsOnly": false,
  "hyperV": false,
  "id": "/subscriptions/************************************/resourceGroups/RG-WebApp/providers/Microsoft.Web/sites/webmat02t38w03",
  "identity": null,
  "inProgressOperationId": null,
  "isDefaultContainer": null,
  "isXenon": false,
  "kind": "app",
  "lastModifiedTimeUtc": "2019-06-21T16:22:41.496666",
  "location": "East US",
  "maxNumberOfWorkers": null,
  "name": "webmat02t38w03",
  "outboundIpAddresses": "40.79.154.194,137.117.100.130,137.117.103.80,137.117.97.189,137.117.101.166",
  "possibleOutboundIpAddresses": "40.79.154.194,137.117.100.130,137.117.103.80,137.117.97.189,137.117.101.166,137.117.98.138,137.117.99.211,137.117.97.27",
  "redundancyMode": "None",
  "repositorySiteName": "webmat02t38w03",
  "reserved": false,
  "resourceGroup": "RG-WebApp",
  "scmSiteAlsoStopped": false,
  "serverFarmId": "/subscriptions/************************************/resourceGroups/RG-WebApp/providers/Microsoft.Web/serverfarms/aspmat02t38w03",
  "siteConfig": null,
  "slotSwapStatus": null,
  "state": "Running",
  "suspendedTill": null,
  "tags": null,
  "targetSwapSlot": null,
  "trafficManagerHostNames": null,
  "type": "Microsoft.Web/sites",
  "usageState": "Normal"
}
```

## Implementación de la aplicación de ejemplo desde el repositorio de GitHub

Ahora tendremos que implementar el repositorio de GitHub en nuestra Web App, para ello ejecutaremos el siguiente código:

```Bash
az webapp deployment source config --name $webapp \
--resource-group RG-WebApp --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

Obtendremos la siguiente salida:

```Bash
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/************************************/resourceGroups/RG-WebApp/providers/Microsoft.Web/sites/webmat02t38w03/sourcecontrols/web",
  "isManualIntegration": true,
  "isMercurial": false,
  "kind": null,
  "location": "East US",
  "name": "webmat02t38w03",
  "repoUrl": "https://github.com/Azure-Samples/storage-blob-upload-from-webapp",
  "resourceGroup": "RG-WebApp",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## Configuración

En esta parte configuraremos la Web App para que realice la carga de las imágenes que le pasaremos:

```Bash
az webapp config appsettings set --name $webapp --resource-group RG-WebApp \
--settings AzureStorageConfig__AccountName=$blobStorageAccount \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountKey=$blobStorageAccountKey
```

Obtendremos la siguiente salida:

```Bash
[
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "10.14"
  },
  {
    "name": "AzureStorageConfig__AccountName",
    "slotSetting": false,
    "value": "sawebmat02t38w03"
  },
  {
    "name": "AzureStorageConfig__ImageContainer",
    "slotSetting": false,
    "value": "images"
  },
  {
    "name": "AzureStorageConfig__ThumbnailContainer",
    "slotSetting": false,
    "value": "thumbnails"
  },
  {
    "name": "AzureStorageConfig__AccountKey",
    "slotSetting": false,
    "value": "*****************************************"
  }
]
```

## Event Grid

Una vez que tenemos configurada la carga de imágenes en nuestra Web App, deberemos de configurar Event Grid, para poder automatizar el proceso de mover las imágenes de contenedores en nuestra cuenta de almacenamiento:

Lo primero que tendremos que realizar es registrar el proveedor de Event Grid:

```Bash
az provider register --namespace Microsoft.EventGrid
```

Para la función que crearemos a continuación necesitaremos crear una cuenta de almacenamiento a parte:

```Bash
functionstorage=safunmat02t38w03

az storage account create --name $functionstorage --location eastus \
--resource-group RG-WebApp --sku Standard_LRS --kind storage
```

Obtendremos la siguiente salida:

```Bash
{
  "accessTier": null,
  "creationTime": "2019-06-21T16:42:09.095015+00:00",
  "customDomain": null,
  "enableAzureFilesAadIntegration": null,
  "enableHttpsTrafficOnly": false,
  "encryption": {
    "keySource": "Microsoft.Storage",
    "keyVaultProperties": null,
    "services": {
      "blob": {
        "enabled": true,
        "lastEnabledTime": "2019-06-21T16:42:09.235689+00:00"
      },
      "file": {
        "enabled": true,
        "lastEnabledTime": "2019-06-21T16:42:09.235689+00:00"
      },
      "queue": null,
      "table": null
    }
  },
  "failoverInProgress": null,
  "geoReplicationStats": null,
  "id": "/subscriptions/*****************************/resourceGroups/RG-WebApp/providers/Microsoft.Storage/storageAccounts/safunmat02t38w03",
  "identity": null,
  "isHnsEnabled": null,
  "kind": "Storage",
  "lastGeoFailoverTime": null,
  "location": "eastus",
  "name": "safunmat02t38w03",
  "networkRuleSet": {
    "bypass": "AzureServices",
    "defaultAction": "Allow",
    "ipRules": [],
    "virtualNetworkRules": []
  },
  "primaryEndpoints": {
    "blob": "https://safunmat02t38w03.blob.core.windows.net/",
    "dfs": null,
    "file": "https://safunmat02t38w03.file.core.windows.net/",
    "queue": "https://safunmat02t38w03.queue.core.windows.net/",
    "table": "https://safunmat02t38w03.table.core.windows.net/",
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

## Creación de una aplicación de función

La función nos proporcionará un ámbito de ejecución sin ningún servidor. Para ello ejecutaremos el siguiente código:

```Bash
functionapp=safunmat02t38w03

az functionapp create --name $functionapp --storage-account $functionstorage \
--resource-group RG-WebApp --consumption-plan-location eastus
```

Obtendremos la siguiente salida:

```Bash
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": false,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "safunmat02t38w03.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "safunmat02t38w03.azurewebsites.net",
    "safunmat02t38w03.scm.azurewebsites.net"
  ],
  "geoDistributions": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "ipBasedSslResult": null,
      "ipBasedSslState": "NotConfigured",
      "name": "safunmat02t38w03.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "toUpdateIpBasedSsl": null,
      "virtualIp": null
    },
    {
      "hostType": "Repository",
      "ipBasedSslResult": null,
      "ipBasedSslState": "NotConfigured",
      "name": "safunmat02t38w03.scm.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "toUpdateIpBasedSsl": null,
      "virtualIp": null
    }
  ],
  "hostNames": [
    "safunmat02t38w03.azurewebsites.net"
  ],
  "hostNamesDisabled": false,
  "hostingEnvironmentProfile": null,
  "httpsOnly": false,
  "hyperV": false,
  "id": "/subscriptions/*****************************/resourceGroups/RG-WebApp/providers/Microsoft.Web/sites/safunmat02t38w03",
  "identity": null,
  "inProgressOperationId": null,
  "isDefaultContainer": null,
  "isXenon": false,
  "kind": "functionapp",
  "lastModifiedTimeUtc": "2019-06-21T16:50:12.813333",
  "location": "eastus",
  "maxNumberOfWorkers": null,
  "name": "safunmat02t38w03",
  "outboundIpAddresses": "40.79.154.194,137.117.100.130,137.117.103.80,137.117.97.189,137.117.101.166",
  "possibleOutboundIpAddresses": "40.79.154.194,137.117.100.130,137.117.103.80,137.117.97.189,137.117.101.166,137.117.98.138,137.117.99.211,137.117.97.27",
  "redundancyMode": "None",
  "repositorySiteName": "safunmat02t38w03",
  "reserved": false,
  "resourceGroup": "RG-WebApp",
  "scmSiteAlsoStopped": false,
  "serverFarmId": "/subscriptions/*****************************/resourceGroups/RG-WebApp/providers/Microsoft.Web/serverfarms/EastUSPlan",
  "siteConfig": null,
  "slotSwapStatus": null,
  "state": "Running",
  "suspendedTill": null,
  "tags": null,
  "targetSwapSlot": null,
  "trafficManagerHostNames": null,
  "type": "Microsoft.Web/sites",
  "usageState": "Normal"
}
```

A continuación deberemos de configurar la función ya que necesita de credenciales para su ejecución:

```Bash
blobStorageAccount=sawebmat02t38w03
storageConnectionString=$(az storage account show-connection-string --resource-group RG-WebApp --name $blobStorageAccount --query connectionString --output tsv)

az functionapp config appsettings set --name $functionapp --resource-group RG-WebApp \
--settings AzureWebJobsStorage=$storageConnectionString THUMBNAIL_CONTAINER_NAME=thumbnails \
THUMBNAIL_WIDTH=100 FUNCTIONS_EXTENSION_VERSION=~2
```

Obtendremos la siguiente salida:

```Bash
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "AzureWebJobsStorage",
    "slotSetting": false,
    "value": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=sawebmat02t38w03;AccountKey=****************"
  },
  {
    "name": "AzureWebJobsDashboard",
    "slotSetting": false,
    "value": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=safunmat02t38w03;AccountKey=****************"
  },
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "10.14.1"
  },
  {
    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
    "slotSetting": false,
    "value": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=safunmat02t38w03;AccountKey=****************"
  },
  {
    "name": "WEBSITE_CONTENTSHARE",
    "slotSetting": false,
    "value": "safunmat02t38w03"
  },
  {
    "name": "THUMBNAIL_CONTAINER_NAME",
    "slotSetting": false,
    "value": "thumbnails"
  },
  {
    "name": "THUMBNAIL_WIDTH",
    "slotSetting": false,
    "value": "100"
  }
]
```

## Implementación del código de función

Para que pueda redimensionar el tamaño lo implementaremos desde otro repositorio de GitHub:

```Bash
az functionapp deployment source config --name $functionapp --resource-group RG-WebApp --branch master --manual-integration --repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

Obtendremos la siguiente salida:

```Bash
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/*******************/resourceGroups/RG-WebApp/providers/Microsoft.Web/sites/safunmat02t38w03/sourcecontrols/web",
  "isManualIntegration": true,
  "isMercurial": false,
  "kind": null,
  "location": "East US",
  "name": "safunmat02t38w03",
  "repoUrl": "https://github.com/Azure-Samples/function-image-upload-resize",
  "resourceGroup": "RG-WebApp",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## Creación de una suscripción a evento

A continuación deberemos de crear la suscripción al evento para que envíe las notificaciones al portal de Azure:

<p align="center">
  <a><img src="https://i.imgur.com/mVH0yYv.png" title="suscripcion" /></a>
</p>

Dejaremos la configuración como se indica en la siguiente imagen:

<p align="center">
  <a><img src="https://i.imgur.com/t6y1xQZ.png" title="source: imgur.com" /></a>
</p>
