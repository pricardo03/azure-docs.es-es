---
title: Creación de un área de trabajo con una plantilla de Azure Resource Manager
titleSuffix: Azure Machine Learning
description: Aprenda a usar una plantilla de Azure Resource Manager para crear una nueva área de trabajo de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 11/04/2019
ms.custom: seoapril2019
ms.openlocfilehash: b8584a77eaf9f8db13c948051c34b18236e2cc1a
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76986383"
---
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning"></a>Uso de una plantilla de Azure Resource Manager para crear un área de trabajo para Azure Machine Learning

En este artículo aprenderá varias formas de crear un área de trabajo de Azure Machine Learning mediante plantillas de Azure Resource Manager. Una plantilla de Resource Manager facilita la creación de recursos en una única operación coordinada. Una plantilla es un documento JSON que define los recursos que son necesarios para una implementación. También puede especificar los parámetros de implementación. Los parámetros se emplean para proporcionar valores de entrada cuando se usa la plantilla.

Para obtener más información, consulte [Implementación de una aplicación con la plantilla de Azure Resource Manager](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="prerequisites"></a>Prerequisites

* Una **suscripción de Azure**. Si no tiene una ya, pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Para usar una plantilla desde una CLI, necesita [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) o la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="resource-manager-template"></a>Plantilla de Resource Manager

La siguiente plantilla de Resource Manager se puede usar para crear un área de trabajo de Azure Machine Learning y los recursos de Azure asociados:

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

    La plantilla utilizará la ubicación que seleccione para la mayoría de los recursos. La excepción es el servicio Application Insights que no está disponible en todas las ubicaciones en las que están los demás servicios. Si selecciona una ubicación donde no está disponible, el servicio se creará en la ubicación Centro-sur de EE. UU.

* El **nombre del área de trabajo**, que es el nombre descriptivo del área de trabajo de Azure Machine Learning.

    > [!NOTE]
    > El nombre del área de trabajo no distingue mayúsculas de minúsculas.

    Los nombres de los demás servicios se generan aleatoriamente.

> [!TIP]
> Aunque la plantilla asociada a este documento crea un nuevo registro de contenedor de Azure, también puede crear una nueva área de trabajo sin necesidad de generar un registro de contenedor. Si no hay ningún registro de contenedor en el área de trabajo, se creará uno cuando se realice una operación que lo requiera. Por ejemplo, al entrenar o implementar un modelo.
>
> También se puede hacer referencia a un registro de contenedor o una cuenta de almacenamiento existentes en la plantilla de Azure Resource Manager, en lugar de crear otros nuevos.

Para más información sobre las plantillas, consulte los siguientes artículos:

* [Creación de plantillas del Administrador de recursos de Azure](../azure-resource-manager/templates/template-syntax.md)
* [Implementación de una aplicación con las plantillas de Azure Resource Manager](../azure-resource-manager/templates/deploy-powershell.md)
* [Tipos de recurso de Microsoft.MachineLearningServices](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

1. Siga los pasos que se indican en [Implementación de recursos desde plantilla personalizada](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template). Cuando llegue a la pantalla __Editar plantilla__, pegue la plantilla desde este documento.
1. Seleccione __Guardar__ para usar la plantilla. Proporcione la siguiente información y acepte los términos y condiciones que aparecen:

   * Suscripción: Seleccione la suscripción de Azure que va a usar para estos recursos.
   * Grupo de recursos: Seleccione o cree un grupo de recursos que contenga los servicios.
   * Nombre del área de trabajo: El nombre que se usará para el área de trabajo de Azure Machine Learning que se va a crear. El nombre del área de trabajo debe tener entre 3 y 33 caracteres. Solo puede contener caracteres alfanuméricos y "-".
   * Ubicación: Seleccione la ubicación en la que se crearán los recursos.

Para más información, vea [Implementación de recursos desde plantilla personalizada](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Uso de Azure PowerShell

En este ejemplo se da por supuesto que ha guardado la plantilla en un archivo denominado `azuredeploy.json` en el directorio actual:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace" -sku "basic"
```

Para más información, vea [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) e [Implementar la plantilla de Resource Manager privada con el token de SAS y Azure PowerShell](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="use-azure-cli"></a>Uso de CLI de Azure

En este ejemplo se da por supuesto que ha guardado la plantilla en un archivo denominado `azuredeploy.json` en el directorio actual:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace location=eastus sku=basic
```

Para más información, vea [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/templates/deploy-cli.md) e [Implementar la plantilla de Resource Manager privada con el token de SAS y la CLI de Azure](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="troubleshooting"></a>Solución de problemas

### <a name="resource-provider-errors"></a>Errores del proveedor de recursos

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Directiva de acceso de Azure Key Vault y plantillas de Azure Resource Manager

Una plantilla de Azure Resource Manager se usa para crear el área de trabajo y los recursos asociados (incluido Azure Key Vault) varias veces. Por ejemplo, con el uso de la plantilla varias veces con los mismos parámetros como parte de una canalización de implementación e integración continuas.

La mayoría de las operaciones de creación de recursos mediante plantillas son idempotentes, pero Key Vault borra las directivas de acceso cada vez que se usa la plantilla. Al borrar las directivas de acceso, se interrumpe el acceso al almacén de claves para las áreas de trabajo existentes que lo estén usando. Por ejemplo, las funcionalidades de detección y creación de Azure Notebooks pueden producir un error.  

Para evitar este problema, se recomienda uno de los siguientes enfoques:

* No implemente la plantilla de más de una vez con los mismos parámetros. O bien, elimine los recursos existentes antes de usar la plantilla para volver a crearlos.

* Examine las directivas de acceso de Key Vault y, luego, use estas directivas para establecer la propiedad `accessPolicies` de la plantilla. Para ver las directivas de acceso, use el siguiente comando de CLI de Azure:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query properties.accessPolicies
    ```

    Para más información sobre el uso de la `accessPolicies` sección de la plantilla, consulte la referencia del objeto [AccessPolicyEntry](https://docs.microsoft.com/azure/templates/Microsoft.KeyVault/2018-02-14/vaults#AccessPolicyEntry).

* Compruebe si ya existe el recurso de Key Vault. Si es así, no lo vuelva a crear con la plantilla. Por ejemplo, para usar el Key Vault existente en lugar de crear uno nuevo, realice los siguientes cambios en la plantilla:

    * **Agregue** un parámetro que acepte el identificador de un recurso de Key Vault existente:

        ```json
        "keyVaultId":{
          "type": "string",
          "metadata": {
            "description": "Specify the existing Key Vault ID."
          }
        }
      ```

    * **Elimine** la sección que crea un recurso de Key Vault:

        ```json
        {
          "type": "Microsoft.KeyVault/vaults",
          "apiVersion": "2018-02-14",
          "name": "[variables('keyVaultName')]",
          "location": "[parameters('location')]",
          "properties": {
            "tenantId": "[variables('tenantId')]",
            "sku": {
              "name": "standard",
              "family": "A"
            },
            "accessPolicies": [
            ]
          }
        },
        ```

    * **Elimine** la línea de la `"[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",` sección `dependsOn` del área de trabajo. También **cambie** la `keyVault` entrada en la sección `properties` del área de trabajo para hacer referencia al `keyVaultId` parámetro:

        ```json
        {
          "type": "Microsoft.MachineLearningServices/workspaces",
          "apiVersion": "2019-11-01",
          "name": "[parameters('workspaceName')]",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
            "[resourceId('Microsoft.Insights/components', variables('applicationInsightsName'))]"
          ],
          "identity": {
            "type": "systemAssigned"
          },
          "sku": {
            "tier": "[parameters('sku')]",
            "name": "[parameters('sku')]"
          },
          "properties": {
            "friendlyName": "[parameters('workspaceName')]",
            "keyVault": "[parameters('keyVaultId')]",
            "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
            "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccountName'))]"
          }
        }
        ```

    Después de estos cambios, puede especificar el identificador del recurso de Key Vault existente al ejecutar la plantilla. La plantilla volverá a usar el Key Vault estableciendo la propiedad `keyVault` del área de trabajo en su identificador.

    Para obtener el identificador de la Key Vault, puede hacer referencia a la salida de la ejecución de la plantilla original o usar el CLI de Azure. El siguiente comando es un ejemplo de uso de la CLI de Azure para obtener el identificador de recurso de Key Vault:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query id
    ```

    Este comando devuelve un valor similar al siguiente texto:

    ```text
    /subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de recursos con las plantillas de Resource Manager y la API REST de Resource Manager](../azure-resource-manager/templates/deploy-rest.md).
* [Creación e implementación de grupos de recursos de Azure mediante Visual Studio](../azure-resource-manager/templates/create-visual-studio-deployment-project.md).
