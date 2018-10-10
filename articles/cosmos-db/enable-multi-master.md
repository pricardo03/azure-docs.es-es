---
title: Habilitación de la arquitectura multimaestro para las cuentas de Azure Cosmos DB
description: En este artículo se describe cómo habilitar la compatibilidad con la arquitectura multimaestro durante la creación de una cuenta de Azure Cosmos DB con Azure Portal, PowerShell, la CLI o una plantilla de Azure Resource Manager.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 71579e28f389d91498ef5f37c5d43dc9d5140234
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963226"
---
# <a name="enable-multi-master-for-azure-cosmos-db-accounts"></a>Habilitación de la arquitectura multimaestro para las cuentas de Azure Cosmos DB

La compatibilidad con la arquitectura multimaestro se habilita durante la creación de una cuenta de Azure Cosmos DB. Se puede crear una cuenta de Azure Cosmos DB mediante Azure Portal, PowerShell, la CLI o una plantilla de Azure Resource Manager.

> [!IMPORTANT]
> Actualmente, la compatibilidad con la arquitectura multimaestro solo se puede habilitar para las nuevas cuentas de Azure Cosmos DB. Las cuentas de Azure Cosmos DB existentes no pueden usar la característica. Estamos trabajando para ofrecer compatibilidad para las cuentas existentes y se anunciará esta compatibilidad cuando esté disponible.

Después de crear una cuenta de Azure Cosmos DB con compatibilidad con la arquitectura multimaestro, puede crear bases de datos, contenedores, cargar documentos y asignar directivas de resolución de conflictos. Para información sobre la resolución de conflictos en la arquitectura multimaestro y ejemplos de código, consulte el artículo [Ejemplos de código de la arquitectura multimaestro](multi-master-conflict-resolution.md#code-samples).

## <a name="enable-multi-master-using-azure-portal"></a>Habilitación de la arquitectura multimaestro con Azure Portal

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

2. Seleccione **Crear un recurso > Bases de datos > Azure Cosmos DB**.

3. En el panel **Nueva cuenta**, escriba la siguiente configuración para una nueva cuenta de Azure Cosmos DB:

   |**Configuración**  |**Valor sugerido** |**Descripción**|
   |---------|---------|---------|
   |Subscription   | {Su suscripción}  |Seleccionen la suscripción de Azure que quiere usar para esta cuenta de Azure Cosmos DB.  |
   |Grupo de recursos  |   {Nombre del grupo de recursos}    |  Seleccione un grupo de recursos existente o seleccione  **Crear nuevo** y, a continuación, escriba un nuevo nombre de grupo de recursos para la cuenta. |
   |Nombre de cuenta | {Nombre de cuenta}   |  Escriba un nombre único para identificar la cuenta de Azure Cosmos DB.        |
   |API  |   Cualquiera   |  Seleccione un tipo de API.   |
   |Ubicación  | Seleccionar cualquier región   | Seleccione la ubicación geográfica en la que se va a hospedar la cuenta de Azure Cosmos DB. Puede elegir cualquier región, puesto que esta cuenta estará en varias regiones.  |
   |Habilitar redundancia geográfica   |  Habilitar  |  Seleccione esta opción para poder habilitar a continuación la arquitectura multimaestro.   |
   |Habilitar Multi Master | Habilitar  | Seleccione esta opción para habilitar la arquitectura multimaestro para esta cuenta. |


## <a name="using-multi-master-in-sdks"></a>Uso de la arquitectura multimaestro en los SDK

Con una cuenta habilitada para la arquitectura multimaestro, en las aplicaciones, puede aprovechar las ventajas de la arquitectura multimaestro utilizando el objeto ConnectionPolicy tal como se muestra a continuación.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
{
   ConnectionMode = ConnectionMode.Direct,
   ConnectionProtocol = Protocol.Tcp,
   UseMultipleWriteLocations = true,
};
policy.PreferredLocations.Add("West US");
policy.PreferredLocations.Add("North Europe");
policy.PreferredLocations.Add("Southeast Asia");
```

## <a name="enable-multi-master-using-powershell"></a>Habilitación de la arquitectura multimaestro con PowerShell

También puede crear una cuenta de Cosmos DB habilitada para la arquitectura multimaestro estableciendo el parámetro `enableMultipleWriteLocations` en "true". Para crear una cuenta de Azure Cosmos DB con la arquitectura multimaestro habilitada, abra una ventana de PowerShell y ejecute el siguiente script:

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = "<ip-range-filter>"

$consistencyPolicy = @{"defaultConsistencyLevel"="Session";
                       "maxIntervalInSeconds"= "10";
                       "maxStalenessPrefix"="200"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

## <a name="enable-multi-master-using-cli"></a>Habilitación de la arquitectura multimaestro con la CLI

Puede habilitar la arquitectura multimaestro estableciendo el parámetro enable-multiple-write-locations en "true". Para crear una cuenta de Azure Cosmos DB con la arquitectura multimaestro habilitada, abra la CLI de Azure o Cloud Shell y ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --default-consistency-level "Session" \
   --enable-automatic-failover "true" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="enable-multi-master-using-resource-manager-template"></a>Habilitación de la arquitectura multimaestro con una plantilla de Resource Manager

El siguiente código JSON es una plantilla de Resource Manager de ejemplo que puede usar para implementar una cuenta de Azure Cosmos DB. Para obtener información sobre el formato y la sintaxis de una plantilla de Resource Manager, consulte la documentación de [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). El parámetro principal que debe observar en esta plantilla es "enableMultipleWriteLocations": true.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre cómo habilitar la compatibilidad con la arquitectura multimaestro para las cuentas de Azure Cosmos DB. A continuación, eche un vistazo a los siguientes recursos:

* [Uso de la arquitectura multimaestro con bases de datos NoSQL de código abierto](multi-master-oss-nosql.md)

* [Descripción de resolución de conflictos en Azure Cosmos DB](multi-master-conflict-resolution.md)
