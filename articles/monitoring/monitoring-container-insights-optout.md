---
title: Cómo detener la supervisión del clúster de Azure Kubernetes Service | Microsoft Docs
description: En este artículo se describe cómo puede interrumpir la supervisión del clúster de Azure AKS con Azure Monitor para contenedores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/13/2018
ms.author: magoedte
ms.openlocfilehash: 2b989fbebe237e4e3746ef2f237193587173dfe4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963413"
---
# <a name="how-to-stop-monitoring-your-azure-kubernetes-service-aks-azure-monitor-for-containers"></a>Cómo detener la supervisión de Azure Kubernetes Service (AKS) con Azure Monitor para contenedores

Si después de habilitar la supervisión del clúster de AKS decide que ya no desea supervisarlo, puede *descartarla* mediante las plantillas de Azure Resource Manager que proporcionan con el cmdlet **New-AzureRmResourceGroupDeployment** de PowerShell o la CLI de Azure. Una plantilla JSON especifica la configuración que se *descarta*. La otro contiene los valores de parámetro que se configuran para especificar el grupo de recursos y el identificador de recursos de AKS en que se implementa el clúster. 

Si no conoce el concepto de implementación de recursos mediante una plantilla, consulte:
* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/resource-group-template-deploy-cli.md)

Si decide usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecuta la versión 2.0.27 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Si necesita instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="create-template"></a>Creación de una plantilla

1. Copie y pegue la siguiente sintaxis JSON en el archivo:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
           "type": "string",
           "metadata": {
             "description": "AKS Cluster Resource ID"
           }
       },
      "aksResourceLocation": {
        "type": "string",
        "metadata": {
           "description": "Location of the AKS resource e.g. \"East US\""
         }
       }
    },
    "resources": [
      {
        "name": "[split(parameters('aksResourceId'),'/')[8]]",
        "type": "Microsoft.ContainerService/managedClusters",
        "location": "[parameters('aksResourceLocation')]",
        "apiVersion": "2018-03-31",
        "properties": {
          "mode": "Incremental",
          "id": "[parameters('aksResourceId')]",
          "addonProfiles": {
            "omsagent": {
              "enabled": false,
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Guarde este archivo como **OptOutTemplate.json** en una carpeta local.
3. Pegue la siguiente sintaxis de JSON en el archivo:

    ```json
    {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "aksResourceId": {
         "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
      },
      "aksResourceLocation": {
        "value": "<aksClusterRegion>"
        }
      }
    }
    ```

4. Edite los valores de **aksResourceId** y **aksResourceLocation** usando los valores del clúster de AKS, que puede encontrar en la página **Propiedades** del clúster seleccionado.

    ![Página de propiedades del contenedor](./media/monitoring-container-health/container-properties-page.png)

    Mientras está en la página de **propiedades**, copie también el **identificador de recurso del área de trabajo**. Este valor es necesario si decide que desea eliminar el área de trabajo de Log Analytics más adelante. La eliminación del área de trabajo de Log Analytics no se realiza como parte de este proceso. 

5. Guarde este archivo como **OptOutParam.json** en una carpeta local.
6. Está listo para implementar esta plantilla. 

## <a name="remove-the-solution-using-azure-cli"></a>Eliminación de la solución mediante la CLI de Azure
Ejecute el siguiente comando con la CLI de Azure en Linux para eliminar la solución y borrar la configuración en el clúster de AKS.

```azurecli
az login   
az account set --subscription "Subscription Name" 
az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

El cambio de configuración puede tardar unos minutos en completarse. Cuando termine, verá un mensaje similar al siguiente que incluye el resultado:

```azurecli
ProvisioningState       : Succeeded
```

## <a name="remove-the-solution-using-powershell"></a>Eliminación de la solución mediante PowerShell

Ejecute los siguientes comandos de PowerShell en la carpeta que contiene la plantilla para eliminar la solución y borrar la configuración del clúster de AKS.    

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
```

El cambio de configuración puede tardar unos minutos en completarse. Cuando termine, verá un mensaje similar al siguiente que incluye el resultado:

```powershell
ProvisioningState       : Succeeded
```

Si el área de trabajo se creó solamente para admitir la supervisión del clúster y ya no es necesario, tiene que eliminarlo manualmente. Si no está familiarizado con la eliminación de un área de trabajo, consulte [Eliminación de una área de trabajo de Azure Log Analytics con Azure Portal](../log-analytics/log-analytics-manage-del-workspace.md). No se olvide del **identificador de recurso del área de trabajo** que copió anteriormente en el paso 4, lo necesitará. 

