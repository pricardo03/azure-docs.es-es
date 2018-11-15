---
title: Modos de implementación de Azure Resource Manager | Microsoft Docs
description: Descripción de cómo especificar si desea usar un modo de implementación completa o incremental con Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: c4347254df59c62085b2bfb195496bf479cf7b35
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344594"
---
# <a name="azure-resource-manager-deployment-modes"></a>Modos de implementación de Azure Resource Manager

Al implementar los recursos, debe especificar si la implementación es una actualización incremental o una actualización completa.  La diferencia principal entre estos dos modos es la forma en que Resource Manager controla los recursos existentes en el grupo de recursos que no están en la plantilla. El modo predeterminado es el incremental.

## <a name="incremental-and-complete-deployments"></a>Implementaciones de incrementales y completadas

Al implementar recursos:

* En el modo completo, Resource Manager **elimina** los recursos que existen en el grupo de recursos pero que no se especifican en la plantilla.
* En el modo incremental, Resource Manager **deja sin modificar** los recursos que existen en el grupo de recursos pero que no se especifican en la plantilla.

En ambos modos, Resource Manager intenta crear todos los recursos especificados en la plantilla. Si el recurso ya existe en el grupo de recursos y su configuración es igual, los resultados de la operación no cambian. Si cambia los valores de propiedad de un recurso, el recurso se actualiza con esos nuevos valores. Si intenta actualizar la ubicación o el tipo de un recurso existente, la implementación produce un error. En su lugar, implemente un nuevo recurso con la ubicación o escriba la que necesite.

Al volver a implementar un recurso en modo incremental, especifique todos los valores de propiedad del recurso, no solo los que va a actualizar. Si no especifica determinadas propiedades, Resource Manager interpreta la actualización como que sobrescribe esos valores.

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
New-AzureRmResourceGroupDeployment `
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

Al usar una [plantilla vinculada o anidada](resource-group-linked-templates.md), debe establecer la propiedad `mode` en `Incremental`. Solo las plantillas de nivel de raíz admiten el modo de implementación completa.

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
