---
title: 'Tutorial: Ingesta de datos de diagnóstico y de registro de actividad en Azure Data Explorer sin una línea de código'
description: En este tutorial, obtendrá información sobre cómo introducir datos en Azure Data Explorer sin una línea de código y consultar datos.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 2/5/2019
ms.openlocfilehash: 145a56bee857debdbf028834a3ed378efd8671c8
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447504"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Tutorial: Ingesta de datos en Azure Data Explorer sin una línea de código

Este tutorial le enseñará cómo ingerir datos de diagnóstico y de registro de actividad para un clúster de Azure Data Explorer sin una línea de código. Este método de ingesta simple le permite empezar a consultar datos de Azure Data Explorer rápidamente para el análisis de datos.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Cree la asignación de la ingesta y las tablas en una base de datos de Azure Data Explorer.
> * Dé formato a los datos ingeridos mediante una directiva de actualización.
> * Cree un [centro de eventos](/azure/event-hubs/event-hubs-about) y conéctelo a Azure Data Explorer.
> * Transmita los datos a un centro de eventos desde los [registros de diagnóstico de Azure Monitor](/azure/azure-monitor/platform/diagnostic-logs-overview) y los [registros de actividad de Azure Monitor](/azure/azure-monitor/platform/activity-logs-overview).
> * Consulte los datos ingeridos mediante Azure Data Explorer.

> [!NOTE]
> Cree todos los recursos en la misma ubicación o región de Azure. Esto es un requisito para los registros de diagnóstico de Azure Monitor.

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* [Un clúster y la base de datos de Azure Data Explorer](create-cluster-database-portal.md). En este tutorial, el nombre de la base de datos es *AzureMonitoring*.

## <a name="azure-monitoring-data-provider---diagnostic-and-activity-logs"></a>Diagnóstico y registros de actividad del proveedor de datos de supervisión de Azure

Vea y conozca los datos proporcionados por los registros de diagnóstico y actividad de la supervisión de Azure. Se creará una canalización de ingesta en función de estos esquemas de datos.

### <a name="diagnostic-logs-example"></a>Ejemplo de los registros de diagnóstico

Los registros de diagnóstico de Azure son emitidos por un servicio de Azure que proporciona datos sobre el funcionamiento de dicho servicio. Los datos se agregan con un intervalo de agregación de 1 minuto. Cada evento de los registros de diagnóstico contiene un registro. Lo siguiente es un ejemplo de un esquema de eventos de métricas de Azure Data Explorer, según la duración de la consulta:

```json
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
}
```

### <a name="activity-logs-example"></a>Registros de actividad de ejemplo

Los registros de actividad de Azure son registros de nivel de suscripción que contienen una colección de registros. Proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. A diferencia de los registros de diagnóstico, un evento de los registros de actividad tiene una matriz de registros. Tendremos que dividir esta matriz de registros más adelante en el tutorial. Este es un ejemplo de un evento del registro de actividad para la comprobación de acceso:

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

## <a name="set-up-ingestion-pipeline-in-azure-data-explorer"></a>Configuración de la canalización de ingesta en Azure Data Explorer 

La configuración de la canalización de Azure Data Explorer contiene varios pasos que incluyen la [ingesta de datos y la creación de tablas](/azure/data-explorer/ingest-sample-data#ingest-data). También puede manipular, asignar y actualizar los datos.

### <a name="connect-to-azure-data-explorer-web-ui"></a>Conexión a la UI de Azure Data Explorer

1. En la base de datos *AzureMonitoring* de Azure Data Explorer, seleccione **Consulta**, con lo que se abrirá la interfaz de usuario web de Azure Data Explorer.

    ![Consultar](media/ingest-data-no-code/query-database.png)

### <a name="create-target-tables"></a>Creación de tablas de destino

Use la interfaz de usuario web de Azure Data Explorer para crear las tablas de destino en la base de datos de Azure Data Explorer.

#### <a name="diagnostic-logs-table"></a>Tabla de registros de diagnóstico

1. Cree una tabla *DiagnosticLogsRecords* en la base de datos *AzureMonitoring* que recibirá los registros del registro de diagnóstico mediante el comando de control `.create table`:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Seleccione **Ejecutar** para crear la tabla.

    ![Ejecución de la consulta](media/ingest-data-no-code/run-query.png)

#### <a name="activity-logs-tables"></a>Tablas de los registros de actividad

Puesto que la estructura de los registros de actividad no es tabular, tendrá que manipular los datos y expandir cada evento a uno o varios registros. Los datos sin procesar se ingerirán en una tabla intermedia *ActivityLogsRawRecords*. En ese momento, los datos se manipularán y se expandirán. Los datos expandidos entonces se ingerirán en la tabla *ActivityLogsRecords* mediante una directiva de actualización. Por lo tanto, deberá crear dos tablas independientes para la ingesta de registros de actividad.

1. Cree una tabla *ActivityLogsRecords* en la base de datos *AzureMonitoring* que vaya a recibir entradas del registro de actividad. Ejecute la siguiente consulta de Azure Data Explorer para crear la tabla:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Cree la tabla de datos intermedios *ActivityLogsRawRecords* en la base de datos *AzureMonitoring* para la manipulación de datos:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Creación de asignaciones de tablas

 El formato de datos es `json`, por lo tanto, se requiere la asignación de datos. La asignación `json` asigna cada ruta de acceso JSON a un nombre de columna de tabla.

#### <a name="diagnostic-logs-table-mapping"></a>Asignación de la tabla de registros de diagnóstico

Para asignar los datos a la tabla, utilice la siguiente consulta:

```kusto
.create table DiagnosticLogsRecords ingestion json mapping 'DiagnosticLogsRecordsMapping' '[{"column":"Timestamp","path":"$.time"},{"column":"ResourceId","path":"$.resourceId"},{"column":"MetricName","path":"$.metricName"},{"column":"Count","path":"$.count"},{"column":"Total","path":"$.total"},{"column":"Minimum","path":"$.minimum"},{"column":"Maximum","path":"$.maximum"},{"column":"Average","path":"$.average"},{"column":"TimeGrain","path":"$.timeGrain"}]'
```

#### <a name="activity-logs-table-mapping"></a>Asignación de la tabla de registros de actividad

Para asignar los datos a la tabla, utilice la siguiente consulta:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

### <a name="create-update-policy"></a>Creación de la directiva de actualización

1. Cree una [función](/azure/kusto/management/functions) que expanda la colección de registros para que cada valor de la colección reciba una fila independiente. Use el operador [`mvexpand`](/azure/kusto/query/mvexpandoperator):

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mvexpand events = Records
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

2. Agregue una [directiva de actualización](/azure/kusto/concepts/updatepolicy) a la tabla de destino. Se ejecutará automáticamente la consulta en cualquier dato recién ingerido en la tabla de datos intermedia *ActivityLogsRawRecords* e ingerirá sus resultados en la tabla *ActivityLogsRecords*:

    ```kusto
    .alter table ActivityLogsRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-event-hub-namespace"></a>Creación de un espacio de nombres del centro de eventos

Los registros de diagnóstico de Azure permiten exportar métricas a una cuenta de Storage o un centro de eventos. En este tutorial, se enrutan las métricas a través de un centro de eventos. Va a crear un centro de eventos y un espacio de nombres Event Hubs para registros de diagnóstico en los pasos siguientes. Supervisión de Azure creará el centro de eventos *insights-operational-logs* para los registros de actividad.

1. Cree un centro de eventos mediante una plantilla de Azure Resource Manager en Azure Portal. Use el botón siguiente para iniciar la implementación. Haga clic con el botón derecho y seleccione **Abrir en una ventana nueva** para seguir el resto de los pasos de este artículo. El botón **Deploy to Azure** (Implementar en Azure) le lleva a Azure Portal.

    [![Implementación en Azure](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Cree un espacio de nombres del centro de eventos y un centro de eventos para los registros de diagnóstico.

    ![Creación de un centro de eventos](media/ingest-data-no-code/event-hub.png)

    Rellene el formulario con la siguiente información. Use los valores predeterminados para cualquier configuración que no aparezca en la tabla siguiente.

    **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | Subscription | Su suscripción | Seleccione la suscripción de Azure que quiere usar para el centro de eventos.|
    | Grupos de recursos | *test-resource-group* | Cree un nuevo grupo de recursos. |
    | Ubicación | Seleccione la región que mejor se adapte a sus necesidades. | Cree el espacio de nombres del centro de eventos en la misma ubicación que otros recursos.
    | Nombre del espacio de nombres | *AzureMonitoringData* | Elija un nombre único que identifique el espacio de nombres.
    | Nombre del centro de eventos | *DiagnosticLogsData* | El centro de eventos se encuentra bajo el espacio de nombres, que proporciona un contenedor de ámbito único. |
    | Nombre del grupo de consumidores | *adxpipeline* | Cree un nombre para el grupo de consumidores. Permite que varias aplicaciones de consumo tengan cada una una vista independiente del flujo de eventos. |
    | | |

## <a name="connect-azure-monitoring-logs-to-event-hub"></a>Conexión de los registros de supervisión de Azure al centro de eventos

### <a name="diagnostic-logs-connection-to-event-hub"></a>Conexión de los registros de diagnóstico al centro de eventos

Seleccione un recurso desde el que se va a exportar las métricas. Hay varios tipos de recursos que permiten exportar los registros de diagnóstico incluidos Event Hub Namespace, KeyVault, IoT Hub y el clúster de Azure Data Explorer. En este tutorial, usamos el clúster de Azure Data Explorer como nuestro recurso.

1. Seleccione el clúster de Kusto en Azure Portal.

    ![Configuración de diagnóstico](media/ingest-data-no-code/diagnostic-settings.png)

1. Seleccione **Configuración de diagnóstico** en el menú de la izquierda.
1. Haga clic en el vínculo **Activar diagnóstico**. Se abre la ventana **Configuración de diagnóstico**.

    ![Ventana Configuración de diagnóstico](media/ingest-data-no-code/diagnostic-settings-window.png)

1. En el panel **Configuración de diagnóstico.**:
    1. Denomine los datos del registro de diagnóstico: *ADXExportedData*
    1. Seleccione la casilla **AllMetrics** (opcional).
    1. Seleccione la casilla **Transmitir a un centro de eventos**.
    1. Haga clic en **Configurar**.

1. En el panel **Seleccionar centro de eventos**, configure la exportación al centro de eventos que creó:
    1. **Seleccionar el espacio de nombres del Centro de eventos** *AzureMonitoringData* desde la lista desplegable.
    1. **Seleccionar nombre de centro de eventos** *diagnosticlogsdata* desde la lista desplegable.
    1. **Seleccionar el nombre de directiva del Centro de eventos** desde la lista desplegable.
    1. Haga clic en **OK**.

1. Haga clic en **Save**(Guardar). En la ventana aparecerán el espacio de nombres del centro de eventos, el nombre y el nombre de la directiva.

    ![Guardar la configuración de diagnóstico](media/ingest-data-no-code/save-diagnostic-settings.png)

### <a name="activity-logs-connection-to-event-hub"></a>Conexión de los registros de actividad al centro de eventos

1. En el menú izquierdo de Azure Portal, seleccione **Registro de actividad**.
1. Se abre la ventana **Registro de actividad**. **Haga clic en Exportar a Centro de eventos**.

    ![Registro de actividades](media/ingest-data-no-code/activity-log.png)

1. En la ventana **Exportar registro de actividad**:
 
    ![Exportar registro de actividad](media/ingest-data-no-code/export-activity-log.png)

    1. Seleccione su suscripción.
    1. En la lista desplegable **Regiones**, elija **Seleccionar todo**
    1. Seleccione la casilla **Exportar a un centro de eventos**.
    1. Haga clic en **Select a service bus namespace** (Seleccionar un espacio de nombres del bus de servicio) para abrir el panel **Seleccionar centro de eventos**.
    1. En el panel **Seleccionar centro de eventos**, seleccione en los menús desplegables su suscripción, el espacio de nombres de evento *AzureMonitoringData* y el nombre de la directiva del centro de eventos predeterminada.
    1. Haga clic en **OK**.
    1. Haga clic en **Guardar** en la parte superior derecha de la ventana. Se creará un centro de eventos con el nombre *insights-operational-logs*.

### <a name="see-data-flowing-to-your-event-hubs"></a>Visualización de los datos que fluyen a Event Hubs

1. Espere unos minutos hasta que se defina la conexión y se complete la exportación del registro de actividad al centro de eventos. Vaya al espacio de nombres del centro de eventos para ver los centros de eventos que creó.

    ![Event Hubs creado](media/ingest-data-no-code/event-hubs-created.png)

1. Vea los datos que pasan al centro de eventos:

    ![Datos de Event Hubs](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-event-hub-to-azure-data-explorer"></a>Conexión del centro de eventos a Azure Data Explorer

### <a name="diagnostic-logs-data-connection"></a>Conexión de datos de los registros de diagnóstico

1. En el clúster de Azure Data Explorer *kustodocs*, seleccione **Bases de datos** en el menú izquierdo.
1. En la ventana **Bases de datos**, seleccione el nombre de la base de datos *AzureMonitoring*
1. En el menú de la izquierda, seleccione **Ingesta de datos**.
1. En la ventana **Ingesta de datos**, haga clic en **+ Agregar conexión de datos**
1. En la ventana **Conexión de datos**, escriba la siguiente información:

    ![Conexión del centro de eventos](media/ingest-data-no-code/event-hub-data-connection.png)

    Origen de datos:

    **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | Nombre de la conexión de datos | *DiagnosticsLogsConnection* | Nombre de la conexión que desea crear en el Explorador de datos de Azure.|
    | Espacio de nombres del centro de eventos | *AzureMonitoringData* | Nombre elegido anteriormente que identifica el espacio de nombres. |
    | Centro de eventos | *diagnosticlogsdata* | El centro de eventos que creó. |
    | Grupo de consumidores | *adxpipeline* | Grupo de consumidores de eventos definido en el centro de eventos que creó. |
    | | |

    Tabla de destino:

    Hay dos opciones para el enrutamiento: *estático* y *dinámico*. En este tutorial, usará el enrutamiento estático (el predeterminado), en el que se especifican el nombre de la tabla, el formato de archivo y la asignación. Por tanto, deje **My data includes routing info** (Mis datos incluyen información de enrutamiento) sin seleccionar.

     **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | Tabla | *DiagnosticLogsRecords* | La tabla que creó en la base de datos *AzureMonitoring*. |
    | Formato de datos | *JSON* | Formato de tabla. |
    | Asignación de columnas | *DiagnosticLogsRecordsMapping* | La asignación que creó en la base de datos *AzureMonitoring*, que asigna los datos JSON entrantes a los nombres de columna los y tipos de datos de *DiagnosticLogsRecords*.|
    | | |

1. Haga clic en **Crear**  

### <a name="activity-logs-data-connection"></a>Conexión de datos de los registros de actividad

Repita los pasos de la sección de [conexión de datos de registros de diagnóstico](#diagnostic-logs-data-connection) para crear la conexión de datos de los registros de actividad.

1. Inserte la configuración siguiente en la ventana **Conexión de datos**:

    Origen de datos:

    **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | Nombre de la conexión de datos | *ActivityLogsConnection* | Nombre de la conexión que desea crear en el Explorador de datos de Azure.|
    | Espacio de nombres del centro de eventos | *AzureMonitoringData* | Nombre elegido anteriormente que identifica el espacio de nombres. |
    | Centro de eventos | *insights-operational-logs* | El centro de eventos que creó. |
    | Grupo de consumidores | *$Default* | El grupo de consumidores predeterminado. Si es necesario, puede crear un grupo de consumidores diferente. |
    | | |

    Tabla de destino:

    Hay dos opciones para el enrutamiento: *estático* y *dinámico*. En este tutorial, usará el enrutamiento estático (el predeterminado), en el que se especifican el nombre de la tabla, el formato de archivo y la asignación. Por tanto, deje **My data includes routing info** (Mis datos incluyen información de enrutamiento) sin seleccionar.

     **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | Tabla | *ActivityLogsRawRecords* | La tabla que creó en la base de datos *AzureMonitoring*. |
    | Formato de datos | *JSON* | Formato de tabla. |
    | Asignación de columnas | *ActivityLogsRawRecordsMapping* | La asignación que creó en la base de datos *AzureMonitoring*, que asigna los datos JSON entrantes a los nombres de columna y los tipos de datos de *ActivityLogsRawRecords*.|
    | | |

1. Haga clic en **Crear**  

## <a name="query-the-new-tables"></a>Consulta de las nuevas tablas

Tiene una canalización con el flujo de datos. La ingesta a través del clúster tarda cinco minutos, de forma predeterminada, por lo que los datos fluyen durante unos minutos antes de empezar la consulta.

### <a name="diagnostic-logs-table-query-example"></a>Ejemplo de consulta de la tabla de registros de diagnóstico

La siguiente consulta analiza los datos de duración de la consulta de los registros del registro de diagnóstico de Azure Data Explorer:

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

### <a name="activity-logs-table-query-example"></a>Ejemplo de consulta de la tabla de registros de actividad

La siguiente consulta analiza los datos de los registros del registro de actividad de Azure Data Explorer:

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
> [Escritura de consultas para Azure Data Explorer](write-queries.md)
