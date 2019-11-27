---
title: Modos de implementación de Azure Resource Manager | Microsoft Docs
description: Descripción de cómo especificar si desea usar un modo de implementación completa o incremental con Azure Resource Manager.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: tomfitz
ms.openlocfilehash: 762b0e74e8da20d1b48703385853765d5cc643af
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73953241"
---
# <a name="azure-resource-manager-deployment-modes"></a>Modos de implementación de Azure Resource Manager

Al implementar los recursos, debe especificar si la implementación es una actualización incremental o una actualización completa.  La diferencia entre estos dos modos es la forma en que Resource Manager controla los recursos existentes en el grupo de recursos que no están en la plantilla. El modo predeterminado es el incremental.

En ambos modos, Resource Manager intenta crear todos los recursos especificados en la plantilla. Si el recurso ya existe en el grupo de recursos y su configuración es igual, no se realizará ninguna operación en ese recurso. Si cambia los valores de propiedad de un recurso, el recurso se actualiza con esos nuevos valores. Si intenta actualizar la ubicación o el tipo de un recurso existente, la implementación produce un error. En su lugar, implemente un nuevo recurso con la ubicación o escriba la que necesite.

## <a name="complete-mode"></a>Modo completo

En el modo completo, Resource Manager **elimina** los recursos que existen en el grupo de recursos pero que no se especifican en la plantilla.

Si la plantilla incluye un recurso que no se ha implementado porque la [condición](conditional-resource-deployment.md) se evalúa como false, el resultado depende de la versión de API REST que use para implementar la plantilla. Si usa una versión anterior a 2019-05-10, el recurso **no se elimina**. Con 2019-05-10 o posterior, el recurso **se elimina**. Las versiones más recientes de Azure PowerShell y la CLI de Azure eliminan el recurso.

Tenga cuidado al usar el modo completo con [bucles de copia](resource-group-create-multiple.md). Se eliminan todos los recursos que no se especifican en la plantilla después de resolver el bucle de copia.

Si implementa en [más de un grupo de recursos en una plantilla](resource-manager-cross-resource-group-deployment.md), se pueden eliminar los recursos del grupo de recursos especificado en la operación de implementación. Los recursos de los grupos de recursos secundarios no se eliminarán.

Hay algunas diferencias en la forma en que los diferentes tipos de recursos controlan las eliminaciones en modo completo. Los recursos principales se eliminan automáticamente cuando no están en una plantilla implementada en modo completo. Por el contrario, algunos recursos secundarios no se eliminan automáticamente cuando no están en la plantilla. A pesar de ello, estos recursos secundarios sí se eliminan si también se elimina el recurso principal. 

Por ejemplo, si el grupo de recursos contiene una zona DNS (Microsoft.Network/tipo de recurso dnsZones) y un registro CNAME (Microsoft.Network/dnsZones/tipo de recurso CNAME), la zona DNS estará en el recurso principal del registro CNAME. Si la implementación se realiza en modo completo y no se incluye la zona DNS en la plantilla, se eliminará tanto la zona DNS como el registro CNAME. Si se incluye la zona DNS en la plantilla, pero no se incluye el registro CNAME, el registro CNAME no se eliminará. 

Para ver una lista del modo en que los diferentes tipos de recursos administran las eliminaciones, consulte [Eliminación de recursos de Azure en implementaciones en modo completo](complete-mode-deletion.md).

Si el grupo de recursos está [bloqueado](resource-group-lock-resources.md), el modo completo no elimina los recursos.

> [!NOTE]
> Solo las plantillas de nivel de raíz admiten el modo de implementación completa. En cuanto a [ plantillas vinculadas o anidadas ](resource-group-linked-templates.md), debe usar el modo incremental. 
>
> Las [implementaciones de nivel de suscripción](deploy-to-subscription.md) no son compatibles con el modo completo.
>
> Actualmente, el portal no admite el modo completo.
>

## <a name="incremental-mode"></a>Modo incremental

En el modo incremental, Resource Manager **deja sin modificar** los recursos que existen en el grupo de recursos pero que no se especifican en la plantilla.

Aun así, al volver a implementar un recurso existente en el modo incremental, el resultado es diferente. Especifique todas las propiedades para el recurso, no solo las que va a actualizar. Suele pensarse erróneamente que las propiedades que no se especifican se mantienen sin cambios. Si no especifica determinadas propiedades, Resource Manager interpreta la actualización como que sobrescribe esos valores.

## <a name="example-result"></a>Resultado de ejemplo

Para ilustrar la diferencia entre el modo incremental y el completo, considere el escenario siguiente.

El **grupo de recursos** contiene:

* Recurso A
* Recurso B
* Recurso C

La **plantilla** contiene:

* Recurso A
* Recurso B
* Recurso D

Cuando se implementa en modo **incremental**, el grupo de recursos contiene:

* Recurso A
* Recurso B
* Recurso C
* Recurso D

Cuando se implementa en modo **completo**, se elimina el recurso C. El grupo de recursos contiene:

* Recurso A
* Recurso B
* Recurso D

## <a name="set-deployment-mode"></a>Establecimiento del modo de implementación

Para establecer el modo de implementación al implementar con PowerShell, use el parámetro `Mode`.

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Para establecer el modo de implementación al implementar con la CLI de Azure, use el parámetro `mode`.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

En el siguiente ejemplo se muestra un conjunto de plantillas vinculadas al modo de implementación incremental:

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la creación de plantillas del Administrador de recursos, consulte [Creación de plantillas del Administrador de recursos de Azure](resource-group-authoring-templates.md).
* Para obtener más información sobre la implementación de recursos, consulte [Implementación de una aplicación con la plantilla del Administrador de recursos de Azure](resource-group-template-deploy.md).
* Para ver las operaciones de un proveedor de recursos, consulte [API de REST de Azure](/rest/api/).
