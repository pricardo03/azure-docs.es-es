---
title: 'Implementación de aplicaciones lógicas con plantillas de Azure Resource Manager: Azure Logic Apps'
description: Implementación de aplicaciones lógicas con plantillas de Azure Resource Manager
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.date: 10/15/2017
ms.openlocfilehash: bbb10bf0174b6e06e28d171510345ed92b6642d9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59357097"
---
# <a name="deploy-logic-apps-with-azure-resource-manager-templates"></a>Implementación de aplicaciones lógicas con plantillas de Azure Resource Manager

Después de crear una plantilla de Azure Resource Manager para implementar la aplicación lógica, puede implementar la plantilla de las siguientes maneras:

* [Azure Portal](#portal)
* [Azure PowerShell](#powershell)
* [CLI de Azure](#cli)
* [API de REST del Administrador de recursos de Azure](../azure-resource-manager/resource-group-template-deploy-rest.md)
* [Canalizaciones de Azure de DevOps de Azure](#azure-pipelines)

<a name="portal"></a>

## <a name="deploy-through-azure-portal"></a>Implementar a través del portal de Azure

Para implementar automáticamente una plantilla de aplicación lógica en Azure, puede elegir la siguiente **implementar en Azure** botón, que inicia sesión en el portal de Azure y le pide información sobre la aplicación lógica. A continuación, hacer los cambios necesarios en la plantilla de aplicación lógica o los parámetros.

[![Implementación en Azure](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Por ejemplo, se le pedirá esta información después de iniciar sesión el portal de Azure:

* El nombre de la suscripción a Azure
* El grupo de recursos que desea usar
* La ubicación de la aplicación lógica
* Nombre de la aplicación lógica
* Un URI de prueba
* La aceptación de los términos y condiciones especificados

Para obtener más información, consulte [implementar recursos con plantillas de Azure Resource Manager y Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).

## <a name="authorize-oauth-connections"></a>Autorización de conexiones de OAuth

Después de la implementación, la aplicación lógica funciona de un extremo a otro con parámetros válidos. Sin embargo, sigue siendo necesario autorizar las conexiones de OAuth para generar un token de acceso válido. Para las implementaciones automatizadas, puede utilizar una secuencia de comandos que da su consentimiento a cada conexión de OAuth, por ejemplo, esto [script de ejemplo en el proyecto de GitHub LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth). También puede autorizar las conexiones de OAuth a través del portal de Azure o en Visual Studio, abra la aplicación lógica en el Diseñador de Logic Apps.

<a name="powershell"></a>

## <a name="deploy-with-azure-powershell"></a>Implementación con Azure PowerShell

Para implementar en un determinado *grupo de recursos de Azure*, use este comando:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <Azure-resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json 
```

Para implementar en una suscripción de Azure específica, use este comando:

```powershell
New-AzDeployment -Location <location> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json 
```

* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)
* [`New-AzResourceGroupDeployment`](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)
* [`New-AzDeployment`](/powershell/module/az.resources/new-azdeployment)

<a name="cli"></a>

## <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

Para implementar en un determinado *grupo de recursos de Azure*, use este comando:

```azurecli
az group deployment create -g <Azure-resource-group-name> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Para implementar en una suscripción de Azure específica, use este comando:

```azurecli
az deployment create --location <location> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Para más información, consulte los temas siguientes: 

* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/resource-group-template-deploy-cli.md) 
* [`az group deployment create`](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)
* [`az deployment create`](https://docs.microsoft.com/cli/azure/deployment?view=azure-cli-latest#az-deployment-create)

<a name="azure-pipelines"></a>

## <a name="deploy-with-azure-devops"></a>Implementar con Azure DevOps

Para implementar plantillas de aplicación lógica y administrar entornos, los equipos suelen usan una herramienta como [canalizaciones de Azure](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines) en [Azure DevOps](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops-services). Las canalizaciones de Azure proporciona un [tareas de implementación de grupo de recursos de Azure](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzureResourceGroupDeploymentV2) que puede agregar a cualquier compilación o versión de canalización.
Para que la autorización implementar y generar la canalización de versiones, también se necesita un Azure Active Directory (AD) [serviceprincipal](../active-directory/develop/app-objects-and-service-principals.md). Obtenga más información sobre [mediante entidades de servicio con las canalizaciones de Azure](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure). 

Estos son los pasos de alto nivel generales para el uso de canalizaciones de Azure:

1. En las canalizaciones de Azure, cree una canalización vacía.

1. Elija los recursos que necesita para la canalización, como la plantilla de aplicación lógica y los archivos de parámetros de plantilla, que genera manualmente o como parte del proceso de compilación.

1. Para el trabajo del agente, busque y agregue el **implementación del grupo de recursos de Azure** tarea.

   ![Agregar tarea "Implementación de grupo de recursos de Azure"](./media/logic-apps-create-deploy-template/add-azure-resource-group-deployment-task.png)

1. Configurar con un [serviceprincipal](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure). 

1. Agregue referencias a la plantilla de aplicación lógica y los archivos de parámetros de plantilla.

1. Siga compilando los pasos del proceso de lanzamiento de otros entornos, pruebas automatizadas o aprobadores según sea necesario.

## <a name="get-support"></a>Obtención de soporte técnico

Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Supervisión de aplicaciones lógicas](../logic-apps/logic-apps-monitor-your-logic-apps.md)
