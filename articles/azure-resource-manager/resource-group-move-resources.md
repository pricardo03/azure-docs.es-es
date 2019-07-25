---
title: Traslado de recursos de Azure a una nueva suscripción o grupo de recursos | Microsoft Docs
description: Use Azure Resource Manager para trasladar recursos a un nuevo grupo de recursos o a una nueva suscripción.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 01ec8facf2771de9ec01b9470521340a59ee4d0d
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721387"
---
# <a name="move-resources-to-a-new-resource-group-or-subscription"></a>Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción

En este artículo se explica cómo trasladar recursos de Azure a otra suscripción de Azure o a otro grupo de recursos en la misma suscripción. Puede usar Azure Portal, Azure PowerShell, la CLI de Azure o la API REST para trasladar recursos.

Tanto el grupo de origen como el grupo de destino se bloquean durante la operación de traslado. Las operaciones de escritura y eliminación están bloqueadas en los grupos de recursos hasta que se completa el movimiento. Este bloqueo significa que no puede agregar, actualizar ni eliminar recursos de los grupos de recursos, pero no que los recursos queden bloqueados. Por ejemplo, si mueve un servidor SQL Server y su base de datos a un nuevo grupo de recursos, una aplicación que utiliza la base de datos no experimenta ningún tiempo de inactividad. Todavía puede leer y escribir en la base de datos.

Si se mueve un recurso, solo se mueve a un nuevo grupo de recursos o suscripción. No cambia la ubicación del recurso.

## <a name="checklist-before-moving-resources"></a>Lista de comprobación antes de mover recursos

Hay algunos pasos importantes que deben realizarse antes de mover un recurso. Puede evitar errores mediante la comprobación de estas condiciones.

1. Los recursos que desea trasladar deben ser compatibles con la operación de traslado. Para una lista de qué recursos son compatibles con el traslado, consulte [Compatibilidad con la operación de traslado para recursos](move-support-resources.md).

1. Algunos servicios tienen limitaciones o requisitos específicos al trasladar recursos. Si ha trasladado cualquiera de los siguientes servicios, compruebe esa guía antes de trasladarlo.

   * [Guía de traslado de App Services](./move-limitations/app-service-move-limitations.md)
   * [Guía de traslado de Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
   * [Guía de traslado del modelo de implementación clásica](./move-limitations/classic-model-move-limitations.md): proceso clásico, almacenamiento clásico, redes virtuales clásicas y Cloud Services
   * [Guía de traslado de Recovery Services](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
   * [Guía de movimiento de Virtual Machines](./move-limitations/virtual-machines-move-limitations.md)
   * [Guía de movimiento de Virtual Networks](./move-limitations/virtual-network-move-limitations.md)

1. Las suscripciones de origen y de destino deben estar activas. Si tiene problemas para habilitar una cuenta que se ha deshabilitado, [cree una solicitud de soporte técnico de Azure](../azure-supportability/how-to-create-azure-support-request.md). Seleccione **Administración de suscripciones** para el tipo de problema.

1. Las suscripciones de origen y destino deben existir en el mismo [inquilino de Azure Active Directory](../active-directory/develop/quickstart-create-new-tenant.md). Para comprobar que ambas suscripciones tienen el mismo identificador de inquilino, utilice Azure PowerShell o la CLI de Azure.

   Para Azure PowerShell, use:

   ```azurepowershell-interactive
   (Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
   (Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
   ```

   Para la CLI de Azure, utilice:

   ```azurecli-interactive
   az account show --subscription <your-source-subscription> --query tenantId
   az account show --subscription <your-destination-subscription> --query tenantId
   ```

   Si los identificadores de inquilino de las suscripciones de origen y destino no son los mismos, use los siguientes métodos para conciliarlos:

   * [Transferencia de la propiedad de una suscripción de Azure a otra cuenta](../billing/billing-subscription-transfer.md)
   * [Asociación o adición de una suscripción de Azure a Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. La suscripción de destino correspondiente al proveedor de recursos del recurso que se traslada debe estar registrada. Si no es así, recibirá un error en el que se indicará que la **suscripción no está registrada para un tipo de recurso**. Podría encontrar este error al mover un recurso a una nueva suscripción que nunca se ha utilizado con ese tipo de recurso.

   En PowerShell, use los siguientes comandos para obtener el estado de registro:

   ```azurepowershell-interactive
   Set-AzContext -Subscription <destination-subscription-name-or-id>
   Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
   ```

   Para registrar un proveedor de recursos, use:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
   ```

   En la CLI de Azure, use los siguientes comandos para obtener el estado de registro:

   ```azurecli-interactive
   az account set -s <destination-subscription-name-or-id>
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
   ```

   Para registrar un proveedor de recursos, use:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Batch
   ```

1. La cuenta que mueve los recursos debe tener al menos los permisos siguientes:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** en el grupo de recursos de origen.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** en el grupo de recursos de destino.

1. Antes de mover los recursos, compruebe las cuotas de la suscripción a la que está trasladando los recursos. Si trasladar los recursos significa que la suscripción excederá sus límites, debe revisar si puede solicitar un aumento de la cuota. Para ver una lista de estos límites y cómo solicitar un aumento, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).

## <a name="validate-move"></a>Validar el movimiento

La [operación de validación del movimiento](/rest/api/resources/resources/validatemoveresources) le permite probar el escenario de movimiento sin mover realmente los recursos. Use esta operación para comprobar si el movimiento se realizará correctamente. La validación se llama automáticamente cuando se envía una solicitud de traslado. Use esta operación solo cuando necesite determinar de antemano los resultados. Para ejecutar esta operación, necesita el:

* nombre del grupo de recursos de origen
* identificador de recurso del grupo de recursos de destino
* identificador de recurso de cada recurso que se va a mover
* [token de acceso](/rest/api/azure/#acquire-an-access-token) de la cuenta

Envíe la solicitud siguiente:

```HTTP
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2019-05-10
Authorization: Bearer <access-token>
Content-type: application/json
```

Con un cuerpo de la solicitud:

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Si la solicitud tiene un formato correcto, la operación devuelve:

```HTTP
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

El código de estado 202 indica que se ha aceptado la solicitud de validación, pero aún no se ha determinado si la operación de movimiento se realizará correctamente. El valor `location` contiene una dirección URL que se usa para comprobar el estado de la operación de ejecución larga.  

Para comprobar el estado, envíe la solicitud siguiente:

```HTTP
GET <location-url>
Authorization: Bearer <access-token>
```

Mientras todavía se esté ejecutando la operación, continuará recibiendo el código de estado 202. Espere el número de segundos que se indica en el valor `retry-after` antes de intentarlo de nuevo. Si la operación de movimiento se valida correctamente, recibirá el código de estado 204. Si falla la validación de movimiento, recibirá un mensaje de error, como:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="use-the-portal"></a>Uso del portal

Para trasladar recursos, seleccione el grupo de recursos con esos recursos y, después, el botón **Mover**.

![Mover recursos](./media/resource-group-move-resources/select-move.png)

Seleccione si va a mover los recursos a un nuevo grupo de recursos o a una nueva suscripción.

Seleccione los recursos que trasladar y el grupo de recursos de destino. Confirme que tiene que actualizar los scripts para estos recursos y seleccione **Aceptar**. Si ha seleccionado el icono de edición de la suscripción en el paso anterior, también debe seleccionar la suscripción de destino.

![seleccionar destino](./media/resource-group-move-resources/select-destination.png)

En **Notificaciones**, podrá ver que la operación de traslado está en curso.

![mostrar el estado del traslado](./media/resource-group-move-resources/show-status.png)

Cuando haya finalizado, se le notificará del resultado.

![mostrar el resultado del traslado](./media/resource-group-move-resources/show-result.png)

Si se produce un error, consulte [Troubleshoot moving Azure resources to new resource group or subscription](troubleshoot-move.md) (Solución de problemas del traslado de los recursos de Azure a una nueva suscripción o grupo de recursos).

## <a name="use-azure-powershell"></a>Uso de Azure PowerShell

Para mover recursos existentes a otro grupo de recursos o a otra suscripción, use el comando [Move-AzResource](/powershell/module/az.resources/move-azresource). El siguiente ejemplo muestra cómo trasladar varios recursos a un nuevo grupo de recursos.

```azurepowershell-interactive
$webapp = Get-AzResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Para moverlos a una nueva suscripción, especifique un valor para el parámetro `DestinationSubscriptionId`.

Si se produce un error, consulte [Troubleshoot moving Azure resources to new resource group or subscription](troubleshoot-move.md) (Solución de problemas del traslado de los recursos de Azure a una nueva suscripción o grupo de recursos).

## <a name="use-azure-cli"></a>Uso de CLI de Azure

Para trasladar recursos existentes a otro grupo de recursos o a otra suscripción, use el comando [az resource move](/cli/azure/resource?view=azure-cli-latest#az-resource-move). Proporcione los identificadores de recursos de los recursos que se van a mover. El siguiente ejemplo muestra cómo trasladar varios recursos a un nuevo grupo de recursos. En el parámetro `--ids`, ofrezca una lista separada por espacios de los identificadores de recurso que se van a trasladar.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Para mover a una nueva suscripción, proporcione el parámetro `--destination-subscription-id`.

Si se produce un error, consulte [Troubleshoot moving Azure resources to new resource group or subscription](troubleshoot-move.md) (Solución de problemas del traslado de los recursos de Azure a una nueva suscripción o grupo de recursos).

## <a name="use-rest-api"></a>Use la API de REST

Para mover recursos existentes a otro grupo de recursos o a otra suscripción, use la operación [Mover recursos](/rest/api/resources/Resources/MoveResources).

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

En el cuerpo de la solicitud, especifique el grupo de recursos de destino y los recursos a mover.

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Si se produce un error, consulte [Troubleshoot moving Azure resources to new resource group or subscription](troubleshoot-move.md) (Solución de problemas del traslado de los recursos de Azure a una nueva suscripción o grupo de recursos).

## <a name="next-steps"></a>Pasos siguientes

Para una lista de qué recursos son compatibles con el traslado, consulte [Compatibilidad con la operación de traslado para recursos](move-support-resources.md).
