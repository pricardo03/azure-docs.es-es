---
title: 'Funciones de la plantilla de Azure Resource Manager: recursos | Microsoft Docs'
description: Describe las funciones para usar en una plantilla de Azure Resource Manager para recuperar valores sobre recursos.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 08/20/2019
ms.author: tomfitz
ms.openlocfilehash: 2cd37405176eefa8f4445942b9fbf1afc2a7404a
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69650429"
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Funciones de recursos para las plantillas de Azure Resource Manager

El Administrador de recursos ofrece las siguientes funciones para obtener valores de recursos:

* [list*](#list)
* [providers](#providers)
* [reference](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [suscripción](#subscription)

Para obtener valores de parámetro, variables o la implementación actual, consulte [Funciones con valores de implementación](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="list"></a>lista*

`list{Value}(resourceName or resourceIdentifier, apiVersion, functionValues)`

La sintaxis de esta función varía según el nombre de las operaciones de la lista. Cada implementación devuelve valores para el tipo de recurso que admite una operación de la lista. El nombre de la operación debe empezar por `list`. Algunos usos habituales son `listKeys` y `listSecrets`. 

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | type | DESCRIPCIÓN |
|:--- |:--- |:--- |:--- |
| resourceName o resourceIdentifier |Sí |string |Identificador único para el recurso. |
| apiVersion |Sí |string |Versión de API de estado en tiempo de ejecución de un recurso. Por lo general, en el formato, **aaaa-mm-dd**. |
| functionValues |Sin |objeto | Un objeto que tiene valores para la función. Proporcione este objeto solo para las funciones que admiten la recepción de un objeto con valores de parámetro, como **listAccountSas** en una cuenta de almacenamiento. En este artículo se muestra un ejemplo de cómo pasar los valores de funciones. | 

### <a name="implementations"></a>Implementaciones

Los usos posibles de la lista* se muestran en la tabla siguiente.

| Tipo de recurso | Nombre de función |
| ------------- | ------------- |
| Microsoft.AnalysisServices/servers | [listGatewayStatus](/rest/api/analysisservices/servers/listgatewaystatus) |
| Microsoft.AppConfiguration/configurationStores | ListKeys |
| Microsoft.Automation/automationAccounts | [listKeys](/rest/api/automation/keys/listbyautomationaccount) |
| Microsoft.Batch/batchAccounts | [listkeys](/rest/api/batchmanagement/batchaccount/getkeys) |
| Microsoft.BatchAI/workspaces/experiments/jobs | [listoutputfiles](/rest/api/batchai/jobs/listoutputfiles) |
| Microsoft.Blockchain/blockchainMembers | [listApiKeys](/rest/api/blockchain/2019-06-01-preview/blockchainmembers/listapikeys) |
| Microsoft.Blockchain/blockchainMembers/transactionNodes | [listApiKeys](/rest/api/blockchain/2019-06-01-preview/transactionnodes/listapikeys) |
| Microsoft.BotService/botServices/channels | listChannelWithKeys |
| Microsoft.Cache/redis | [listKeys](/rest/api/redis/redis/listkeys) |
| Microsoft.CognitiveServices/accounts | [listKeys](/rest/api/cognitiveservices/accountmanagement/accounts/listkeys) |
| Microsoft.ContainerRegistry/registries | [listBuildSourceUploadUrl](/rest/api/containerregistry/registries%20(tasks)/getbuildsourceuploadurl) |
| Microsoft.ContainerRegistry/registries | [listCredentials](/rest/api/containerregistry/registries/listcredentials) |
| Microsoft.ContainerRegistry/registries | [listPolicies](/rest/api/containerregistry/registries/listpolicies) |
| Microsoft.ContainerRegistry/registries | [listUsages](/rest/api/containerregistry/registries/listusages) |
| Microsoft.ContainerRegistry/registries/webhooks | [listEvents](/rest/api/containerregistry/webhooks/listevents) |
| Microsoft.ContainerRegistry/registries/runs | [listLogSasUrl](/rest/api/containerregistry/runs/getlogsasurl) |
| Microsoft.ContainerRegistry/registries/tasks | [listDetails](/rest/api/containerregistry/tasks/getdetails) |
| Microsoft.ContainerService/managedClusters | [listClusterAdminCredential](/rest/api/aks/managedclusters/listclusteradmincredentials) |
| Microsoft.ContainerService/managedClusters | [listClusterUserCredential](/rest/api/aks/managedclusters/listclusterusercredentials) |
| Microsoft.ContainerService/managedClusters/accessProfiles | [listCredential](/rest/api/aks/managedclusters/getaccessprofile) |
| Microsoft.DataBox/jobs | listCredentials |
| Microsoft.DataFactory/datafactories/gateways | listauthkeys |
| Microsoft.DataFactory/factories/integrationruntimes | [listauthkeys](/rest/api/datafactory/integrationruntimes/listauthkeys) |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers | [listSasTokens](/rest/api/datalakeanalytics/storageaccounts/listsastokens) |
| Microsoft.Devices/iotHubs | [listkeys](/rest/api/iothub/iothubresource/listkeys) |
| Microsoft.Devices/provisioningServices/keys | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeysforkeyname) |
| Microsoft.Devices/provisioningServices | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeys) |
| Microsoft.DevTestLab/labs | [ListVhds](/rest/api/dtl/labs/listvhds) |
| Microsoft.DevTestLab/labs/schedules | [ListApplicable](/rest/api/dtl/schedules/listapplicable) |
| Microsoft.DevTestLab/labs/users/serviceFabrics | [ListApplicableSchedules](/rest/api/dtl/servicefabrics/listapplicableschedules) |
| Microsoft.DevTestLab/labs/virtualMachines | [ListApplicableSchedules](/rest/api/dtl/virtualmachines/listapplicableschedules) |
| Microsoft.DocumentDB/databaseAccounts | [listConnectionStrings](/rest/api/cosmos-db-resource-provider/databaseaccounts/listconnectionstrings) |
| Microsoft.DocumentDB/databaseAccounts | [listKeys](/rest/api/cosmos-db-resource-provider/databaseaccounts/listkeys) |
| Microsoft.DomainRegistration | [listDomainRecommendations](/rest/api/appservice/domains/listrecommendations) |
| Microsoft.DomainRegistration/topLevelDomains | [listAgreements](/rest/api/appservice/topleveldomains/listagreements) |
| Microsoft.EventGrid/domains | [listKeys](/rest/api/eventgrid/domains/listsharedaccesskeys) |
| Microsoft.EventGrid/topics | [listKeys](/rest/api/eventgrid/topics/listsharedaccesskeys) |
| Microsoft.EventHub/namespaces/authorizationRules | [listkeys](/rest/api/eventhub/namespaces/listkeys) |
| Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/eventhub/disasterrecoveryconfigs/listkeys) |
| Microsoft.EventHub/namespaces/eventhubs/authorizationRules | [listkeys](/rest/api/eventhub/eventhubs/listkeys) |
| Microsoft.ImportExport/jobs | [listBitLockerKeys](/rest/api/storageimportexport/bitlockerkeys/list) |
| Microsoft.LabServices/users | [ListEnvironments](/rest/api/labservices/globalusers/listenvironments) |
| Microsoft.LabServices/users | [ListLabs](/rest/api/labservices/globalusers/listlabs) |
| Microsoft.Logic/integrationAccounts/agreements | [listContentCallbackUrl](/rest/api/logic/agreements/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/assemblies | [listContentCallbackUrl](/rest/api/logic/integrationaccountassemblies/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listCallbackUrl](/rest/api/logic/integrationaccounts/getcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listKeyVaultKeys](/rest/api/logic/integrationaccounts/listkeyvaultkeys) |
| Microsoft.Logic/integrationAccounts/maps | [listContentCallbackUrl](/rest/api/logic/maps/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/partners | [listContentCallbackUrl](/rest/api/logic/partners/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/schemas | [listContentCallbackUrl](/rest/api/logic/schemas/listcontentcallbackurl) |
| Microsoft.Logic/workflows | [listCallbackUrl](/rest/api/logic/workflows/listcallbackurl) |
| Microsoft.Logic/workflows | [listSwagger](/rest/api/logic/workflows/listswagger) |
| Microsoft.Logic/workflows/triggers | [listCallbackUrl](/rest/api/logic/workflowtriggers/listcallbackurl) |
| Microsoft.Logic/workflows/versions/triggers | [listCallbackUrl](/rest/api/logic/workflowversions/listcallbackurl) |
| Microsoft.MachineLearning/webServices | [listkeys](/rest/api/machinelearning/webservices/listkeys) |
| Microsoft.MachineLearning/Workspaces | listworkspacekeys |
| Microsoft.MachineLearningServices/workspaces/computes | listKeys |
| Microsoft.MachineLearningServices/workspaces | listKeys |
| Microsoft.Maps/accounts | [listKeys](/rest/api/maps-management/accounts/listkeys) |
| Microsoft.Media/mediaservices/assets | [listContainerSas](/rest/api/media/assets/listcontainersas) |
| Microsoft.Media/mediaservices/assets | [listStreamingLocators](/rest/api/media/assets/liststreaminglocators) |
| Microsoft.Media/mediaservices/streamingLocators | [listContentKeys](/rest/api/media/streaminglocators/listcontentkeys) |
| Microsoft.Media/mediaservices/streamingLocators | [listPaths](/rest/api/media/streaminglocators/listpaths) |
| Microsoft.Network/applicationSecurityGroups | listIpConfigurations |
| Microsoft.NotificationHubs/Namespaces/authorizationRules | [listkeys](/rest/api/notificationhubs/namespaces/listkeys) |
| Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules | [listkeys](/rest/api/notificationhubs/notificationhubs/listkeys) |
| Microsoft.OperationalInsights/workspaces | [listKeys](/rest/api/loganalytics/workspaces%202015-03-20/listkeys) |
| Microsoft.Relay/namespaces/authorizationRules | [listkeys](/rest/api/relay/namespaces/listkeys) |
| Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules | listkeys |
| Microsoft.Relay/namespaces/HybridConnections/authorizationRules | [listkeys](/rest/api/relay/hybridconnections/listkeys) |
| Microsoft.Relay/namespaces/WcfRelays/authorizationRules | [listkeys](/rest/api/relay/wcfrelays/listkeys) |
| Microsoft.Search/searchServices | [listAdminKeys](/rest/api/searchmanagement/adminkeys/get) |
| Microsoft.Search/searchServices | [listQueryKeys](/rest/api/searchmanagement/querykeys/listbysearchservice) |
| Microsoft.ServiceBus/namespaces/authorizationRules | [listkeys](/rest/api/servicebus/namespaces/listkeys) |
| Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/servicebus/disasterrecoveryconfigs/listkeys) |
| Microsoft.ServiceBus/namespaces/queues/authorizationRules | [listkeys](/rest/api/servicebus/queues/listkeys) |
| Microsoft.ServiceBus/namespaces/topics/authorizationRules | [listkeys](/rest/api/servicebus/topics/listkeys) |
| Microsoft.SignalRService/SignalR | [listkeys](/rest/api/signalr/signalr/listkeys) |
| Microsoft.Storage/storageAccounts | [listAccountSas](/rest/api/storagerp/storageaccounts/listaccountsas) |
| Microsoft.Storage/storageAccounts | [listkeys](/rest/api/storagerp/storageaccounts/listkeys) |
| Microsoft.Storage/storageAccounts | [listServiceSas](/rest/api/storagerp/storageaccounts/listservicesas) |
| Microsoft.StorSimple/managers/devices | [listFailoverSets](/rest/api/storsimple/devices/listfailoversets) |
| Microsoft.StorSimple/managers/devices | [listFailoverTargets](/rest/api/storsimple/devices/listfailovertargets) |
| Microsoft.StorSimple/managers | [listActivationKey](/rest/api/storsimple/managers/getactivationkey) |
| Microsoft.StorSimple/managers | [listPublicEncryptionKey](/rest/api/storsimple/managers/getpublicencryptionkey) |
| Microsoft.Web/connectionGateways | ListStatus |
| microsoft.web/connections | listconsentlinks |
| Microsoft.Web/customApis | listWsdlInterfaces |
| microsoft.web/locations | listwsdlinterfaces |
| microsoft.web/apimanagementaccounts/apis/connections | listconnectionkeys |
| microsoft.web/apimanagementaccounts/apis/connections | listsecrets |
| microsoft.web/sites/functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecrets) |
| microsoft.web/sites/hybridconnectionnamespaces/relays | [listkeys](/rest/api/appservice/webapps/listhybridconnectionkeys) |
| microsoft.web/sites | [listsyncfunctiontriggerstatus](/rest/api/appservice/webapps/listsyncfunctiontriggers) |
| microsoft.web/sites/slots/functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecretsslot) |

Para determinar qué tipos de recursos tienen una operación de lista, dispone de las siguientes opciones:

* Vea las [operaciones de API de REST](/rest/api/) para un proveedor de recursos y busque operaciones List. Por ejemplo, las cuentas de almacenamiento tienen una [operación listKeys](/rest/api/storagerp/storageaccounts).
* Utilice el cmdlet de PowerShell [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation). En el ejemplo siguiente se obtienen todas las operaciones List para cuentas de almacenamiento:

  ```powershell
  Get-AzProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Use el siguiente comando de la CLI de Azure para filtrar solo las operaciones de la lista:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

### <a name="return-value"></a>Valor devuelto

El objeto devuelto varía según la función de la lista que use. Por ejemplo, listKeys para una cuenta de almacenamiento devuelve el siguiente formato:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Otras funciones List tienen diferentes formatos de retorno. Para ver el formato de una función, inclúyalo en la sección de resultados tal y como se muestra en la plantilla de ejemplo.

### <a name="remarks"></a>Comentarios

Especifique el recursos con el nombre del recurso o la [función resourceId](#resourceid). Al usar una función de la lista en la misma plantilla que implementa el recurso al que se hace referencia, use el nombre del recurso.

Si usa una función **list** con un recurso que se implementa de forma condicional, se puede evaluar la función incluso si el recurso no está implementado. Se genera un error si la función **list** hace referencia a un recurso que no existe. Use la función **if** para asegurarse de que la función se evalúa solo cuando se implementa el recurso. Consulte la función [if function](resource-group-template-functions-logical.md#if) para una plantilla de ejemplo que use if y list con un recurso implementado de forma condicional.

### <a name="example"></a>Ejemplo

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) siguiente se muestra cómo se devuelven las claves principal y secundaria de una cuenta de almacenamiento en la sección de salidas. También devuelve un token de SAS para la cuenta de almacenamiento. 

Para obtener el token de SAS, pase un objeto para la hora de expiración. Dicha hora debe ser futura. Este ejemplo está pensado para mostrar cómo usar las funciones de la lista. Normalmente, se podría usar el token SAS en un valor de recurso en lugar de devolverse como un valor de salida. Los valores de salida se almacenan en el historial de implementación y no son seguros.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagename": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus"
        },
        "accountSasProperties": {
            "type": "object",
            "defaultValue": {
                "signedServices": "b",
                "signedPermission": "r",
                "signedExpiry": "2018-08-20T11:00:00Z",
                "signedResourceTypes": "s"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('storagename')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": false,
                "accessTier": "Hot",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "keys": {
            "type": "object",
            "value": "[listKeys(parameters('storagename'), '2018-02-01')]"
        },
        "accountSAS": {
            "type": "object",
            "value": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('accountSasProperties'))]"
        }
    }
}
```

## <a name="providers"></a>providers

`providers(providerNamespace, [resourceType])`

Devuelve información acerca de un proveedor de recursos y sus tipos de recursos admitidos. Si no proporciona un tipo de recurso, la función devuelve todos los tipos admitidos para el proveedor de recursos.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | type | DESCRIPCIÓN |
|:--- |:--- |:--- |:--- |
| providerNamespace |Sí |string |Espacio de nombres del proveedor |
| resourceType |Sin |string |El tipo de recurso en el espacio de nombres especificado. |

### <a name="return-value"></a>Valor devuelto

Se devuelve cada tipo admitido en el formato siguiente: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

No se garantiza el orden de matriz de los valores devueltos.

### <a name="example"></a>Ejemplo

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) siguiente se muestra cómo utilizar la función de proveedor:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "providerNamespace": {
            "type": "string"
        },
        "resourceType": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers(parameters('providerNamespace'), parameters('resourceType'))]",
            "type" : "object"
        }
    }
}
```

Para el proveedor de recursos **Microsoft.Web** y el tipo de recurso **sites**, el ejemplo anterior devuelve un objeto con el formato siguiente:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

## <a name="reference"></a>reference

`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Devuelve un objeto que representa el estado de tiempo de ejecución de un recurso.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | type | DESCRIPCIÓN |
|:--- |:--- |:--- |:--- |
| resourceName o resourceIdentifier |Sí |string |Nombre o identificador único de un recurso. Al hacer referencia a un recurso en la plantilla actual, proporcione solo el nombre del recurso como parámetro. Al hacer referencia a un recurso implementado previamente, especifique el identificador del recurso. |
| apiVersion |Sin |string |Versión de la API del recurso especificado. Incluya este parámetro cuando el recurso no esté aprovisionado en la misma plantilla. Por lo general, en el formato, **aaaa-mm-dd**. Para conocer las versiones válidas de la API para su recurso, consulte la [referencia de la plantilla](/azure/templates/). |
| 'Full' |Sin |string |Valor que especifica si se devuelve el objeto de recurso completo. Si no se especifica `'Full'`, se devuelve solo el objeto de propiedades del recurso. El objeto completo incluye valores como el identificador de recurso y la ubicación. |

### <a name="return-value"></a>Valor devuelto

Cada tipo de recurso devuelve propiedades diferentes para la función de referencia. La función no devuelve un solo formato predefinido. Además, el valor devuelto es diferente en función de si se especifica el objeto completo. Para ver las propiedades de un tipo de recurso, devuelva el objeto en la sección de resultados tal como se muestra en el ejemplo.

### <a name="remarks"></a>Comentarios

La función de referencia recupera el estado del entorno de ejecución de un recurso previamente implementado o de un recurso implementado en la plantilla actual. En este artículo se muestran ejemplos de ambos escenarios.

Normalmente, se usa la función de **referencia** para devolver un valor determinado de un objeto, como el identificador URI del punto de conexión de blob o el nombre de dominio completo.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

Utilice `'Full'` cuando necesite valores de recurso que no forman parte del esquema de propiedades. Por ejemplo, para establecer directivas de acceso del almacén de claves, obtenga las propiedades de identidad para una máquina virtual.

```json
{
  "type": "Microsoft.KeyVault/vaults",
  "properties": {
    "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
    "accessPolicies": [
      {
        "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
        "objectId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.principalId]",
        "permissions": {
          "keys": [
            "all"
          ],
          "secrets": [
            "all"
          ]
        }
      }
    ],
    ...
```

### <a name="valid-uses"></a>Usos válidos

La función de referencia solo se puede utilizar en las propiedades de una definición de recursos y en la sección de salidas de una plantilla o implementación. Cuando se usa con la [iteración de la propiedad](resource-group-create-multiple.md#property-iteration), puede usar la función reference para `input` porque la expresión se asigna a la propiedad de recurso. No se puede utilizar con `count` porque debe determinarse el recuento antes de resolver la función reference.

No se puede usar la función reference en las salidas de una [plantilla anidada](resource-group-linked-templates.md#nested-template) para devolver un recurso que ha implementado en la plantilla anidada. En su lugar, use una [plantilla vinculada](resource-group-linked-templates.md#external-template-and-external-parameters).

Si usa una función **reference** con un recurso que se implementa de forma condicional, se puede evaluar la función incluso si el recurso no está implementado.  Se genera un error si la función **reference** a un recurso que no existe. Use la función **if** para asegurarse de que la función se evalúa solo cuando se implementa el recurso. Consulte la función [if](resource-group-template-functions-logical.md#if) para una plantilla de ejemplo que use if y reference con un recurso implementado de forma condicional.

### <a name="implicit-dependency"></a>Dependencia implícita

Mediante el uso de la función de referencia, se declara implícitamente que un recurso depende de otro recurso si el recurso al que se hace referencia se aprovisiona en la misma plantilla y se hace referencia a él por su nombre (y no por el identificador del recurso). No tiene que usar también la propiedad dependsOn. La función no se evalúa hasta que el recurso al que se hace referencia haya completado la implementación.

### <a name="resource-name-or-identifier"></a>Nombre de recurso o identificador

Al hacer referencia a un recurso que esté implementado en la misma plantilla, especifique el nombre del recurso.

```json
"value": "[reference(parameters('storageAccountName'))]"
```

Al hacer referencia a un recurso que no esté implementado en la misma plantilla, especifique el identificador del recurso.

```json
"value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]"
```

Para evitar la ambigüedad acerca del recurso al que hace referencia, puede proporcionar un nombre de recurso completo.

```json
"value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')))]"
```

Al construir una referencia completa a un recurso, el orden para combinar los segmentos a partir del tipo y el nombre no es simplemente una concatenación de los dos. En su lugar, después del espacio de nombres, use una secuencia de pares *tipo/nombre* de menos a más específico:

**{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]**

Por ejemplo:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` es correcto `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` no es correcto

### <a name="example"></a>Ejemplo

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) siguiente se implementa un recurso y se hace referencia a ese recurso.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      },
      "fullReferenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'), '2016-12-01', 'Full')]"
      }
    }
}
``` 

El ejemplo anterior devuelve los dos objetos. El objeto de propiedades está en el formato siguiente:

```json
{
   "creationTime": "2017-10-09T18:55:40.5863736Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

El objeto completo está en el formato siguiente:

```json
{
  "apiVersion":"2016-12-01",
  "location":"southcentralus",
  "sku": {
    "name":"Standard_LRS",
    "tier":"Standard"
  },
  "tags":{},
  "kind":"Storage",
  "properties": {
    "creationTime":"2017-10-09T18:55:40.5863736Z",
    "primaryEndpoints": {
      "blob":"https://examplestorage.blob.core.windows.net/",
      "file":"https://examplestorage.file.core.windows.net/",
      "queue":"https://examplestorage.queue.core.windows.net/",
      "table":"https://examplestorage.table.core.windows.net/"
    },
    "primaryLocation":"southcentralus",
    "provisioningState":"Succeeded",
    "statusOfPrimary":"available",
    "supportsHttpsTrafficOnly":false
  },
  "subscriptionId":"<subscription-id>",
  "resourceGroupName":"functionexamplegroup",
  "resourceId":"Microsoft.Storage/storageAccounts/examplestorage",
  "referenceApiVersion":"2016-12-01",
  "condition":true,
  "isConditionTrue":true,
  "isTemplateResource":false,
  "isAction":false,
  "provisioningOperation":"Read"
}
```

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) siguiente se hace referencia a una cuenta de almacenamiento que no se implementa en esta plantilla. La cuenta de almacenamiento ya existe dentro de la misma suscripción.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageResourceGroup": {
            "type": "string"
        },
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]",
            "type": "object"
        }
    }
}
```

## <a name="resourcegroup"></a>resourceGroup

`resourceGroup()`

Devuelve un objeto que representa el grupo de recursos actual. 

### <a name="return-value"></a>Valor devuelto

El objeto devuelto está en el formato siguiente:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "type":"Microsoft.Resources/resourceGroups",
  "location": "{resourceGroupLocation}",
  "managedBy": "{identifier-of-managing-resource}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

La propiedad **managedBy** solo se devuelve para los grupos de recursos que contienen recursos administrados por otro servicio. En los casos de Managed Applications, Databricks y AKS, el valor de la propiedad es el identificador del recurso que realiza la administración.

### <a name="remarks"></a>Comentarios

La función `resourceGroup()` no se puede usar en una plantilla que está [implementada en el nivel de suscripción](deploy-to-subscription.md). Solo puede usarse en las plantillas que se implementan en un grupo de recursos.

Un uso común de la función resourceGroup es crear recursos en la misma ubicación que el grupo de recursos. En el ejemplo siguiente se utiliza la ubicación del grupo de recursos para asignar la ubicación de un sitio web.

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

También puede usar la función resourceGroup para aplicar etiquetas del grupo de recursos a un recurso. Para más información, consulte [Aplicación de etiquetas de un grupo de recursos](resource-group-using-tags.md#apply-tags-from-resource-group).

### <a name="example"></a>Ejemplo

La [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) siguiente devuelve las propiedades del grupo de recursos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "resourceGroupOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

El ejemplo anterior devuelve un objeto en el formato siguiente:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "type":"Microsoft.Resources/resourceGroups",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

## <a name="resourceid"></a>resourceId

`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2], ...)`

Devuelve el identificador único de un recurso. Utilice esta función cuando el nombre del recurso sea ambiguo o no esté aprovisionado dentro de la misma plantilla. 

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | type | DESCRIPCIÓN |
|:--- |:--- |:--- |:--- |
| subscriptionId |Sin |Cadena (en formato de GUID) |El valor predeterminado es la suscripción actual. Especifique este valor cuando necesite recuperar un recurso en otra suscripción. |
| resourceGroupName |Sin |string |El valor predeterminado es el grupo de recursos actual. Especifique este valor cuando necesite recuperar un recurso en otro grupo de recursos. |
| resourceType |Sí |string |Tipo de recurso, incluido el espacio de nombres del proveedor de recursos. |
| resourceName1 |Sí |string |Nombre del recurso. |
| resourceName2 |Sin |string |Segmento con el nombre del siguiente segmento, si es necesario. |

Siga agregando nombres de recursos como parámetros cuando el tipo de recurso incluya más segmentos.

### <a name="return-value"></a>Valor devuelto

El identificador se devuelve con el formato siguiente:

**/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}**


### <a name="remarks"></a>Comentarios

El número de parámetros que proporcione varía en función de si el recurso es primario o secundario, y de si está en la misma suscripción o grupo de recursos.

Para obtener el Id. de un recurso primario de la misma suscripción y grupo de recursos, proporcione el tipo y el nombre del recurso.

```json
"[resourceId('Microsoft.ServiceBus/namespaces', 'namespace1')]"
```

Para obtener el Id. de un recurso secundario, preste atención al número de segmentos en el tipo de recurso. Proporcione un nombre de recurso para cada segmento del tipo de recurso. El nombre del segmento corresponde al recurso que existe para esa parte de la jerarquía.

```json
"[resourceId('Microsoft.ServiceBus/namespaces/queues/authorizationRules', 'namespace1', 'queue1', 'auth1')]"
```

Para obtener el Id. de un recurso de la misma suscripción en un grupo de recursos distinto, proporcione el nombre del grupo de recursos.

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts', 'examplestorage')]"
```

Para obtener el Id. de un recurso con una suscripción y grupo de recursos distintos, proporcione el Id. de suscripción y el nombre del grupo de recursos.

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Cuando se usa con una [implementación de nivel de suscripción](deploy-to-subscription.md), la función `resourceId()` solo puede recuperar el identificador de los recursos implementados en ese nivel. Por ejemplo, puede obtener el identificador de una definición de directiva o de una definición de función, pero no el identificador de una cuenta de almacenamiento. Para las implementaciones en un grupo de recursos, ocurre lo contrario. No puede obtener el identificador de recurso de los recursos implementados a nivel de suscripción.

Para obtener el identificador de un recurso de nivel de suscripción cuando se implementa en el ámbito de la suscripción, utilice:

```json
"[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
```

A menudo, necesitará utilizar esta función cuando se usa una cuenta de almacenamiento o red virtual en un grupo de recursos alternativo. En el ejemplo siguiente se muestra cómo un recurso de un grupo de recursos externos se puede utilizar fácilmente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "subnet1Ref": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a>Ejemplo

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) siguiente se devuelve el identificador de recursos de la cuenta de almacenamiento en el grupo de recursos:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('11111111-1111-1111-1111-111111111111', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

La salida del ejemplo anterior con el valor predeterminado es:

| NOMBRE | type | Valor |
| ---- | ---- | ----- |
| sameRGOutput | Cadena | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Cadena | /subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Cadena | /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Cadena | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName |

## <a name="subscription"></a>subscription

`subscription()`

Devuelve detalles sobre la suscripción para la implementación actual. 

### <a name="return-value"></a>Valor devuelto

La función devuelve el siguiente formato:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Ejemplo

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) siguiente se muestra la función de suscripción a la que se llama en la sección de salidas. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una descripción de las secciones de una plantilla de Azure Resource Manager, vea [Creación de plantillas de Azure Resource Manager](resource-group-authoring-templates.md).
* Para combinar varias plantillas, vea [Uso de plantillas vinculadas con Azure Resource Manager](resource-group-linked-templates.md).
* Para iterar una cantidad de veces específica al crear un tipo de recurso, vea [Creación de varias instancias de recursos en el Administrador de recursos de Azure](resource-group-create-multiple.md).
* Para saber cómo implementar la plantilla que creó, consulte [Implementación de una aplicación con una plantilla de Azure Resource Manager](resource-group-template-deploy.md).

