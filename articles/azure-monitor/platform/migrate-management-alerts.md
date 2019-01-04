---
title: Migración de las alertas de eventos de administración de Azure a Alertas del registro de actividad
description: Las Alertas de eventos de administración se retirarán el 1 de octubre. Preparar la migración de las alertas existentes.
author: johnkemnetz
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/14/2017
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: 8fd7161208f6303e03deaacca6a1e2fad24ec45e
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53580883"
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a>Migración de las alertas de eventos de administración de Azure a Alertas del registro de actividad


> [!WARNING]
> Las Alertas de eventos de administración se desactivarán a partir del 1 de octubre. Utilice las instrucciones siguientes para determinar si tiene estas alertas y migrarlas en caso afirmativo.
>
> 

## <a name="what-is-changing"></a>Lo que está cambiando

Azure Monitor (anteriormente Azure Insights) ofrecía una funcionalidad para crear una alerta desencadenada por eventos de administración y generar notificaciones a direcciones de correo electrónico o a una dirección URL de webhook. Es posible que haya creado una de estas alertas de cualquiera de las siguientes maneras:
* En Azure Portal para ciertos tipos de recursos, bajo Supervisión -> Alertas -> Agregar alerta, donde "Alerta de" se ha establecido en "Eventos"
* Mediante la ejecución del cmdlet de PowerShell Add-AzureRmLogAlertRule
* Utilizando directamente [la API de REST alert](https://docs.microsoft.com/rest/api/monitor/alertrules) con odata.type = "ManagementEventRuleCondition" y dataSource.odata.type = "RuleManagementEventDataSource"
 
El siguiente script de PowerShell devuelve una lista de todas las alertas de eventos de administración que tiene en su suscripción, así como las condiciones establecidas en cada alerta.

```powershell
Connect-AzureRmAccount
$alerts = $null
foreach ($rg in Get-AzureRmResourceGroup ) {
  $alerts += Get-AzureRmAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

Si no tiene alertas de eventos de administración, el cmdlet de PowerShell anterior dará como resultado una serie de mensajes de advertencia como este:

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

Estos mensajes de advertencia pueden ignorarse. Si tiene alertas de eventos de administración, la salida de este cmdlet de PowerShell tendrá un aspecto similar al siguiente:

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

Cada alerta está separada por una línea discontinua y los detalles incluyen el identificador de recurso de la alerta y la regla específica que se está supervisando.

Esta funcionalidad se ha pasado a [Alertas del registro de actividad de Azure Monitor](../../azure-monitor/platform/activity-log-alerts.md). Estas nuevas alertas permiten establecer una condición en los eventos del registro de actividad y recibir una notificación cuando un nuevo evento coincide con la condición. También ofrecen varias mejoras sobre las alertas de eventos de administración:
* Puede volver a usar el grupo de destinatarios de notificación ("acciones") en muchas alertas mediante los [Grupos de acciones](../../azure-monitor/platform/action-groups.md), lo que reduce la complejidad del cambio de quién debe recibir una alerta.
* Puede recibir una notificación directamente en su teléfono mediante SMS con Grupos de acciones.
* Puede [Crear alertas del registro de actividad con plantillas de Resource Manager](../../azure-monitor/platform/alerts-activity-log.md).
* Puede crear condiciones con mayor flexibilidad y complejidad para satisfacer sus necesidades específicas.
* Las notificaciones se entregan más rápidamente.
 
## <a name="how-to-migrate"></a>Cómo migrar
 
Para crear una nueva alerta del registro de actividad:
* Siga [nuestra guía sobre cómo crear una alerta en Azure Portal](../../azure-monitor/platform/activity-log-alerts.md)
* Obtenga información acerca de cómo [crear una alerta mediante una plantilla de Resource Manager](../../azure-monitor/platform/alerts-activity-log.md)
 
Las alertas de eventos de administración que haya creado previamente no se migrarán automáticamente a las alertas del registro de actividad. Debe utilizar el script de PowerShell anterior para mostrar las alertas de eventos de administración que tiene configuradas y volver a crearlas manualmente como alertas del registro de actividad. Esto debe hacerse antes del 1 de octubre, después del cual las alertas de eventos de administración ya no estará visibles en la suscripción de Azure. Otros tipos de alertas de Azure, incluidas las alertas de métricas de Azure Monitor, las alertas de Application Insights y las alertas de Log Analytics no se ven afectadas por este cambio. Si tiene alguna pregunta, indíquela en los comentarios a continuación.


## <a name="next-steps"></a>Pasos siguientes

* Más información sobre el [registro de actividad](../../azure-monitor/platform/activity-logs-overview.md)
* Configuración de [alertas del registro de actividad a través de Azure Portal](../../azure-monitor/platform/activity-log-alerts.md)
* Configuración de [alertas del registro de actividad a través de Resource Manager](../../azure-monitor/platform/alerts-activity-log.md)
* Revisión del [esquema de webhook de alertas del registro de actividad](../../azure-monitor/platform/activity-log-alerts-webhook.md)
* Más información sobre las [notificaciones del servicio](../../azure-monitor/platform/service-notifications.md)
* Más información sobre los [grupos de acciones](../../azure-monitor/platform/action-groups.md)
