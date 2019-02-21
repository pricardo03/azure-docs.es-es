---
title: Traslado de recursos de Azure a otro grupo de recursos | Microsoft Docs
description: Aprenda a trasladar recursos de Azure desde un grupo de recursos a otro, o desde una suscripción a otra.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 12/19/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: cf1894a218af35459e0d0dc432c5813169856cca
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267707"
---
# <a name="tutorial-move-azure-resources-to-another-resource-group-or-subscription"></a>Tutorial: Traslado de recursos de Azure a otro grupo de recursos o a otra suscripción

Aprenda a trasladar recursos de Azure desde un grupo de recursos a otro. También puede trasladar recursos de Azure desde una suscripción de Azure a otra. En este tutorial usará una plantilla de Resource Manager para implementar dos grupos de recursos y una cuenta de almacenamiento. Posteriormente, trasladará la cuenta de almacenamiento de un grupo de recursos a otro.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Preparación de los recursos.
> * Comprobación de que el recurso se puede trasladar.
> * Lista de comprobación antes de trasladar el recurso.
> * Validación de la operación de traslado.
> * Traslado del recurso.
> * Limpieza de recursos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prepare-the-resources"></a>Preparación de los recursos

Se ha creado una plantilla y se ha colocado en una [cuenta de almacenamiento compartido](https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json). La plantilla define dos grupos de recursos y una cuenta de almacenamiento. Al implementar la plantilla, deberá proporcionar un nombre de proyecto. El nombre del proyecto se utiliza para generar nombres de recursos únicos.  El siguiente JSON se extrae de la plantilla:

```json
"variables": {
  "resourceGroupSource": {
    "name": "[concat(parameters('projectName'), 'rg1')]",
    "location": "eastus"
  },
  "resourceGroupDestination": {
    "name": "[concat(parameters('projectName'), 'rg2')]",
    "location": "westus"      
  },
  "storageAccount": {
    "name": "[concat(parameters('projectName'), 'store')]",
    "location": "eastus"
  }
},
```

Observe las ubicaciones definidas en el archivo json. Los dos grupos de recursos están ubicados en Este de EE. UU. y Oeste de EE. UU. La cuenta de almacenamiento se encuentra en la región Este de EE. UU. Cuando traslada un recurso a otro grupo de recursos con una ubicación diferente, la operación de traslado no cambia la ubicación del recurso.

Seleccione **Probar** para abrir Cloud Shell y, a continuación, ejecute el script de PowerShell en Cloud Shell:

```azurepowershell-interactive
$projectName = Read-Host -prompt "Enter a project name"
New-AzDeployment `
    -Name $projectname `
    -Location "centralus" `
    -TemplateUri "https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json" `
    -projectName $projectName
```

Espere hasta que el script se haya completado correctamente. Después, abra [Azure Portal](https://portal.azure.com) y compruebe que los grupos de recursos y la cuenta de almacenamiento se han implementado de la forma esperada.

> [!NOTE]
> Puesto que la plantilla define dos grupos de recursos, esta implementación se considera una implementación en el nivel de suscripción. La implementación de plantillas de Azure Portal no admite implementaciones en el nivel de suscripción. Por tanto, se usará Azure PowerShell en este tutorial. La CLI de Azure también admite implementaciones en el nivel de suscripción. Consulte [Creación de grupos de recursos y otros recursos para una suscripción de Azure](./deploy-to-subscription.md).

## <a name="verify-the-resource-can-be-moved"></a>Comprobación de que el recurso se puede trasladar

No todos los recursos se pueden trasladar. El recurso utilizado en este tutorial es una cuenta de almacenamiento que sí se puede trasladar. Para comprobar si se puede trasladar un recurso, consulte [Servicios que se pueden mover](./resource-group-move-resources.md#services-that-can-be-moved).

## <a name="checklist-before-moving-resources"></a>Lista de comprobación antes de mover recursos

Este paso es opcional para este tutorial tal y como se ha realizado.

Hay algunos pasos importantes que deben realizarse antes de mover un recurso. Consulte [Lista de comprobación antes de trasladar recursos](./resource-group-move-resources.md#checklist-before-moving-resources).

## <a name="validate-the-move"></a>Validación del traslado

La validación del traslado es opcional para este tutorial tal y como se ha realizado.

La operación de validación del traslado le permite probar el escenario de traslado sin trasladar realmente los recursos. Use esta operación para determinar si el movimiento se realizará correctamente. Para más información, consulte [Validación del traslado](./resource-group-move-resources.md#validate-move).

## <a name="move-the-resource"></a>Traslado del recurso

La cuenta de almacenamiento está dentro del grupo de recursos de origen (rg1). Ejecute el siguiente script de PowerShell para mover el recurso al grupo de recursos de destino (rg2). Asegúrese de usar el mismo nombre de proyecto que utilizó al implementar los recursos.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

```azurepowershell-interactive
$projectName = Read-Host -prompt "Enter a project name"
$resourceGroupSource = $projectName + "rg1"
$resourceGroupDestination = $projectName + "rg2"
$storageAccountName = $projectName + "store"

$storageAccount = Get-AzResource -ResourceGroupName $resourceGroupSource -ResourceName $storageAccountName
Move-AzResource -DestinationResourceGroupName $resourceGroupDestination -ResourceId $storageAccount.ResourceId
```

Abra [Azure Portal](https://portal.azure.com), compruebe que la cuenta de almacenamiento se ha trasladado al otro grupo de recursos y compruebe también que la ubicación de la cuenta de almacenamiento sigue siendo Este de EE. UU.

Al mover los recursos, el grupo de origen y el grupo de destino se bloquean durante la operación. Las operaciones de escritura y eliminación están bloqueadas en los grupos de recursos hasta que se completa el movimiento. Este bloqueo significa que no puede agregar, actualizar ni eliminar recursos de los grupos de recursos, pero no que los recursos queden bloqueados. Por ejemplo, si mueve un servidor SQL Server y su base de datos a un nuevo grupo de recursos, una aplicación que utiliza la base de datos no experimenta ningún tiempo de inactividad. Todavía puede leer y escribir en la base de datos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos de origen.  
4. Seleccione **Eliminar grupo de recursos** del menú superior.
5. Seleccione el nombre del grupo de recursos de destino.  
6. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a trasladar una cuenta de almacenamiento de un grupo de recursos a otro. Hasta ahora, ha tratado con una cuenta de almacenamiento o varias instancias de cuenta de almacenamiento. En el tutorial siguiente, desarrollará una plantilla con varios recursos y varios tipos de recursos. Algunos de los recursos tienen recursos dependientes.

> [!div class="nextstepaction"]
> [Creación de plantillas de Azure Resource Manager con recursos dependientes](./resource-manager-tutorial-create-templates-with-dependent-resources.md)
