---
title: Implementación de aplicaciones lógicas con plantillas de Azure Resource Manager - Azure Logic Apps
description: Implementación de aplicaciones lógicas mediante plantillas de Azure Resource Manager
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.date: 10/15/2017
ms.openlocfilehash: 21603ff213bc8babb4145209a76aee0b4150cc12
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60681768"
---
# <a name="deploy-logic-apps-with-azure-resource-manager-templates"></a>Implementación de aplicaciones lógicas mediante plantillas de Azure Resource Manager

Después de crear una plantilla de Azure Resource Manager para implementar la aplicación lógica, puede implementar la plantilla de las siguientes maneras:

* [Azure Portal](#portal)
* [Azure PowerShell](#powershell)
* [CLI de Azure](#cli)
* [API de REST del Administrador de recursos de Azure](../azure-resource-manager/resource-group-template-deploy-rest.md)
* [Azure Pipelines para Azure DevOps](#azure-pipelines)

<a name="portal"></a>

## <a name="deploy-through-azure-portal"></a>Implementación a través de Azure Portal

Para implementar automáticamente una plantilla de aplicación lógica en Azure, puede elegir el siguiente botón **Implementar en Azure**, que inicia sesión en Azure Portal y le pide información sobre la aplicación lógica. A continuación, haga los cambios necesarios en la plantilla o los parámetros de la aplicación lógica.

[![Implementación en Azure](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Por ejemplo, se le pedirá esta información después de iniciar sesión en Azure Portal:

* El nombre de la suscripción a Azure
* El grupo de recursos que desea usar
* La ubicación de la aplicación lógica
* Nombre de la aplicación lógica
* Un URI de prueba
* La aceptación de los términos y condiciones especificados

Para más información, consulte [Implementación de aplicaciones lógicas con plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy-portal.md).

## <a name="authorize-oauth-connections"></a>Autorización de conexiones de OAuth

Después de la implementación, la aplicación lógica funciona de un extremo a otro con parámetros válidos. Sin embargo, sigue siendo necesario autorizar las conexiones de OAuth para generar un token de acceso válido. En el caso de las implementaciones automatizadas, puede utilizar un script que admite cada conexión de OAuth, como este [script de ejemplo del proyecto LogicAppConnectionAuth de GitHub](https://github.com/logicappsio/LogicAppConnectionAuth). También puede autorizar las conexiones de OAuth a través de Azure Portal o en Visual Studio abriendo la aplicación lógica en el Diseñador de Logic Apps.

<a name="powershell"></a>

## <a name="deploy-with-azure-powershell"></a>Implementación con Azure PowerShell

Para implementar en un determinado *grupo de recursos de Azure*, use este comando:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <Azure-resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json 
```

Para implementar en una suscripción de Azure determinada, use este comando:

```powershell
New-AzDeployment -Location <location> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json 
```

* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)
* [`New-AzResourceGroupDeployment`](/powershell/module/az.resources/new-azresourcegroupdeployment)
* [`New-AzDeployment`](/powershell/module/az.resources/new-azdeployment)

<a name="cli"></a>

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

Para implementar en un determinado *grupo de recursos de Azure*, use este comando:

```azurecli
az group deployment create -g <Azure-resource-group-name> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Para implementar en una suscripción de Azure determinada, use este comando:

```azurecli
az deployment create --location <location> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Para más información, consulte los temas siguientes: 

* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/resource-group-template-deploy-cli.md) 
* [`az group deployment create`](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)
* [`az deployment create`](https://docs.microsoft.com/cli/azure/deployment?view=azure-cli-latest#az-deployment-create)

<a name="azure-pipelines"></a>

## <a name="deploy-with-azure-devops"></a>Implementación con Azure DevOps

Para implementar las plantillas de aplicación lógica y administrar entornos, los equipos suelen usan una herramienta como [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines) en [Azure DevOps](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops-services). Azure Pipelines proporciona una [tarea Implementación de un grupo de recursos de Azure](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzureResourceGroupDeploymentV2), que se puede agregar a cualquier canalización de la compilación o versión.
Para que la autorización implemente y genere la canalización de versión, también necesita una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md) de Azure Active Directory. Obtenga más información sobre [el uso de entidades de servicio con Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure). 

Aquí le mostramos unos pasos generales de alto nivel para usar Azure Pipelines:

1. En Azure Pipelines, cree una canalización vacía.

1. Elija los recursos que necesita para la canalización, como la plantilla de aplicación lógica y los archivos de parámetros de plantilla, que genera manualmente o como parte del proceso de compilación.

1. Para el trabajo del agente, busque y agregue la tarea de **implementación del grupo de recursos de Azure**.

   ![Incorporación de la tarea "Implementación de un grupo de recursos de Azure"](./media/logic-apps-create-deploy-template/add-azure-resource-group-deployment-task.png)

1. Configúrelo con una [entidad de servicio](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure). 

1. Agregue referencias a la plantilla de la aplicación lógica y los archivos de parámetros de la plantilla.

1. Siga compilando los pasos del proceso de lanzamiento de otros entornos, pruebas automatizadas o aprobadores según sea necesario.

## <a name="get-support"></a>Obtención de soporte técnico

Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Supervisión de aplicaciones lógicas](../logic-apps/logic-apps-monitor-your-logic-apps.md)
