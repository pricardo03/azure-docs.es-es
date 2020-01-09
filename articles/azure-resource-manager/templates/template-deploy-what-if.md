---
title: Despliegue de plantillas hipotéticas (Vista previa)
description: Determine los cambios que se producirán en los recursos antes de implementar una plantilla de Azure Resource Manager.
author: mumian
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: jgao
ms.openlocfilehash: edb9f5e35008b1270031d8e2d5c8a5efa37cb554
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476274"
---
# <a name="resource-manager-template-deployment-what-if-operation-preview"></a>Operación hipotética de la implementación de plantilla al Administrador de recursos (vista previa)

Antes de implementar una plantilla, es posible que desee obtener una vista previa de los cambios que se producirán. Azure Resource Manager proporciona la operación hipotética que le permite ver cómo cambiarán los recursos si implementa la plantilla. La operación hipotética no realiza ningún cambio en los recursos existentes. En su lugar, predice los cambios si se implementa la plantilla especificada.

> [!NOTE]
> La operación hipotética se encuentra actualmente en versión preliminar. Para usarlo, debe [registrarse en la versión preliminar](https://aka.ms/armtemplatepreviews). Como versión preliminar, los resultados a veces pueden mostrar que un recurso cambiará cuando realmente no se produzca ningún cambio. Trabajamos para reducir estos problemas, pero necesitamos su ayuda. Informe de estos problemas en [https://aka.ms/whatifissues](https://aka.ms/whatifissues).

Puede usar la operación hipotética con la operación REST `New-AzDeploymentWhatIf`Comando PowerShell o las [implementaciones hipotéticas](/rest/api/resources/deployments/whatif).

En PowerShell, el resultado tiene el siguiente aspecto:

![Operación hipotética de implementación de plantilla al administrador de recursos fullresourcepayload y tipos de cambio](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

## <a name="change-types"></a>Tipos de cambio

La operación hipotética muestra seis tipos diferentes de cambios:

- **Crear**: El recurso no existe actualmente, pero se define en la plantilla. Se creará el recurso.

- **Eliminar**: Este tipo de cambio solo se aplica cuando se usa [Modo completo](deployment-modes.md) para la implementación. El recurso existe, pero no se define en la plantilla. Con el modo completo, se eliminará el recurso. Solo los recursos que [admiten la eliminación de modo completo](complete-mode-deletion.md) se incluyen en este tipo de cambio.

- **Ignorar**: El recurso existe, pero no se define en la plantilla. No se implementará ni modificará el recurso.

- **NoChange**: El recurso existe y se define en la plantilla. El recurso se volverá a implementar, pero las propiedades del recurso no cambiarán. Este tipo de cambio se devuelve cuando [ResultFormat](#result-format) se establece en `FullResourcePayloads`, que es el valor predeterminado.

- **Modificar**: El recurso existe y se define en la plantilla. El recurso se volverá a implementar y las propiedades del recurso cambiarán. Este tipo de cambio se devuelve cuando [ResultFormat](#result-format) se establece en `FullResourcePayloads`, que es el valor predeterminado.

- **Implementación**: El recurso existe y se define en la plantilla. El recurso será reimplementado. Es posible que las propiedades del recurso cambien o no. La operación devuelve este tipo de cambio cuando no tiene información suficiente para determinar si cualquier propiedad cambiará. Solo verá esta condición cuando [ResultFormat](#result-format) se establece en `ResourceIdOnly`.

## <a name="deployment-scope"></a>Ámbito de la implementación

Puede usar la operación hipotética para las implementaciones en el nivel de suscripción o de grupo de recursos. Establezca el ámbito de implementación con el parámetro `-ScopeType`. Los valores aceptados son `Subscription` y `ResourceGroup`. En este artículo se muestran las implementaciones del grupo de recursos.

Para más información sobre las implementaciones de nivel de suscripción, consulte [Crear grupos de recursos y recursos en el nivel de suscripción](deploy-to-subscription.md#).

## <a name="result-format"></a>Formato de resultado

Puede controlar el nivel de detalle que se devuelve sobre los cambios de predicción. Establezca el parámetro `ResultFormat` en `FullResourcePayloads` para obtener una lista de recursos que cambiarán y detalles sobre las propiedades que cambiarán. Establezca el parámetro `ResultFormat` en `ResourceIdOnly` para obtener una lista de los recursos que cambiarán. El valor predeterminado es `FullResourcePayloads`.  

Las capturas de pantallas siguientes muestran los dos formatos de salida diferentes:

- Cargas de recursos completas

    ![Operación hipotética de implementación de plantilla al administrador de recursos salida de fullresourcepayload](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-fullresourcepayload.png)

- Solo identificador de recurso

    ![Operación hipotética de implementación de plantilla al administrador de recursos salida de resourceidonly](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-resourceidonly.png)

## <a name="run-what-if-operation"></a>Ejecutar la operación hipotética

### <a name="set-up-environment"></a>Configuración de entorno

Para ver cómo funciona, vamos a ejecutar algunas pruebas. En primer lugar, implemente una plantilla desde [Plantillas de inicio rápido de Azure que crea una cuenta de almacenamiento](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json). El tipo de cuenta de almacenamiento predeterminado es `Standard_LRS`. Usará esta cuenta de almacenamiento para probar cómo se registran los cambios en el caso de que ocurran.

```azurepowershell-interactive
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

### <a name="test-modification"></a>Modificación de prueba

Una vez finalizada la implementación, está listo para probar la operación hipotética. Ejecute el comando what-if, pero cambie el tipo de cuenta de almacenamiento a `Standard_GRS`.

```azurepowershell-interactive
New-AzDeploymentWhatIf `
  -ScopeType ResourceGroup `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" `
  -storageAccountType Standard_GRS
```

La salida de what-if es similar a:

![Administrador de recursos de implementación de plantilla: salida de la operación hipotética](./media/template-deploy-what-if/resource-manager-deployment-whatif-output.png)

Observe en la parte superior de la salida que los colores están definidos para indicar el tipo de cambios.

En la parte inferior de la salida, se muestra el nombre de la SKU (tipo de cuenta de almacenamiento) que se va a cambiar de **Standard_LRS** a **Standard_GRS**.

Algunas de las propiedades que se enumeran como eliminadas no cambiarán realmente. En la imagen anterior, estas propiedades son accessTier, encryption.keySource y otras de esa sección. Las propiedades se pueden notificar incorrectamente como eliminadas cuando no están en la plantilla, pero se establecen de forma automática durante la implementación como valores predeterminados. Este resultado se considera "ruido" en la respuesta what-if. El recurso implementado final tendrá los valores establecidos para las propiedades. A medida que la operación what-if evolucione, estas propiedades se excluirán del resultado.

### <a name="test-deletion"></a>Eliminación de prueba

La operación hipotética admite el uso del [modo de implementación](deployment-modes.md). Cuando se establece en el modo completo, se eliminan los recursos que no están en la plantilla. En el ejemplo siguiente se implementa una plantilla [que no tiene recursos definidos](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json) en el modo completo.

```azurepowershell-interactive
New-AzDeploymentWhatIf `
  -ScopeType ResourceGroup `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json" `
  -Mode Complete
```

Dado que no hay recursos definidos en la plantilla y el modo de implementación está establecido en completo, se eliminará la cuenta de almacenamiento.

![Implementación de plantilla del Administrador de recursos modo de implementación de salida de operación hipotética completa](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

Es importante recordar qué sucede si no realiza ningún cambio real. La cuenta de almacenamiento todavía existe en el grupo de recursos.

## <a name="next-steps"></a>Pasos siguientes

- Si observa resultados incorrectos de la versión preliminar de what-if, informe de los problemas en [https://aka.ms/whatifissues](https://aka.ms/whatifissues).
- Para implementar plantillas con Azure PowerShell, consulte [Implementar recursos con plantillas del Administrador de recursos y Azure PowerShell](deploy-powershell.md).
- Para implementar plantillas con REST, consulte [Implementación de recursos con plantillas del Administrador de recursos y Administrador de recursos API de REST](deploy-rest.md).
- Para revertir a una implementación correcta cuando se produce un error, consulte [Revertir en caso de error a una implementación correcta](rollback-on-error.md).
