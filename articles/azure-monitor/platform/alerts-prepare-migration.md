---
title: Preparar para la migración de las alertas clásicas de Azure Monitor mediante la actualización de sus aplicaciones lógicas y runbooks
description: Obtenga información sobre cómo modificar el webhook, aplicación lógica y los runbooks para preparar la migración voluntaria.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 3c47404826d5055d4a82d4842523f790fb11f000
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632515"
---
# <a name="prepare-your-logic-apps-and-run-books-for-classic-alert-rules-migration"></a>Preparar las aplicaciones lógicas y ejecutar los libros en pantalla para la migración de las reglas de alerta clásica

Como [anunciaron](monitoring-classic-retirement.md), las alertas clásicas en Azure Monitor se retirará en julio de 2019. La herramienta de migración para desencadenar voluntariamente la migración está disponible en Azure portal y está implementando en los clientes que usan reglas de alerta clásicas.

Si opta por migrar voluntariamente las reglas de alertas clásicas para nuevas reglas de alerta, hay algunas diferencias entre los dos sistemas que debe tener en cuenta. Este artículo le guiará a través de las diferencias entre los dos sistemas y cómo puede prepararse para el cambio.

## <a name="api-changes"></a>Cambios de API

Las API utilizadas para crear y administrar reglas de alerta clásicas (`microsoft.insights/alertrules`) son diferentes de las API utilizadas para crear y administrar nuevas alertas de métrica (`microsoft.insights/metricalerts`). Si mediante programación crea y administra reglas de alertas clásicas hoy en día, actualice los scripts de implementación para trabajar con las nuevas API.

En la tabla siguiente proporciona una referencia a interfaces de programación para las alertas clásicas y nuevas.

|         |Alertas clásicas  |Nuevas alertas de métrica |
|---------|---------|---------|
|API DE REST     | [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](https://docs.microsoft.com/rest/api/monitor/metricalerts)       |
|Azure CLI     | [alerta de monitor AZ](https://docs.microsoft.com/cli/azure/monitor/alert?view=azure-cli-latest)        | [alerta de métricas de monitor AZ](https://docs.microsoft.com/cli/azure/monitor/metrics/alert?view=azure-cli-latest)        |
|PowerShell      | [Referencia](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule)       |      |
| Plantilla del Administrador de recursos de Azure | [Para las alertas clásicas](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-enable-template)|[Nuevas alertas de métrica](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates)|

## <a name="notification-payload-changes"></a>Cambios de carga de notificación

El formato de carga de notificación es ligeramente diferente entre [reglas de alerta clásicas](alerts-webhooks.md) y [nuevas alertas de métrica](alerts-metric-near-real-time.md#payload-schema). Si tiene cualquier webhook, la aplicación lógica o las acciones de runbook se desencadene mediante reglas de alerta clásicas, deberá actualizar esos puntos de conexión de notificación para que acepte el formato de carga de nuevas alertas de métrica.

Puede usar la tabla siguiente para asignar los campos de carga de webhook de la regla de alerta clásica y la nueva carga de webhook de alertas de métrica.

|  |Alertas clásicas  |Nuevas alertas de métrica |
|---------|---------|---------|
|Fue la alerta activa o resuelto     | status       | data.status |
|Información contextual sobre la alerta     | contexto        | data.context        |
|Marca de tiempo en el que se activó o resolver la alerta      | context.timestamp       | data.context.timestamp        |
| Id. de regla de alerta | context.id | data.context.id |
| Nombre de regla de alerta | context.name | data.context.name |
| Descripción de la regla de alerta | context.description | data.context.description |
| Condición de regla de alerta | context.condition | data.context.condition|
| Nombre de métrica | context.condition.metricName| data.context.condition.allOf[0].metricName|
| Agregación de tiempo (cómo se agrega la métrica en la ventana de evaluación)|data.context.condition.timeAggregation|data.context.condition.timeAggregation|
| Período de evaluación | context.condition.windowSize | data.context.condition.windowSize|
| Operador (cómo se compara el valor de métrica agregado con el umbral) | context.condition.operator | data.context.condition.operator|
| Umbral | context.condition.threshold| data.context.condition.allOf[0].threshold|
| Valor de métrica | context.condition.metricValue | data.context.condition.allOf[0].metricValue|
| Id. de suscripción | context.subscriptionId | data.context.subscriptionId|
| Grupo de recursos del recurso afectado | context.resourceGroup | data.context.resourceGroup|
| Nombre del recurso afectado | context.resourceName | data.context.resourceName |
| Tipo del recurso afectado | context.resourceType | data.context.resourceType |
|  Id. de recurso del recurso afectado | context.resourceId | data.context.resourceId |
| Un vínculo directo a la página de resumen de recursos del portal | context.portalLink | data.context.portalLink|
| Campos de carga personalizada que se pasarán al webhook o aplicación lógica | propiedades |data.properties |

Como puede ver, las cargas de ambos son similares. Sección siguiente tiene los detalles sobre las aplicaciones lógicas de ejemplo y un runbook de ejemplo para analizar la carga de notificación para las alertas nuevas.

## <a name="using-a-logic-app-that-receives-a-metric-alert-notification"></a>Mediante una aplicación lógica que recibe una notificación de alerta de métrica

Si usas aplicaciones lógicas con las alertas clásicas, deberá modificar la aplicación lógica para analizar la carga de las alertas de métricas nuevo.

1. Cree una nueva aplicación lógica.

2. Use la plantilla "Azure Monitor – métricas alerta Handler". Esta plantilla tiene un **solicitud HTTP** desencadenador con el esquema apropiado definido

    ![plantilla de aplicación lógica](media/alerts-migration/logic-app-template.png "plantilla de alerta de métrica")

3. Agregar una acción para hospedar su lógica de procesamiento.

## <a name="using-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Uso de un runbook de automation que recibe una notificación de alerta de métrica

El ejemplo siguiente proporciona código de PowerShell que puede usarse en el runbook que pueda analizar las cargas para reglas de alerta de métrica clásicas y nuevas reglas de alerta de métrica.

```PS
## Sample PowerShell code to be used in a runbook to handle parsing of both classic and new metric alerts

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

    # Identify if the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new Metric Alert schema
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic Metric Alert schema
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is not either a classic metric alert or a new metric alert
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    #parse fields related to resource affected
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

Vea un ejemplo completo de un runbook que se detiene una máquina virtual cuando se desencadena una alerta en [documentación de Azure Automation](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook).

## <a name="partner-integration-via-webhooks"></a>Integración de asociados a través de webhooks

La mayoría de [nuestros asociados que se integran con las alertas clásicas](https://docs.microsoft.com/azure/azure-monitor/platform/partners) ya son compatibles con nuevas alertas de métricas a través de sus integraciones. Integraciones conocidas que ya trabajan con nuevas alertas de métrica se enumeran a continuación.

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Signl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Si usas una integración de asociados que no aparece aquí, confirme con el proveedor de integración que funciona la integración con nuevas alertas de métrica.

## <a name="next-steps"></a>Pasos siguientes

- [Cómo usar la herramienta de migración](alerts-using-migration-tool.md)
- [Comprender el funcionamiento de la herramienta de migración](alerts-understand-migration.md)
