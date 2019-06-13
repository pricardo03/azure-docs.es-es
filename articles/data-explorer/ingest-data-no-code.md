---
title: 'Tutorial: Ingesta de datos de diagnóstico y de registro de actividad en Azure Data Explorer sin una línea de código'
description: En este tutorial, obtendrá información sobre cómo introducir datos en Azure Data Explorer sin una línea de código y consultar datos.
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 04/29/2019
ms.openlocfilehash: 187aa4b02e389c485b24ad7de256422d1880182b
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64872600"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Tutorial: Ingesta de datos en Azure Data Explorer sin una línea de código

En este tutorial se muestra cómo ingerir datos de registros de actividad y diagnóstico para un clúster de Azure Data Explorer sin escribir código. Con este sencillo método de ingesta, puede empezar a consultar datos de Azure Data Explorer rápidamente para el análisis de datos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Cree la asignación de la ingesta y las tablas en una base de datos de Azure Data Explorer.
> * Aplicar formato a los datos ingeridos mediante una directiva de actualización
> * Crear un [centro de eventos](/azure/event-hubs/event-hubs-about) y conectarlo a Azure Data Explorer
> * Transmitir datos a un centro de eventos desde los [registros de diagnóstico de Azure Monitor](/azure/azure-monitor/platform/diagnostic-logs-overview) y los [registros de actividad de Azure Monitor](/azure/azure-monitor/platform/activity-logs-overview)
> * Consultar los datos ingeridos mediante Azure Data Explorer

> [!NOTE]
> Crear todos los recursos en la misma ubicación o región de Azure Esto es un requisito para los registros de diagnóstico de Azure Monitor.

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* [Un clúster y la base de datos de Azure Data Explorer](create-cluster-database-portal.md). En este tutorial, el nombre de la base de datos es *TestDatabase*.

## <a name="azure-monitor-data-provider-diagnostic-and-activity-logs"></a>Proveedor de datos de Azure Monitor: registros de actividad y diagnóstico

Vea y comprenda los datos proporcionados por los registros de actividad y diagnóstico de Azure Monitor a continuación. Se creará una canalización de ingesta en función de estos esquemas de datos. Tenga en cuenta que cada evento de un registro tiene una matriz de registros. Esta matriz de registros se dividirá más adelante en el tutorial.

### <a name="diagnostic-logs-example"></a>Ejemplo de los registros de diagnóstico

Los registros de diagnóstico de Azure son emitidos por un servicio de Azure que proporciona datos sobre el funcionamiento de dicho servicio. Los datos se agregan con un intervalo de agregación de 1 minuto. Este es un ejemplo de un esquema de eventos de métricas de Azure Data Explorer, según la duración de la consulta:

```json
{
    "records": [
    {
        "count": 14,
        "total": 0,
        "minimum": 0,
        "maximum": 0,
        "average": 0,
        "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
        "time": "2018-12-20T17:00:00.0000000Z",
        "metricName": "QueryDuration",
        "timeGrain": "PT1M"
    },
    {
        "count": 12,
        "total": 0,
        "minimum": 0,
        "maximum": 0,
        "average": 0,
        "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
        "time": "2018-12-21T17:00:00.0000000Z",
        "metricName": "QueryDuration",
        "timeGrain": "PT1M"
    }
    ]
}
```

### <a name="activity-logs-example"></a>Registros de actividad de ejemplo

Los registros de actividad de Azure son registros de nivel de suscripción que proporcionan información sobre las operaciones realizadas en los recursos de la suscripción. Este es un ejemplo de un evento del registro de actividad para la comprobación del acceso:

```json
{
    "records": [
    {
        "time": "2018-12-26T16:23:06.1090193Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Start",
        "resultSignature": "Started.",
        "durationMs": 0,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            ...
        }
    },
    {
        "time": "2018-12-26T16:23:06.3040244Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Success",
        "resultSignature": "Succeeded.OK",
        "durationMs": 194,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            "statusCode": "OK",
            "serviceRequestId": "87acdebc-945f-4c0c-b931-03050e085626"
        }
    }]
}
```

## <a name="set-up-an-ingestion-pipeline-in-azure-data-explorer"></a>Configuración de una canalización de ingesta en Azure Data Explorer

La configuración de una canalización de Azure Data Explorer implica varios pasos, como la [creación de tablas y la ingesta de datos](/azure/data-explorer/ingest-sample-data#ingest-data). También puede manipular, asignar y actualizar los datos.

### <a name="connect-to-the-azure-data-explorer-web-ui"></a>Conexión a la interfaz de usuario web de Azure Data Explorer

En la base de datos *TestDatabase* de Azure Data Explorer, seleccione **Query** (Consultar) para abrir la interfaz de usuario web de Azure Data Explorer.

![Página de consulta](media/ingest-data-no-code/query-database.png)

### <a name="create-the-target-tables"></a>Creación de las tablas de destino

La estructura de los registros de Azure Monitor no es tabular. Podrá manipular los datos y expandir cada evento a uno o más registros. Los datos sin procesar se ingieren en una tabla intermedia denominada *ActivityLogsRawRecords* (registros de actividad) y *DiagnosticLogsRawRecords* (registros de diagnóstico). En ese momento, los datos se manipularán y se expandirán. Con una directiva de actualización, los datos expandidos se introducen en la tabla *ActivityLogsRecords* (registros de actividad) y *DiagnosticLogsRecords* (registros de diagnóstico). Esto significa que deberá crear dos tablas independientes para la ingesta de los registros de actividad y dos tablas independientes para la ingesta de registros de diagnóstico.

Use la interfaz de usuario web de Azure Data Explorer para crear las tablas de destino en la base de datos de Azure Data Explorer.

#### <a name="the-diagnostic-logs-table"></a>Tabla de registros de diagnóstico

1. En la base de datos *TestDatabase*, cree una tabla llamada *DiagnosticLogsRecords* para almacenar los registros de diagnóstico. Use el siguiente comando de control `.create table`:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Seleccione **Ejecutar** para crear la tabla.

    ![Ejecutar consulta](media/ingest-data-no-code/run-query.png)

1. Cree la tabla de datos intermedia llamada *DiagnosticLogsRawRecords* en la base de datos *TestDatabase* para la manipulación de los datos mediante la siguiente consulta. Seleccione **Ejecutar** para crear la tabla.

    ```kusto
    .create table DiagnosticLogsRawRecords (Records:dynamic)
    ```

#### <a name="the-activity-logs-tables"></a>Tablas de registros de actividad

1. Cree una tabla llamada *ActivityLogsRecords* en la base de datos *TestDatabase* para recibir registros de actividad. Para crear la tabla, ejecute la siguiente consulta de Azure Data Explorer:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Cree la tabla de datos intermedia llamada *ActivityLogsRawRecords* en la base de datos *TestDatabase* para la manipulación de los datos:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Creación de asignaciones de tablas

 Como el formato de datos es `json`, se requiere la asignación de datos. La asignación `json` asigna cada ruta de acceso JSON a un nombre de columna de tabla.

#### <a name="table-mapping-for-diagnostic-logs"></a>Asignación de tablas para registros de diagnóstico

Para asignar los datos de los registros de diagnóstico a la tabla, use la siguiente consulta:

```kusto
.create table DiagnosticLogsRawRecords ingestion json mapping 'DiagnosticLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

#### <a name="table-mapping-for-activity-logs"></a>Asignación de tablas para registros de actividad

Para asignar los datos de los registros de actividad a la tabla, use la siguiente consulta:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

### <a name="create-the-update-policy-for-log-data"></a>Creación de la directiva de actualización para los datos de registro

#### <a name="activity-log-data-update-policy"></a>Directiva de actualización de los datos de registro de actividad

1. Cree una [función](/azure/kusto/management/functions) que expanda la colección de registros de actividad para que cada valor de la colección reciba una fila independiente. Use el operador [`mv-expand`](/azure/kusto/query/mvexpandoperator):

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mv-expand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            OperationName = tostring(events["operationName"]),
            Category = tostring(events["category"]),
            ResultType = tostring(events["resultType"]),
            ResultSignature = tostring(events["resultSignature"]),
            DurationMs = toint(events["durationMs"]),
            IdentityAuthorization = events.identity.authorization,
            IdentityClaims = events.identity.claims,
            Location = tostring(events["location"]),
            Level = tostring(events["level"])
    }
    ```

2. Agregue la [directiva de actualización](/azure/kusto/concepts/updatepolicy) a la tabla de destino. Esta directiva ejecuta automáticamente la consulta en cualquier dato recién ingerido en la tabla de datos intermedia *ActivityLogsRawRecords* e ingiere sus resultados en la tabla *ActivityLogsRecords*:

    ```kusto
    .alter table ActivityLogsRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

#### <a name="diagnostic-log-data-update-policy"></a>Directiva de actualización de los datos de registro de diagnóstico

1. Cree una [función](/azure/kusto/management/functions) que expanda la colección de registros de diagnóstico para que cada valor de la colección reciba una fila independiente. Use el operador [`mv-expand`](/azure/kusto/query/mvexpandoperator):
     ```kusto
    .create function DiagnosticLogRecordsExpand() {
        DiagnosticLogsRawRecords
        | mv-expand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            MetricName = tostring(events["metricName"]),
            Count = toint(events["count"]),
            Total = todouble(events["total"]),
            Minimum = todouble(events["minimum"]),
            Maximum = todouble(events["maximum"]),
            Average = todouble(events["average"]),
            TimeGrain = tostring(events["timeGrain"])
    }
    ```

2. Agregue la [directiva de actualización](/azure/kusto/concepts/updatepolicy) a la tabla de destino. Esta directiva ejecuta automáticamente la consulta en cualquier dato recién ingerido en la tabla de datos intermedia *DiagnosticLogsRawRecords* e ingiere sus resultados en la tabla *DiagnosticLogsRecords*:

    ```kusto
    .alter table DiagnosticLogsRecords policy update @'[{"Source": "DiagnosticLogsRawRecords", "Query": "DiagnosticLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-azure-event-hubs-namespace"></a>Creación de un espacio de nombres de Azure Event Hubs

Los registros de diagnóstico de Azure permiten exportar métricas a una cuenta de almacenamiento o a un centro de eventos. En este tutorial, enrutará las métricas mediante un centro de eventos. En los pasos siguientes creará un espacio de nombres de Event Hubs y un centro de eventos para los registros de diagnóstico. Azure Monitor creará el centro de eventos *insights-operational-logs* para los registros de actividad.

1. Cree un centro de eventos mediante una plantilla de Azure Resource Manager en Azure Portal. Para seguir el resto de los pasos de este artículo, haga clic con el botón derecho en el botón **Implementar en Azure** y, luego, seleccione **Abrir en una nueva ventana**. El botón **Deploy to Azure** (Implementar en Azure) le lleva a Azure Portal.

    [![Botón Implementar en Azure](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Cree un espacio de nombres de Event Hubs y un centro de eventos para los registros de diagnóstico.

    ![Creación de un centro de eventos](media/ingest-data-no-code/event-hub.png)

1. Rellene el formulario con la siguiente información. Para el caso de opciones de configuración que no se muestren en la siguiente tabla, use los valores predeterminados.

    **Configuración** | **Valor sugerido** | **Descripción**
    |---|---|---|
    | **Suscripción** | *Su suscripción* | Seleccione la suscripción de Azure que quiere usar para el centro de eventos.|
    | **Grupos de recursos** | *test-resource-group* | Cree un nuevo grupo de recursos. |
    | **Ubicación** | Seleccione la región que mejor se adapte a sus necesidades. | Cree el espacio de nombres de Event Hubs en la misma ubicación que otros recursos.
    | **Nombre del espacio de nombres** | *AzureMonitoringData* | Elija un nombre único que identifique el espacio de nombres.
    | **Nombre del centro de eventos** | *DiagnosticLogsData* | El centro de eventos se encuentra bajo el espacio de nombres, que proporciona un contenedor de ámbito único. |
    | **Nombre del grupo de consumidores** | *adxpipeline* | Cree un nombre para el grupo de consumidores. Los grupos de consumidores permiten consumir varias aplicaciones y que cada una tenga una vista independiente del flujo de eventos. |
    | | |

## <a name="connect-azure-monitor-logs-to-your-event-hub"></a>Conexión de registros de Azure Monitor al centro de eventos

Ahora debe conectar los registros de diagnóstico y los registros de actividad al centro de eventos.

### <a name="connect-diagnostic-logs-to-your-event-hub"></a>Conexión de los registros de diagnóstico al centro de eventos

Seleccione un recurso desde el que se va a exportar las métricas. Hay varios tipos de recursos que admiten la exportación de registros de diagnóstico, incluido el espacio de nombres de Event Hubs, Azure Key Vault, Azure IoT Hub y los clústeres de Azure Data Explorer. En este tutorial, se usará el clúster de Azure Data Explorer como recurso.

1. Seleccione el clúster de Kusto en Azure Portal.
1. Seleccione **Configuración de diagnóstico** y, luego, el vínculo **Activar diagnóstico**. 

    ![Configuración de diagnóstico](media/ingest-data-no-code/diagnostic-settings.png)

1. Se abre el panel **Configuración de diagnóstico**. Siga estos pasos.
   1. Asigne a los datos del registro de diagnóstico el nombre *ADXExportedData*.
   1. En **MÉTRICA**, active la casilla **AllMetrics** (Todas las métricas) (opcional).
   1. Active la casilla **Transmitir a un centro de eventos**.
   1. Seleccione **Configurar**.

      ![Panel Configuración de diagnóstico](media/ingest-data-no-code/diagnostic-settings-window.png)

1. En el panel **Seleccionar centro de eventos**, configure cómo exportar datos desde los registros de diagnóstico hasta el centro de eventos que ha creado:
    1. En la lista **Seleccionar el espacio de nombres del Centro de eventos**, seleccione *AzureMonitoringData*.
    1. En la lista **Seleccionar nombre de centro de eventos**, seleccione *diagnosticlogsdata*.
    1. En la lista **Seleccionar el nombre de directiva del Centro de eventos**, seleccione **RootManagerSharedAccessKey**.
    1. Seleccione **Aceptar**.

1. Seleccione **Guardar**.

### <a name="connect-activity-logs-to-your-event-hub"></a>Conexión de los registros de actividad al centro de eventos

1. En el menú izquierdo de Azure Portal, seleccione **Registro de actividad**.
1. Se abre la ventana **Registro de actividad**. Seleccione **Exportar a Centro de eventos**.

    ![Ventana Registro de actividad](media/ingest-data-no-code/activity-log.png)

1. Se abre la ventana **Exportar registro de actividad**:
 
    ![Ventana Exportar registro de actividad](media/ingest-data-no-code/export-activity-log.png)

1. En la ventana **Exportar registro de actividad**, realice los pasos siguientes:
      1. Seleccione su suscripción.
      1. En la lista **Regiones**, elija **Seleccionar todo**.
      1. Active la casilla **Exportar a un centro de eventos**.
      1. Elija **Select a service bus namespace** (Seleccionar un espacio de nombres del bus de servicio) para abrir el panel **Seleccionar centro de eventos**.
      1. En el panel **Seleccionar centro de eventos**, seleccione su suscripción.
      1. En la lista **Seleccionar el espacio de nombres del Centro de eventos**, seleccione *AzureMonitoringData*.
      1. En la lista **Seleccionar el nombre de directiva del Centro de eventos**, seleccione el nombre de la directiva del centro de eventos predeterminada.
      1. Seleccione **Aceptar**.
      1. En la esquina superior izquierda de la ventana, seleccione **Guardar**.
   Se creará un centro de eventos con el nombre *insights-operational-logs*.

### <a name="see-data-flowing-to-your-event-hubs"></a>Visualización del flujo de datos a los centros de eventos

1. Espere unos minutos hasta que se defina la conexión y haya finalizado la exportación del registro de actividad al centro de eventos. Vaya al espacio de nombres de Event Hubs para ver los centros de eventos que ha creado.

    ![Centros de eventos creados](media/ingest-data-no-code/event-hubs-created.png)

1. Vea el flujo de datos al centro de eventos:

    ![Datos del centro de eventos](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-an-event-hub-to-azure-data-explorer"></a>Conexión de un centro de eventos a Azure Data Explorer

Ahora debe crear las conexiones de datos para sus registros de diagnóstico y registros de actividad.

### <a name="create-the-data-connection-for-diagnostic-logs"></a>Creación de la conexión de datos para los registros de diagnóstico

1. En el clúster de Azure Data Explorer llamado *kustodocs*, seleccione **Bases de datos** en el menú izquierdo.
1. En la ventana **Bases de datos**, seleccione la base de datos *TestDatabase*.
1. En el menú izquierdo, seleccione **Ingesta de datos**.
1. En la ventana **Ingesta de datos**, haga clic en **+ Agregar conexión de datos**.
1. En la ventana **Conexión de datos**, escriba la siguiente información:

    ![Conexión de datos del centro de eventos](media/ingest-data-no-code/event-hub-data-connection.png)

    Origen de datos:

    **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | **Nombre de la conexión de datos** | *DiagnosticsLogsConnection* | Nombre de la conexión que desea crear en el Explorador de datos de Azure.|
    | **Espacio de nombres del centro de eventos** | *AzureMonitoringData* | Nombre elegido anteriormente que identifica el espacio de nombres. |
    | **Centro de eventos** | *diagnosticlogsdata* | El centro de eventos que creó. |
    | **Grupo de consumidores** | *adxpipeline* | Grupo de consumidores de eventos definido en el centro de eventos que creó. |
    | | |

    Tabla de destino:

    Hay dos opciones para el enrutamiento: *estático* y *dinámico*. En este tutorial usará el enrutamiento estático (el predeterminado), en el que se especifican el nombre de la tabla, el formato de los datos y la asignación. Deje **Mis datos incluyen información de enrutamiento** sin seleccionar.

     **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | **Table** | *DiagnosticLogsRawRecords* | La tabla que creó en la base de datos *TestDatabase*. |
    | **Formato de datos** | *JSON* | El formato usado en la tabla. |
    | **Asignación de columnas** | *DiagnosticLogsRecordsMapping* | La asignación que creó en la base de datos *TestDatabase*, que asigna los datos JSON entrantes a los nombres de columna y los tipos de datos de la tabla *DiagnosticLogsRawRecords*.|
    | | |

1. Seleccione **Crear**.  

### <a name="create-the-data-connection-for-activity-logs"></a>Creación de la conexión de datos para los registros de actividad

Repita los pasos de la sección Creación de la conexión de datos para los registros de diagnóstico para crear la conexión de datos para los registros de actividad.

1. Use la configuración siguiente en la ventana **Conexión de datos**:

    Origen de datos:

    **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | **Nombre de la conexión de datos** | *ActivityLogsConnection* | Nombre de la conexión que desea crear en el Explorador de datos de Azure.|
    | **Espacio de nombres del centro de eventos** | *AzureMonitoringData* | Nombre elegido anteriormente que identifica el espacio de nombres. |
    | **Centro de eventos** | *insights-operational-logs* | El centro de eventos que creó. |
    | **Grupo de consumidores** | *$Default* | El grupo de consumidores predeterminado. Si es necesario, puede crear un grupo de consumidores diferente. |
    | | |

    Tabla de destino:

    Hay dos opciones para el enrutamiento: *estático* y *dinámico*. En este tutorial usará el enrutamiento estático (el predeterminado), en el que se especifican el nombre de la tabla, el formato de los datos y la asignación. Deje **Mis datos incluyen información de enrutamiento** sin seleccionar.

     **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | **Table** | *ActivityLogsRawRecords* | La tabla que creó en la base de datos *TestDatabase*. |
    | **Formato de datos** | *JSON* | El formato usado en la tabla. |
    | **Asignación de columnas** | *ActivityLogsRawRecordsMapping* | La asignación que creó en la base de datos *TestDatabase*, que asigna los datos JSON entrantes a los nombres de columna y los tipos de datos de la tabla *ActivityLogsRawRecords*.|
    | | |

1. Seleccione **Crear**.  

## <a name="query-the-new-tables"></a>Consulta de las nuevas tablas

Ahora tiene una canalización con el flujo de datos. La ingesta a través del clúster tarda cinco minutos de forma predeterminada, por lo que los datos fluyen durante unos minutos antes de empezar la consulta.

### <a name="an-example-of-querying-the-diagnostic-logs-table"></a>Ejemplo de consulta en la tabla de registros de diagnóstico

La siguiente consulta analiza los datos de duración de la consulta del registro de diagnóstico de Azure Data Explorer:

```kusto
DiagnosticLogsRecords
| where Timestamp > ago(15m) and MetricName == 'QueryDuration'
| summarize avg(Average)
```

Resultados de la consulta:

|   |   |
| --- | --- |
|   |  avg_Average |
|   | 00:06.156 |
| | |

### <a name="an-example-of-querying-the-activity-logs-table"></a>Ejemplo de consulta de la tabla de registros de actividad

La siguiente consulta analiza los datos del registro de actividad de Azure Data Explorer:

```kusto
ActivityLogsRecords
| where OperationName == 'MICROSOFT.EVENTHUB/NAMESPACES/AUTHORIZATIONRULES/LISTKEYS/ACTION'
| where ResultType == 'Success'
| summarize avg(DurationMs)
```

Resultados de la consulta:

|   |   |
| --- | --- |
|   |  avg(DurationMs) |
|   | 768,333 |
| | |

## <a name="next-steps"></a>Pasos siguientes

Aprenda a escribir muchas más consultas de los datos que extrajo de Azure Data Explorer en el artículo siguiente:

> [!div class="nextstepaction"]
> [Write queries for Azure Data Explorer](write-queries.md) (Escritura de consultas del Explorador de datos de Azure)
