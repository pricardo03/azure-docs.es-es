---
title: Preparar para la migración de las alertas clásicas de Azure Monitor mediante la actualización de sus aplicaciones lógicas y runbooks
description: Obtenga información sobre cómo modificar los webhooks, las aplicaciones lógicas y runbooks para preparar la migración voluntaria.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: bdbd45c2b10dec8f1c0a85110747a470e818dbf9
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66015607"
---
# <a name="prepare-your-logic-apps-and-runbooks-for-migration-of-classic-alert-rules"></a>Preparar sus aplicaciones lógicas y los runbooks para la migración de las reglas de alerta clásicas

Como [anunciaron](monitoring-classic-retirement.md), las alertas clásicas en Azure Monitor se retirará en septiembre de 2019 (era originalmente de 2019 de julio). Una herramienta de migración está disponible en el portal de Azure a los clientes que utilizan las reglas de alerta clásicas y que desean desencadenar la migración.

> [!NOTE]
> Debido a un retraso en la puesta en servicio de herramienta de migración, la fecha de retirada para la migración de las alertas clásicas se ha ampliado a 31 de agosto de 2019 desde la fecha anunciada originalmente del 30 de junio de 2019.

Si opta por migrar voluntariamente las reglas de alertas clásicas para nuevas reglas de alerta, tenga en cuenta que existen algunas diferencias entre los dos sistemas. En este artículo se explica esas diferencias y cómo puede prepararse para el cambio.

## <a name="api-changes"></a>Cambios de API

Las API que crean y administran las reglas de alerta clásicas (`microsoft.insights/alertrules`) son diferentes de las API que crean y administran nuevas alertas de métrica (`microsoft.insights/metricalerts`). Si se crea mediante programación y administrar reglas de alertas clásicas de hoy en día, actualice los scripts de implementación para trabajar con las nuevas API.

En la tabla siguiente es una referencia a las interfaces de programación para las alertas clásicas y nuevas:

|         |Alertas clásicas  |Nuevas alertas de métrica |
|---------|---------|---------|
|API DE REST     | [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](https://docs.microsoft.com/rest/api/monitor/metricalerts)       |
|Azure CLI     | [alerta de monitor AZ](https://docs.microsoft.com/cli/azure/monitor/alert?view=azure-cli-latest)        | [alerta de métricas de monitor AZ](https://docs.microsoft.com/cli/azure/monitor/metrics/alert?view=azure-cli-latest)        |
|PowerShell      | [Referencia](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule)       |  [Referencia](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrulev2)    |
| Plantilla del Administrador de recursos de Azure | [Para las alertas clásicas](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-enable-template)|[Nuevas alertas de métrica](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates)|

## <a name="notification-payload-changes"></a>Cambios de carga de notificación

El formato de carga de notificación es ligeramente diferente entre [reglas de alerta clásicas](alerts-webhooks.md) y [nuevas alertas de métrica](alerts-metric-near-real-time.md#payload-schema). Si tiene cualquier webhook, aplicación lógica o acciones de runbook que se desencadenan mediante reglas de alerta clásicas, debe actualizar esos puntos de conexión de notificación para que acepte el formato de carga de nuevas alertas de métrica.

Utilice la tabla siguiente para asignar los campos de carga de webhook desde el formato clásico al nuevo formato:

|  |Alertas clásicas  |Nuevas alertas de métrica |
|---------|---------|---------|
|¿Fue la alerta activa o resueltos?    | **estado**       | **data.status** |
|Información contextual sobre la alerta     | **context**        | **data.context**        |
|Marca de tiempo en el que se activó o resolver la alerta     | **context.timestamp**       | **data.context.timestamp**        |
| Id. de regla de alerta | **context.id** | **data.context.id** |
| Nombre de la regla de alertas | **context.name** | **data.context.name** |
| Descripción de la regla de alerta | **context.description** | **data.context.description** |
| Condición de regla de alerta | **context.condition** | **data.context.condition** |
| Nombre de la métrica | **context.condition.metricName** | **data.context.condition.allOf[0].metricName** |
| Agregación de tiempo (cómo se agrega la métrica en la ventana de evaluación)| **data.context.condition.timeAggregation** | **data.context.condition.timeAggregation** |
| Período de evaluación | **context.condition.windowSize** | **data.context.condition.windowSize** |
| Operador (cómo se compara el valor de métrica agregado con el umbral) | **context.condition.operator** | **data.context.condition.operator** |
| Umbral | **context.condition.threshold** | **data.context.condition.allOf[0].threshold** |
| Valor de métrica | **context.condition.metricValue** | **data.context.condition.allOf[0].metricValue** |
| Id. de suscripción | **context.subscriptionId** | **data.context.subscriptionId** |
| Grupo de recursos del recurso afectado | **context.resourceGroup** | **data.context.resourceGroup** |
| Nombre del recurso afectado | **context.resourceName** | **data.context.resourceName** |
| Tipo del recurso afectado | **context.resourceType** | **data.context.resourceType** |
| Id. de recurso del recurso afectado | **context.resourceId** | **data.context.resourceId** |
| Vínculo directo a la página de resumen de recursos del portal | **context.portalLink** | **data.context.portalLink** |
| Campos de carga personalizada que se pasarán a la aplicación lógica o webhook | **properties** | **data.properties** |

Las cargas son similares, como puede ver. Ofrece la siguiente sección:

- Detalles acerca de cómo modificar las aplicaciones lógicas para trabajar con el nuevo formato.
- Un ejemplo de runbook que analiza la carga de notificación para las alertas nuevas.

## <a name="modify-a-logic-app-to-receive-a-metric-alert-notification"></a>Modificar una aplicación lógica para recibir una notificación de alerta de métrica

Si usa logic apps con las alertas clásicas, debe modificar el código de aplicación de lógica para analizar la carga de las alertas de métricas nuevo. Siga estos pasos:

1. Cree una nueva aplicación lógica.

1. Use la plantilla "Azure Monitor – métricas alerta Handler". Esta plantilla tiene un **solicitud HTTP** desencadenador con el esquema apropiado definido.

    ![plantilla de aplicación lógica](media/alerts-migration/logic-app-template.png "plantilla de alerta de métrica")

1. Agregar una acción para hospedar su lógica de procesamiento.

## <a name="use-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Usar un runbook de automation que recibe una notificación de alerta de métrica

El ejemplo siguiente proporciona código de PowerShell para usar en su runbook. Este código puede analizar las cargas para reglas de alerta de métrica clásicas y nuevas reglas de alerta de métrica.

```PowerShell
## Example PowerShell code to use in a runbook to handle parsing of both classic and new metric alerts.

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData.
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Determine whether the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new metric alert schema.
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic metric alert schema.
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is neither a classic metric alert nor a new metric alert.
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    # Parse fields related to resource affected.
    $ResourceName = $AlertContext.resourceName
    $ResourceType = $AlertContext.resourceType
    $ResourceGroupName = $AlertContext.resourceGroupName
    $ResourceId = $AlertContext.resourceId
    $SubId = $AlertContext.subscriptionId

    ## Your logic to handle the alert here.
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}

```

Para obtener un ejemplo completo de un runbook que se detiene una máquina virtual cuando se desencadena una alerta, consulte el [documentación de Azure Automation](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook).

## <a name="partner-integration-via-webhooks"></a>Integración de asociados a través de webhooks

La mayoría de [nuestros asociados que se integran con las alertas clásicas](https://docs.microsoft.com/azure/azure-monitor/platform/partners) ya son compatibles con nuevas alertas de métricas a través de sus integraciones. Integraciones conocidas que ya trabajan con nuevas alertas de métricas son:

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Signl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Si usa una integración de asociados que no aparece aquí, confirme con el proveedor de integración que funciona la integración con nuevas alertas de métrica.

## <a name="next-steps"></a>Pasos siguientes

- [Cómo usar la herramienta de migración](alerts-using-migration-tool.md)
- [Comprender el funcionamiento de la herramienta de migración](alerts-understand-migration.md)
