---
title: Revertir en caso de error a una implementación correcta
description: Especifique que una implementación con errores debe revertir a una implementación correcta.
ms.topic: conceptual
ms.date: 10/04/2019
ms.openlocfilehash: 0978d2547e23a9ac6f920c76be3e877ec236d9ed
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74150334"
---
# <a name="rollback-on-error-to-successful-deployment"></a>Revertir en caso de error a una implementación correcta

Cuando se produce un error en una implementación, puede ejecutar automáticamente desde el historial de implementación una implementación anterior que sea correcta. Esta funcionalidad es útil si tiene un estado correcto conocido para la implementación de la infraestructura y quiere volver a ese estado. Hay una serie de advertencias y restricciones:

- La reimplementación se ejecuta exactamente como se ejecutó antes y con los mismos parámetros. Los parámetros no se pueden cambiar.
- La implementación anterior se ejecuta con el [modo completo](./deployment-modes.md#complete-mode). Los recursos no incluidos en la implementación anterior se eliminan y las configuraciones del recurso se establecen en su estado anterior. Asegúrese de que comprende perfectamente los [modos de implementación](./deployment-modes.md).
- La reimplementación solo afecta a los recursos; los cambios en los datos no se ven afectados.
- Esta característica solo se puede usar con implementaciones de grupo de recursos, no con implementaciones de nivel de grupo de administración o suscripción. Para más información sobre las implementaciones en el nivel de suscripción, consulte [Creación de grupos de recursos y otros recursos en el nivel de suscripción](./deploy-to-subscription.md).
- Solo se puede usar esta opción con las implementaciones de nivel de raíz. Las implementaciones de una plantilla anidada no están disponibles para volver a implementarse.

Para usar esta opción, las implementaciones deben tener nombres únicos para que se puedan identificar en el historial. Si no tienen nombres únicos, la implementación con error en cuestión podría sobrescribir la implementación anteriormente correcta en el historial.

## <a name="powershell"></a>PowerShell

Para volver a implementar la última implementación correcta, agregue el parámetro `-RollbackToLastDeployment` como una marca.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollbackToLastDeployment
```

Para volver a implementar una implementación específica, use el parámetro `-RollBackDeploymentName` y proporcione el nombre de la implementación. La implementación especificada debe haberse realizado correctamente.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollBackDeploymentName ExampleDeployment01
```

## <a name="azure-cli"></a>CLI de Azure

Para volver a implementar la última implementación correcta, agregue el parámetro `--rollback-on-error` como una marca.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error
```

Para volver a implementar una implementación específica, use el parámetro `--rollback-on-error` y proporcione el nombre de la implementación. La implementación especificada debe haberse realizado correctamente.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment02 \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error ExampleDeployment01
```

## <a name="rest-api"></a>API DE REST

Para volver a implementar la última implementación correcta si se produce un error en la actual, use:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "LastSuccessful",
    }
  }
}
```

Para volver a implementar una implementación específica si se produce un error en la implementación actual, use:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "SpecificDeployment",
      "deploymentName": "<deploymentname>"
    }
  }
}
```

La implementación especificada debe haberse realizado correctamente.

## <a name="next-steps"></a>Pasos siguientes

- Para el lanzamiento seguro del servicio en más de una región, consulte [Azure Deployment Manager](deployment-manager-overview.md).
- Para especificar cómo controlar los recursos que existen en el grupo de recursos, pero que no están definidos en la plantilla, consulte [Modos de implementación de Azure Resource Manager](deployment-modes.md).
- Para entender cómo definir parámetros en la plantilla, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](resource-group-authoring-templates.md).
- Para más información sobre la implementación de una plantilla que requiere un token de SAS, vea [Implementación de una plantilla privada con el token de SAS](resource-manager-powershell-sas-token.md).
