---
title: 'Creación de un centro de eventos con un grupo de consumidores: Azure Event Hubs | Microsoft Docs'
description: Creación de un espacio de nombres de Event Hubs con un centro de eventos y un grupo de consumidores mediante plantillas de Azure Resource Manager
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/16/2018
ms.author: shvija
ms.openlocfilehash: d5dc65dc225d11a996d9b9d3c329151a17321fb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60343501"
---
# <a name="quickstart-create-an-event-hub-using-azure-resource-manager-template"></a>Inicio rápido: Creación de un centro de eventos mediante una plantilla de Azure Resource Manager
Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

En esta guía de inicio rápido, creará un centro de eventos mediante una plantilla de Azure Resource Manager. Usará una plantilla de Azure Resource Manager para crear un espacio de nombres de tipo [Event Hubs](event-hubs-what-is-event-hubs.md), con un centro de eventos y un grupo de consumidores. El artículo muestra cómo definir los recursos que se implementan y los parámetros que se especifican cuando se ejecuta la implementación. Puede usar esta plantilla para sus propias implementaciones o personalizarla para satisfacer sus necesidades. Para información sobre la creación de plantillas, consulte [Creación de plantillas de Azure Resource Manager][Authoring Azure Resource Manager templates]. Para la sintaxis y las propiedades de JSON que se usan en una plantilla, consulte [Tipos de recursos de Microsoft.EventHub](/azure/templates/microsoft.eventhub/allversions).

> [!NOTE]
> Para ver la plantilla completa, consulte la [plantilla de grupos de consumidores y un centro de eventos][Event Hub and consumer group template] en GitHub. Esta plantilla crea un grupo de consumidores además de un espacio de nombres del centro de eventos y un centro de eventos. Para buscar las plantillas más recientes, visite la galería de [Plantillas de inicio rápido de Azure][Azure Quickstart Templates] y busque Event Hubs.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para completar esta guía de inicio rápido, necesita una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

Si desea usar **Azure PowerShell** para implementar la plantilla de Resource Manager, [instale Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Si desea usar la **CLI de Azure** para implementar la plantilla de Resource Manager, [instale la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="create-the-resource-manager-template-json"></a>Creación del archivo JSON de la plantilla de Resource Manager
Cree un archivo JSON denominado MyEventHub.json con el contenido siguiente y guárdelo en una carpeta (por ejemplo: C:\EventHubsQuickstarts\ResourceManagerTemplate).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventhub-namespace-name": {
            "type": "String"
        },
        "eventhub_name": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.EventHub/namespaces",
            "sku": {
                "name": "Standard",
                "tier": "Standard",
                "capacity": 1
            },
            "name": "[parameters('eventhub-namespace-name')]",
            "apiVersion": "2017-04-01",
            "location": "East US",
            "tags": {},
            "scale": null,
            "properties": {
                "isAutoInflateEnabled": false,
                "maximumThroughputUnits": 0
            },
            "dependsOn": []
        },
        {
            "type": "Microsoft.EventHub/namespaces/eventhubs",
            "name": "[concat(parameters('eventhub-namespace-name'), '/', parameters('eventhub_name'))]",
            "apiVersion": "2017-04-01",
            "location": "East US",
            "scale": null,
            "properties": {
                "messageRetentionInDays": 7,
                "partitionCount": 1,
                "status": "Active"
            },
            "dependsOn": [
                "[resourceId('Microsoft.EventHub/namespaces', parameters('eventhub-namespace-name'))]"
            ]
        }
    ]
}
```

## <a name="create-the-parameters-json"></a>Creación del archivo JSON de parámetros
Cree un archivo JSON llamado MyEventHub-Parameters.json que contenga los parámetros para la plantilla de Azure Resource Manager. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
        "eventhub-namespace-name": {
            "value": "<specify a name for the event hub namespace>"
        },
        "eventhub_name": {
            "value": "<Specify a name for the event hub in the namespace>"
        }
  }
}
```



## <a name="use-azure-powershell-to-deploy-the-template"></a>Uso de Azure PowerShell para implementar la plantilla

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
1. Inicie Azure PowerShell

2. Ejecute el siguiente comandos para iniciar sesión en Azure:

   ```azurepowershell
   Login-AzAccount
   ```
3. Emita los comandos siguientes para establecer el contexto de la suscripción actual:

   ```azurepowershell
   Select-AzSubscription -SubscriptionName "<YourSubscriptionName>" 
   ```

### <a name="provision-resources"></a>Aprovisionamiento de recursos
Para implementar y aprovisionar los recursos con Azure PowerShell, cambie a la carpeta C:\EventHubsQuickStart\ARM\ y ejecute los siguientes comandos:

> [!IMPORTANT]
> Especifique un nombre para el grupo de recursos de Azure como un valor para $resourceGroupName antes de ejecutar los comandos. 

```azurepowershell
$resourceGroupName = "<Specify a name for the Azure resource group>"

# Create an Azure resource group
New-AzResourceGroup $resourceGroupName -location 'East US'

# Deploy the Resource Manager template. Specify the names of deployment itself, resource group, JSON file for the template, JSON file for parameters
New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName $resourceGroupName -TemplateFile MyEventHub.json -TemplateParameterFile MyEventHub-Parameters.json
```

## <a name="use-azure-cli-to-deploy-the-template"></a>Uso de la CLI de Azure para implementar la plantilla

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Los pasos siguientes no son necesarios si ejecuta comandos en Cloud Shell. Si está ejecutando la CLI localmente, realice los pasos siguientes para iniciar sesión en Azure y establecer su suscripción actual:

Ejecute el siguiente comandos para iniciar sesión en Azure:

```azurecli
az login
```

Establezca el contexto de la suscripción actual. Reemplace `MyAzureSub` por el nombre de la suscripción de Azure que desea usar:

```azurecli
az account set --subscription <Name of your Azure subscription>
``` 

### <a name="provision-resources"></a>Aprovisionamiento de recursos
Para implementar los recursos con la CLI de Azure, cambie a la carpeta C:\EventHubsQuickStart\ARM\ y ejecute los siguientes comandos:

> [!IMPORTANT]
> Especifique un nombre para el grupo de recursos de Azure en el comando az group create. .

```azurecli
# Create an Azure resource group
az group create --name <YourResourceGroupName> --location eastus

# # Deploy the Resource Manager template. Specify the names of resource group, deployment, JSON file for the template, JSON file for parameters
az group deployment create --name <Specify a name for the deployment> --resource-group <YourResourceGroupName> --template-file MyEventHub.json --parameters @MyEventHub-Parameters.json
```

Felicidades. Ha usado la plantilla de Azure Resource Manager para crear un espacio de nombres de Event Hubs y un centro de eventos dentro de ese espacio de nombres.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado el espacio de nombres de Event Hubs y ha usado aplicaciones de ejemplo para enviar eventos al centro de eventos y recibirlos de este. Para que obtener instrucciones paso a paso enviar eventos a (o) recibir eventos desde un centro de eventos, consulte el **enviar y recibir eventos** tutoriales: 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (solo enviar)](event-hubs-c-getstarted-send.md)
- [Apache Storm (sólo reecive)](event-hubs-storm-getstarted-receive.md)


[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
