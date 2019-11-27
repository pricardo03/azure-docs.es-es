---
title: Trasladar un área de trabajo de Log Analytics en Azure Monitor | Microsoft Docs
description: Aprenda cómo trasladar su área de trabajo de Log Analytics a otra suscripción o grupo de recursos.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/13/2019
ms.openlocfilehash: fd7ff7aa2275defba57aa24b5ef0b9adc78a5355
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74093238"
---
# <a name="move-a-log-analytics-workspace-to-different-subscription-or-resource-group"></a>Trasladar un área de trabajo de Log Analytics a otro grupo de recursos o suscripción

En este artículo, aprenderá los pasos para trasladar un área de trabajo de Log Analytics a otro grupo de recursos o suscripción de la misma región. Puede obtener más información sobre cómo trasladar recursos de Azure a través del Azure portal, PowerShell, la CLI de Azure o la API de REST. en [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../../azure-resource-manager/resource-group-move-resources.md). 

> [!IMPORTANT]
> No puede trasladar un área de trabajo a otra región.

## <a name="verify-active-directory-tenant"></a>Comprobar el Inquilino de Active Directory
Las suscripciones de origen y destino del área de trabajo deben existir dentro del mismo inquilino de Azure Active Directory. Use Azure PowerShell para verificar que ambas suscripciones tengan el mismo Identificador de inquilino.

``` PowerShell
(Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
(Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
```

## <a name="remove-solutions"></a>Eliminación de las soluciones
Las soluciones administradas que se instalan en el área de trabajo se moverán con la operación de traslado de área de trabajo de Log Analytics. Sin embargo, dado que debe eliminar el vínculo del área de trabajo a cualquier cuenta de automatización, las soluciones que dependen de ese vínculo deben eliminarse.

Las soluciones que deben eliminarse incluyen las siguientes: 

- Administración de actualizaciones
- Seguimiento de cambios
- Inicio y detención de máquinas virtuales durante las horas de trabajo


### <a name="azure-portal"></a>Portal de Azure
Use el siguiente procedimiento para eliminar las soluciones con Azure Portal:

1. Abra el menú del grupo de recursos en el que están instaladas las soluciones.
2. Seleccione la solución que quiera quitar.
3. Haga clic en **Eliminar recursos** y luego confirme los recursos que se eliminarán con un clic en **Eliminar**.

![Eliminar soluciones](media/move-workspace/delete-solutions.png)

### <a name="powershell"></a>PowerShell

Para eliminar las soluciones con PowerShell, use el cmdlet [Remove-AzResource](/powershell/module/az.resources/remove-azresource?view=azps-2.8.0) como se muestra en el siguiente ejemplo:

``` PowerShell
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM(<workspace-name>)" -ResourceGroupName <resource-group-name>
```

## <a name="remove-alert-rules"></a>Quitar reglas de alerta
Para la solución **Start/Stop VMs**, también debe quitar las reglas de alerta creadas por la solución. Use el siguiente procedimiento en Azure Portal para eliminar estas reglas.

1. Abra el menú **Supervisar** y luego seleccione **Alertas**.
2. Haga clic en **Administrar regla de alertas**.
3. Seleccione las siguientes tres reglas de alerta y luego haga clic en **Eliminar**.

   - AutoStop_VM_Child
   - ScheduledStartStop_Parent
   - SequencedStartStop_Parent

    ![Eliminar reglas](media/move-workspace/delete-rules.png)

## <a name="unlink-automation-account"></a>Desvincular la cuenta de Automation
Use el siguiente procedimiento para desvincular la cuenta de Automation del área de trabajo con Azure Portal:

1. Abra el menú **Cuentas de Automation** y luego seleccione la cuenta que desea eliminar.
2. En la sección **Recursos relacionados** del menú, seleccione **Área de trabajo vinculada**. 
3. Haga clic en **Desvincular área de trabajo** para desvincular el área de trabajo de su cuenta de Automation.

    ![Unlink workspace (Desvincular área de trabajo)](media/move-workspace/unlink-workspace.png)

## <a name="move-your-workspace"></a>Trasladar su área de trabajo

### <a name="azure-portal"></a>Portal de Azure
Use el siguiente procedimiento para trasladar su área de trabajo con Azure Portal:

1. Abra el menú **Áreas de trabajo de Log Analytics** y luego seleccione su área de trabajo.
2. En la página **Descripción general**, haga clic en **cambiar** después en **Grupo de recursos** o **Suscripción**.
3. Se abrirá una nueva página con una lista de recursos relacionados con el área de trabajo. Seleccione los recursos para pasar a la misma suscripción de destino y grupo de recursos que el área de trabajo. 
4. Seleccione un destino **Suscripción** y **Grupo de recursos**. Si va a trasladar el área de trabajo a otro grupo de recursos de la misma suscripción, no verá la opción **Suscripción**.
5. Haga clic en **Aceptar** para trasladar el área de trabajo y los recursos seleccionados.

    ![Portal](media/move-workspace/portal.png)

### <a name="powershell"></a>PowerShell
Para trasladar el área de trabajo con PowerShell, use [Move-AzResource](/powershell/module/AzureRM.Resources/Move-AzureRmResource) como en el ejemplo siguiente:

``` PowerShell
Move-AzResource -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyResourceGroup01/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace" -DestinationSubscriptionId "00000000-0000-0000-0000-000000000000" -DestinationResourceGroupName "MyResourceGroup02"
```



> [!IMPORTANT]
> Después de la operación de traslado, las soluciones eliminadas y el vínculo de la cuenta de Automation deben reconfigurarse para que el área de trabajo vuelva a su estado anterior.


## <a name="next-steps"></a>Pasos siguientes
- Para una lista de qué recursos son compatibles con el traslado, consulte [Compatibilidad con la operación de traslado para recursos](../../azure-resource-manager/move-support-resources.md).
