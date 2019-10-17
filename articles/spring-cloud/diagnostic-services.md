---
title: Análisis de registros y métricas en Azure Spring Cloud | Microsoft Docs
description: Aprenda a analizar datos de diagnóstico en Azure Spring Cloud
services: spring-cloud
author: jpconnock
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: e6b90fb09c536f68bee7fd5d57507fe3920bcf1e
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038719"
---
# <a name="analyze-logs-and-metrics-with-diagnostic-settings"></a>Análisis de registros y métricas con la configuración de diagnóstico

La funcionalidad de diagnóstico de Azure Spring Cloud permite analizar los registros y las métricas mediante uno de los siguientes servicios:

* Analícelos con Azure Log Analytics, donde los datos se escriben inmediatamente en Azure Log Analytics sin necesidad de escribir primero los datos en el almacenamiento.
* Guárdelos en una cuenta de almacenamiento para auditarlos o para inspeccionarlos manualmente. Puede especificar el tiempo de retención (en días).
* Transmítalos a Event Hubs para la ingesta en un servicio de terceros o una solución de análisis personalizado.

Para empezar, necesitará habilitar uno de estos servicios para recibir los datos.  Para aprender a configurar Log Analytics, consulte [este tutorial](../azure-monitor/log-query/get-started-portal.md).  

## <a name="configure-diagnostic-settings"></a>Configuración del diagnóstico

1. Vaya a la instancia de Azure Spring Cloud en Azure Portal.
1. Seleccione la opción de menú **Configuración de diagnóstico**.
1. Seleccione el botón **Agregar configuración de diagnóstico**.
1. Escriba un nombre para la configuración y elija dónde desea enviar los registros. También puede seleccionar cualquier combinación de las tres opciones siguientes:
    * Archivar en una cuenta de almacenamiento
    * Transmitir a un centro de eventos
    * Enviar a Log Analytics

1. Elija la categoría de registro y de métrica que desea supervisar y especifique el tiempo de retención (en días). El tiempo de retención solo se aplica a la cuenta de almacenamiento.
1. Seleccione **Guardar** para aplicar el cambio.

> [!NOTE]
> Pueden pasar hasta 15 minutos desde que se emiten los registros o las métricas hasta que aparecen en la cuenta de Storage/Event Hubs/Log Analytics.

## <a name="viewing-logs"></a>Visualización de los registros

### <a name="using-log-analytics"></a>Uso de Log Analytics

1. En Azure Portal, seleccione Log Analytics en el menú de navegación izquierdo.
1. Seleccione el área de trabajo Log Analytics que eligió al agregar la configuración de diagnóstico.
1. Seleccione `Logs` para abrir la hoja Búsqueda de registros.
1. Escriba una consulta simple en el cuadro Búsqueda de registros.  Por ejemplo:

    ```sql
    AppPlatformLogsforSpring
    | limit 50
    ```

1. Seleccione `Run` para ver el resultado de búsqueda.
1. Puede buscar los registros de la aplicación o instancia específicas con una condición de filtro:

    ```sql
    AppPlatformLogsforSpring
    | where ServiceName == "YourServiceName" and AppName == "YourAppName" and InstanceName == "YourInstanceName"
    | limit 50
    ```

Más información sobre el lenguaje de consulta que se usa en Log Analytics [en este artículo](../azure-monitor/log-query/query-language.md)

### <a name="using-logs-and-metrics-in-storage-account"></a>Uso de registros y métricas en una cuenta de almacenamiento

1. En Azure Portal, seleccione Cuentas de almacenamiento en el menú de navegación izquierdo.
1. Seleccione la cuenta de almacenamiento que eligió al agregar la configuración de diagnóstico.
1. Seleccione la entrada `Blobs` para abrir la hoja Contenedor de blobs.
1. Busque un contenedor llamado `insights-logs-applicationconsole` para revisar los registros de la aplicación.
1. Busque un contenedor llamado `insights-metrics-pt1m` para revisar las métricas de la aplicación.

[Más información sobre el envío de información de diagnóstico a una cuenta de almacenamiento.](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-to-storage)

### <a name="using-event-hubs"></a>Uso de Event Hubs

1. En Azure Portal, seleccione Event Hubs en el panel de navegación izquierdo.
1. Busque y seleccione la instancia de Event Hubs que eligió al agregar la configuración de diagnóstico.
1. Seleccione `Event Hubs` para abrir la hoja Event Hub List (Lista de centros de eventos).
1. Busque un centro de eventos llamado `insights-logs-applicationconsole` para revisar los registros de la aplicación.
1. Busque un centro de eventos llamado `insights-metrics-pt1m` para revisar las métricas de la aplicación.

[Más información sobre el envío de información de diagnóstico a un centro de eventos](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-stream-event-hubs).

## <a name="analyzing-logs"></a>Análisis de datos

Azure Log Analytics proporciona Kusto para que pueda consultar los registros para el análisis.  Revise el [tutorial de Log Analytics](../azure-monitor/log-query/get-started-portal.md) para una introducción rápida a la consulta de registros con Kusto.

Los registros de aplicaciones proporcionan información crítica sobre el estado de la aplicación, el rendimiento y mucho más.  A continuación se muestran algunas consultas sencillas que le ayudarán a comprender los estados actual y anterior de la aplicación.

### <a name="show-application-logs-from-azure-spring-cloud"></a>Visualización de los registros de aplicaciones de Azure Spring Cloud

Para revisar una lista de los registros de aplicaciones de Azure Spring Cloud ordenados por tiempo donde los más recientes se muestran en primer lugar:

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| sort by TimeGenerated desc
```

### <a name="show-logs-entries-containing-errors-or-exceptions"></a>Visualización de las entradas de registro con errores o excepciones

Esta consulta permite revisar las entradas de registro que mencionan un error o una excepción.  Los resultados no se ordenan.

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| where Log contains "error" or Log contains "exception"
```

Use esta consulta para buscar errores o modificar los términos de la consulta para encontrar códigos de error específicos o excepciones.  

### <a name="show-the-number-of-errors-and-exceptions-reported-by-your-application-over-the-last-hour"></a>Visualización del número de errores y excepciones que ha notificado la aplicación durante la última hora

Esta consulta crea un gráfico circular que muestra el número de errores y excepciones registrados por la aplicación en la última hora:

```sql
AppPlatformLogsforSpring
| where TimeGenerated > ago(1h)
| where Log contains "error" or Log contains "exception"
| summarize count_per_app = count() by AppName
| sort by count_per_app desc
| render piechart
```

### <a name="learn-more-about-querying-application-logs"></a>Más información sobre la consulta de registros de aplicaciones

Azure Monitor proporciona buen soporte técnico para consultar los registros de aplicaciones mediante Log Analytics.  Para más información sobre este servicio, revise el tutorial sobre [consultas de registro](../azure-monitor/log-query/get-started-queries.md) mediante Azure Monitor. [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/log-query/log-query-overview.md) proporciona más información sobre la compilación de consultas para analizar los registros de aplicaciones.
