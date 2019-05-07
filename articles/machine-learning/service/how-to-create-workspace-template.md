---
title: Usar una plantilla de Azure Resource Manager para crear un área de trabajo
titleSuffix: Azure Machine Learning service
description: Aprenda a usar una plantilla de Azure Resource Manager para crear una nueva área de trabajo de Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 04/16/2019
ms.custom: seoapril2019
ms.openlocfilehash: abe497ed96515e8194fb2ddefd8e7f4cb9908758
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205121"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning-service"></a>Usar una plantilla de Azure Resource Manager para crear un área de trabajo para el servicio Azure Machine Learning

En este artículo aprenderá varias formas de crear un área de trabajo de Azure Machine Learning Service mediante plantillas de Azure Resource Manager. Una plantilla de Resource Manager facilita la creación de recursos en una única operación coordinada. Una plantilla es un documento JSON que define los recursos que son necesarios para una implementación. También puede especificar los parámetros de implementación. Los parámetros se emplean para proporcionar valores de entrada cuando se usa la plantilla.

Para obtener más información, consulte [Implementación de una aplicación con la plantilla de Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy.md).

## <a name="prerequisites"></a>Requisitos previos

* Una **suscripción de Azure**. Si no tiene una ya, pruebe la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Para usar una plantilla desde una CLI, necesita [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) o la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="resource-manager-template"></a>Plantilla de Resource Manager

La siguiente plantilla de Resource Manager puede usarse para crear un área de trabajo del servicio de Azure Machine Learning y recursos de Azure asociados:

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

Esta plantilla crea los siguientes servicios de Azure:

* Grupo de recursos de Azure
* Cuenta de Azure Storage
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Área de trabajo de Azure Machine Learning

El grupo de recursos es el contenedor que contiene los servicios. El área de trabajo de Azure Machine Learning requiere los diversos servicios.

La plantilla de ejemplo tiene dos parámetros:

* La **ubicación** en la que se creará el grupo de recursos y de servicios.

    La plantilla utilizará la ubicación que seleccione para la mayoría de los recursos. La excepción es el servicio Application Insights que no está disponible en todas las ubicaciones en las que están los demás servicios. Si selecciona una ubicación donde no está disponible, el servicio se creará en la ubicación Centro y Sur de EE. UU.

* El **nombre del área de trabajo**, que es el nombre descriptivo del área de trabajo de Azure Machine Learning.

    Los nombres de los demás servicios se generan aleatoriamente.

Para más información sobre las plantillas, consulte los siguientes artículos:

* [Creación de plantillas del Administrador de recursos de Azure](../../azure-resource-manager/resource-group-authoring-templates.md)
* [Implementación de una aplicación con las plantillas de Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy.md)
* [Tipos de recurso de Microsoft.MachineLearningServices](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

1. Siga los pasos que se indican en [Implementación de recursos desde plantilla personalizada](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template). Cuando llegue a la pantalla __Editar plantilla__, pegue la plantilla desde este documento.
1. Seleccione __Guardar__ para usar la plantilla. Proporcione la siguiente información y acepte los términos y condiciones que aparecen:

   * Suscripción: Seleccione la suscripción de Azure que va a usar para estos recursos.
   * Grupo de recursos: Seleccione o cree un grupo de recursos que contenga los servicios.
   * Nombre del área de trabajo: El nombre que se usará para el área de trabajo de Azure Machine Learning que se va a crear. El nombre del área de trabajo debe tener entre 3 y 33 caracteres. Solo puede contener caracteres alfanuméricos y "-".
   * Ubicación: Seleccione la ubicación en la que se crearán los recursos.

     ![Los parámetros de la plantilla en Azure Portal](media/how-to-create-workspace-template/template-parameters.png)

Para más información, vea [Implementación de recursos desde plantilla personalizada](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Uso de Azure PowerShell

En este ejemplo se da por supuesto que ha guardado la plantilla en un archivo denominado `azuredeploy.json` en el directorio actual:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace"
```

Para más información, vea [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md) e [Implementar la plantilla de Resource Manager privada con el token de SAS y Azure PowerShell](../../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="use-azure-cli"></a>Uso de CLI de Azure

En este ejemplo se da por supuesto que ha guardado la plantilla en un archivo denominado `azuredeploy.json` en el directorio actual:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace
```

Para más información, vea [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../../azure-resource-manager/resource-group-template-deploy-cli.md) e [Implementar la plantilla de Resource Manager privada con el token de SAS y la CLI de Azure](../../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de recursos con las plantillas de Resource Manager y la API REST de Resource Manager](../../azure-resource-manager/resource-group-template-deploy-rest.md).
* [Creación e implementación de grupos de recursos de Azure mediante Visual Studio](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
