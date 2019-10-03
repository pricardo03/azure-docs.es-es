---
title: Transmisión de los registros de recurso de Azure a un centro de eventos
description: Aprenda a transmitir los registros de recurso de Azure a un centro de eventos.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 1d7a533658b6c72caae9649d7e5a9c4fad117245
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262995"
---
# <a name="stream-azure-resource-logs-to-azure-event-hubs"></a>Transmisión de los registros de recurso de Azure a Azure Event Hubs
Los [registros de recurso](resource-logs-overview.md) en Azure proporcionan datos exhaustivos y frecuentes acerca del funcionamiento interno de un recurso de Azure. En este artículo se describe la transmisión de los registros de recurso a Event Hubs para enviar datos a sistemas externos, como SIEM de terceros y otras soluciones de análisis de registros.


## <a name="what-you-can-do-with-resource-logs-sent-to-an-event-hub"></a>Qué se puede hacer con los registros de recurso enviados a un centro de eventos
Transmita los registros de recurso de Azure a centros de eventos para proporcionar la siguiente funcionalidad:

* **Transmisión de registros a sistemas de registro y telemetría de terceros**: transmita todos los registros de recurso a un centro de eventos único para canalizar datos de registro en una herramienta SIEM o de análisis de registros de terceros.
* **Creación de una plataforma de registro y telemetría personalizada**: la naturaleza altamente escalable de publicación y suscripción de los centros de eventos otorga la flexibilidad necesaria para ingerir registros de recurso en una plataforma de telemetría personalizada. Consulte [Designing and Sizing a Global Scale Telemetry Platform on Azure Event Hubs](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) (Diseño y cambio de tamaño de una plataforma de telemetría a escala global en Azure Event Hubs) para más información.

* **Visualización del estado del servicio mediante la transmisión de datos a Power BI**: con Event Hubs, Stream Analytics y Power BI, transforme los datos de diagnóstico en información casi en tiempo real de los servicios de Azure. Consulte [Stream Analytics y Power BI: panel de análisis en tiempo real de flujo de datos](../../stream-analytics/stream-analytics-power-bi-dashboard.md) para detalles sobre esta solución.

    El siguiente código SQL es una consulta de Stream Analytics de ejemplo que puede utilizar para analizar todos los datos de registro en una tabla de Power BI:
    
    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the Power BI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

## <a name="prerequisites"></a>Requisitos previos
Si no lo tiene, [cree un centro de eventos](../../event-hubs/event-hubs-create.md). Si anteriormente transmitió los registros de recurso a este espacio de nombres de Event Hubs, se reutilizará ese centro de eventos.

La directiva de acceso compartido del espacio de nombres define los permisos que tiene el mecanismo de transmisión. Para transmitir a Event Hubs, se necesitan permisos de administración, envío y escucha. Puede crear o modificar directivas de acceso compartido en la pestaña Configurar de Azure Portal para su espacio de nombres de Event Hubs.

Para actualizar la configuración de diagnóstico para incluir la transmisión, debe tener el permiso ListKey sobre esa regla de autorización de Event Hubs. El espacio de nombres de Event Hubs no tiene que estar necesariamente en la misma suscripción que la suscripción que emite los registros, siempre que el usuario que configure el valor tenga el acceso RBAC adecuado a ambas suscripciones y estas se encuentren en el mismo inquilino de AAD.

## <a name="create-a-diagnostic-setting"></a>Creación de una configuración de diagnóstico
Los registros de recurso no se recopilan de forma predeterminada. Envíelos a un centro de eventos y otros destinos mediante la creación de una configuración de diagnóstico para un recurso de Azure. Consulte [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](diagnostic-settings.md) para más información.

## <a name="stream-data-from-compute-resources"></a>Transmisión de datos desde recursos de proceso
El proceso de este artículo está destinado a los recursos que no son de proceso, tal como se describe en la [introducción a los registros de recurso de Azure](diagnostic-settings.md).
Transmita registros de recurso desde recursos de proceso de Azure mediante el agente de Windows Azure Diagnostics. Consulte [Transmisión de datos de Azure Diagnostics en la ruta de acceso activa mediante Event Hubs](diagnostics-extension-stream-event-hubs.md) para más detalles.


## <a name="consuming-log-data-from-event-hubs"></a>Consumo de los datos de registro desde los centros de eventos
Al consumir registros de recurso desde centros de eventos, tendrán formato JSON con los elementos de la tabla siguiente.

| Nombre del elemento | DESCRIPCIÓN |
| --- | --- |
| records |Matriz de todos los eventos de registro de esta carga. |
| time |Hora a la que se produjo el error. |
| category |Categoría de registro para este evento. |
| resourceId |Id. de recurso del recurso que generó este evento. |
| operationName |Nombre de la operación. |
| level |Opcional. Indica el nivel de registro de eventos. |
| properties |Propiedades del evento. Estas varían para cada servicio de Azure, tal y como se describe en [](). |


A continuación se muestran datos de salida de ejemplo de Event Hubs:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```



## <a name="next-steps"></a>Pasos siguientes

* [Transmisión de registros de Azure Active Directory con Azure Monitor](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md).
* [Más información sobre los registros de recurso de Azure](resource-logs-overview.md).
* [Introducción a Event Hubs](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md).

