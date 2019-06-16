---
title: Creación del espacio de nombres Messaging de Service Bus con una plantilla de Azure Resource Manager | Microsoft Docs
description: Utilice una plantilla de Azure Resource Manager para crear un espacio de nombres Messaging de Service Bus.
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: spelluru
ms.openlocfilehash: 4471c9d5b6c09bcf4d9100cccfa725f36cf9a3f8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66111238"
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Creación de un espacio de nombres de Service Bus mediante una plantilla de Azure Resource Manager
En esta guía de inicio rápido se crea una plantilla de Azure Resource Manager que genera un espacio de nombres de Service Bus del tipo **Mensajería** con una SKU de nivel **estándar**. El artículo también define los parámetros que se especifican para la ejecución de la implementación. Puede usar esta plantilla para sus propias implementaciones o personalizarla para satisfacer sus necesidades. Para más información sobre la creación de plantillas, consulte [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Creación de plantillas de Azure Resource Manager). Para ver la plantilla completa, consulte la [plantilla de espacio de nombres de Service Bus][Service Bus namespace template] en GitHub.

> [!NOTE]
> Las siguientes plantillas de Azure Resource Manager están disponibles para su descarga e implementación. 
> 
> * [Creación de un espacio de nombres de Service Bus con cola](service-bus-resource-manager-namespace-queue.md)
> * [Creación de un espacio de nombres de Service Bus con un tema y una suscripción](service-bus-resource-manager-namespace-topic.md)
> * [Creación de un espacio de nombres de Service Bus con regla de autorización y cola](service-bus-resource-manager-namespace-auth-rule.md)
> * [Create a Service Bus namespace with topic, subscription, and rule](service-bus-resource-manager-namespace-topic-with-rule.md) (Creación de un espacio de nombres de Service Bus con tema, suscripción y regla)
> 
> Para buscar las últimas plantillas, visite la galería [Plantillas de inicio rápido de Azure][Azure Quickstart Templates] y busque "Service Bus".


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="quick-deployment"></a>Implementación rápida
Para ejecutar el ejemplo sin escribir ningún JSON y ejecutar el comando de PowerShell o CLI, seleccione siguiente el botón:

[![Implementación en Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

Para crear e implementar la plantilla manualmente, recorra las siguientes secciones de este artículo.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía de inicio rápido, necesita una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

Si desea usar **Azure PowerShell** para implementar la plantilla de Resource Manager, [instale Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps).

Si desea usar la **CLI de Azure** para implementar la plantilla de Resource Manager, [instale la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="create-the-resource-manager-template-json"></a>Creación del archivo JSON de la plantilla de Resource Manager 
Cree un archivo JSON denominado **MyServiceBusNamespace.json** con el siguiente contenido: 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusSku": {
            "type": "string",
            "allowedValues": [
                "Basic",
                "Standard",
                "Premium"
            ],
            "defaultValue": "Standard",
            "metadata": {
                "description": "The messaging tier for service Bus namespace"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('serviceBusSku')]"
            }
        }
    ]
}
```

Esta plantilla crea un espacio de nombres de Service Bus estándar. Para la sintaxis y las propiedades de JSON, consulte la referencia de la plantilla de los [espacios de nombres](/azure/templates/microsoft.servicebus/namespaces).

## <a name="create-the-parameters-json"></a>Creación del archivo JSON de parámetros
En la plantilla que creó en el paso anterior hay una sección denominada `Parameters`. Defina los parámetros para los valores que variarán según el proyecto que vaya a implementar o según el entorno de destino. En esta plantilla se definen los siguientes parámetros: **serviceBusNamespaceName**, **serviceBusSku** y **location**. Para obtener más información acerca de las SKU de Service Bus, consulte las [SKU de Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) que desee crear.

Cree un archivo JSON denominado **MyServiceBusNamespace-Parameters.json** con el siguiente contenido: 

> [!NOTE] 
> Especifique un nombre para el espacio de nombres de Service Bus. 


```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serviceBusNamespaceName": {
      "value": "<Specify a name for the Service Bus namespace>"
    },
    "serviceBusSku": {
      "value": "Standard"
    },
    "location": {
        "value": "East US"
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

### <a name="deploy-resources"></a>Implementación de recursos
Para implementar los recursos con Azure PowerShell, cambie a la carpeta donde guardó los archivos JSON y ejecute los siguientes comandos:

> [!IMPORTANT]
> Especifique un nombre para el grupo de recursos de Azure como un valor para $resourceGroupName antes de ejecutar los comandos. 

1. Declare una variable para el nombre del grupo de recursos y especifique su valor. 

    ```azurepowershell
    $resourceGroupName = "<Specify a name for the Azure resource group>"
    ```
2. Cree un grupo de recursos de Azure.

    ```azurepowershell
    New-AzResourceGroup $resourceGroupName -location 'East US'
    ```
3. Implemente la plantilla de Resource Manager. Especifique los nombres de la propia implementación, el grupo de recursos, el archivo JSON para la plantilla y el archivo JSON para los parámetros.

    ```azurepowershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName $resourceGroupName -TemplateFile MyServiceBusNamespace.json -TemplateParameterFile MyServiceBusNamespace-Parameters.json
    ```

## <a name="use-azure-cli-to-deploy-the-template"></a>Uso de la CLI de Azure para implementar la plantilla

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

1. Ejecute el siguiente comandos para iniciar sesión en Azure:

    ```azurecli
    az login
    ```
2. Establezca el contexto de la suscripción actual. Reemplace `MyAzureSub` por el nombre de la suscripción de Azure que desea usar:

    ```azurecli
    az account set --subscription <Name of your Azure subscription>
    ``` 

### <a name="deploy-resources"></a>Implementación de recursos
Para implementar los recursos con la CLI de Azure, cambie a la carpeta con los archivos JSON y ejecute los siguientes comandos:

> [!IMPORTANT]
> Especifique un nombre para el grupo de recursos de Azure en el comando az group create. .

1. Cree un grupo de recursos de Azure. 
    ```azurecli
    az group create --name <YourResourceGroupName> --location eastus
    ```

2. Implemente la plantilla de Resource Manager. Especifique los nombres del grupo de recursos, la implementación, el archivo JSON para la plantilla y el archivo JSON para los parámetros.

    ```azurecli
    az group deployment create --name <Specify a name for the deployment> --resource-group <YourResourceGroupName> --template-file MyServiceBusNamespace.json --parameters @MyServiceBusNamespace-Parameters.json
    ```

## <a name="next-steps"></a>Pasos siguientes
En este artículo, se creó un espacio de nombres de Service Bus. Consulte las demás guías de inicio rápido para aprender a crear colas, temas y suscripciones, además de a utilizarlos: 

- [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
- [Introducción a las colas de Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: https://azure.microsoft.com/pricing/details/service-bus/
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
