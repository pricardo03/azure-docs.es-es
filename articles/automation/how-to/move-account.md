---
title: Mover la cuenta de Azure Automation a otra suscripción
description: En este artículo se describe cómo mover la cuenta de Automation a otra suscripción
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 59d433bfb888eaa41cc8f66bdf3ad28c16efbe5c
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225967"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Mover la cuenta de Azure Automation a otra suscripción

Azure proporciona la capacidad para mover algunos recursos a un nuevo grupo de recursos o suscripción. Puede mover recursos a través de Azure portal, PowerShell, la CLI de Azure o la API de REST. Para obtener más información sobre el proceso, consulte [trasladar recursos a un nuevo grupo de recursos o suscripción](../../azure-resource-manager/resource-group-move-resources.md). 

Las cuentas de Azure Automation son uno de los recursos que se pueden mover. En este artículo, obtendrá información sobre los pasos para mover las cuentas de Automation a otra suscripción o recurso.

Los pasos generales para mover la cuenta de Automation son:

1. Quitar sus soluciones.
2. Desvincular el área de trabajo.
3. Mover la cuenta de Automation.
4. Eliminar y volver a crear las cuentas de ejecución.
5. Volver a habilitar las soluciones.

## <a name="remove-solutions"></a>Quitar soluciones

Para desvincular área de trabajo desde su cuenta de Automation, estas soluciones se deben quitar del área de trabajo:
- **Change Tracking e inventario**
- **Administración de actualizaciones** 
- **Start/Stop VMs during fuera del horario laboral** 

En el grupo de recursos, busque cada solución y seleccione **eliminar**. En el **eliminar recursos** página, confirme los recursos que desea quitar y seleccione **eliminar**.

![Eliminar soluciones desde el portal de Azure](../media/move-account/delete-solutions.png)

Puede realizar la misma tarea con la [Remove-AzureRmResource](/powershell/module/azurerm.resources/remove-azurermresource) cmdlet tal y como se muestra en el ejemplo siguiente:

```azurepowershell-interactive
$workspaceName = <myWorkspaceName>
$resourceGroupName = <myResourceGroup>
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
```

### <a name="additional-steps-for-startstop-vms"></a>Pasos adicionales para iniciar o detener las máquinas virtuales

Para el **Start/Stop VMs** solución, también debe quitar las reglas de alerta creadas por la solución.

En el portal de Azure, vaya al grupo de recursos y seleccione **supervisión** > **alertas** > **administrar reglas de alerta**.

![Selección de página que muestra las reglas de alerta de administración de alertas](../media/move-account/alert-rules.png)

En el **reglas** página, debería ver una lista de las alertas configuradas en ese grupo de recursos. El **Start/Stop VMs** solución crea tres reglas de alerta:

* AutoStop_VM_Child
* ScheduledStartStop_Parent
* SequencedStartStop_Parent

Seleccione las tres reglas de alerta y, a continuación, seleccione **eliminar**. Esta acción quitará estas reglas de alerta.

![Página de reglas de solicitud de confirmación de eliminación de las reglas seleccionadas](../media/move-account/delete-rules.png)

> [!NOTE]
> Si no ve ninguna regla de alerta en el **reglas** página, cambie la **estado** para mostrar **deshabilitado** alertas, porque es posible que deshabilitó ellos.

Cuando se quitan las reglas de alerta, quite el grupo de acciones que se creó para el **Start/Stop VMs** las notificaciones de la solución.

En el portal de Azure, seleccione **Monitor** > **alertas** > **administrar grupos de acciones**.

Seleccione **StartStop_VM_Notification** en la lista. En la página del grupo de acción, seleccione **eliminar**.

![Página del grupo de acción, seleccione Eliminar](../media/move-account/delete-action-group.png)

De forma similar, puede eliminar el grupo de acciones mediante el uso de PowerShell con la [Remove-AzureRmActionGroup](/powershell/module/azurerm.insights/remove-azurermactiongroup) cmdlet, tal como se muestra en el ejemplo siguiente:

```azurepowershell-interactive
Remove-AzureRmActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Desvincular el área de trabajo

En el portal de Azure, seleccione **cuenta de Automation** > **recursos relacionados** > **área de trabajo vinculado**. Seleccione **desvincular área de trabajo** desvincular el área de trabajo desde su cuenta de Automation.

![Desvincular un área de trabajo desde una cuenta de Automation](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Mover la cuenta de Automation

Después de quitar los elementos anteriores, puede seguir quitar la cuenta de Automation y sus runbooks. En el portal de Azure, vaya al grupo de recursos de su cuenta de Automation. Seleccione **mover** > **mover a otra suscripción**.

![Página de grupo de recursos, mover a otra suscripción](../media/move-account/move-resources.png)

Seleccione los recursos en el grupo de recursos que desea mover. Asegúrese de que incluye su **cuenta de Automation**, **Runbook**, y **área de trabajo de Log Analytics** recursos.

Una vez completada la migración, existen pasos adicionales necesarios para hacer que todo funcione.

## <a name="re-create-run-as-accounts"></a>Volver a crear las cuentas de ejecución

[Las cuentas de ejecución](../manage-runas-account.md) crear una entidad de servicio en Azure Active Directory para autenticar con recursos de Azure. Al cambiar las suscripciones, la cuenta de Automation ya no utiliza la cuenta de ejecución existente.

Vaya a la cuenta de Automation en la nueva suscripción y seleccione **se ejecutan como cuentas** en **configuración de la cuenta**. Verá que las cuentas de ejecución muestran ahora como incompletas.

![Las cuentas de ejecución están incompletas](../media/move-account/run-as-accounts.png)

Seleccione cada una de ellas. En el **propiedades** página, seleccione **eliminar** para eliminar la cuenta de ejecución.

> [!NOTE]
> Si no tiene permisos para crear o ver las cuentas de ejecución, verá el mensaje siguiente: `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` Para obtener información acerca de los permisos necesarios para configurar una cuenta de ejecución, consulte [los permisos necesitan para configurar las cuentas de ejecución](../manage-runas-account.md#permissions).

Después de eliminan las cuentas de ejecución, seleccione **crear** en **cuenta de ejecución**. En el **Agregar cuenta de ejecución** página, seleccione **crear** para crear la cuenta de ejecución y el servicio principal. Repita los pasos anteriores con el **cuenta de ejecución de Azure clásico como**.

## <a name="enable-solutions"></a>Habilitar soluciones

Después de volver a crear las cuentas de ejecución, se deberá volver a habilitar las soluciones que se haya quitado antes de moverlo. Para activar **Change Tracking e inventario** y **Update Management**, seleccione la funcionalidad correspondiente en la cuenta de Automation. Elija el área de trabajo de Log Analytics mueven y seleccione **habilitar**.

![Volver a habilitar las soluciones en la cuenta de Automation movida](../media/move-account/reenable-solutions.png)

Las máquinas que se incorporó con sus soluciones estará visibles cuando se ha conectado el área de trabajo de Log Analytics existente.

Para activar el **Start/Stop VMs** During OFF-hours, deberá volver a implementar la solución. En **recursos relacionados**, seleccione **Start/Stop VMs** > **Obtenga más información y habilitar la solución** > **crear** para iniciar la implementación.

En el **Agregar solución** página, elija el área de trabajo de Log Analytics y cuenta de Automation.  

![Agregar menú de la solución](../media/move-account/add-solution-vm.png)

Para obtener instrucciones detalladas sobre cómo configurar la solución, consulte [Start/Stop VMs During OFF-hours en Azure Automation](../automation-solution-vm-management.md).

## <a name="post-move-verification"></a>Comprobación posterior al movimiento

Una vez completada la migración, compruebe la siguiente lista de tareas que deben comprobarse:

|Capacidad|Pruebas|Vínculo de solución de problemas|
|---|---|---|
|Runbooks|Un runbook correctamente puede ejecutar y conectarse a recursos de Azure.|[Solución de problemas de runbooks](../troubleshoot/runbooks.md)
| Control de código fuente|Puede ejecutar una sincronización manual en el repositorio de control de código fuente.|[Integrar el control de código fuente](../source-control-integration.md)|
|Change tracking e inventario|Compruebe que puede ver datos del inventario actual de las máquinas.|[Solución de problemas de seguimiento de cambios](../troubleshoot/change-tracking.md)|
|Administración de actualizaciones|Compruebe que ve las máquinas y están en buen Estados.</br>Ejecute una implementación de actualizaciones de software de prueba.|[Solución de problemas de administración de actualizaciones](../troubleshoot/update-management.md)|

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de cómo mover los recursos en Azure, consulte [mover los recursos en Azure](../../azure-resource-manager/move-support-resources.md).
