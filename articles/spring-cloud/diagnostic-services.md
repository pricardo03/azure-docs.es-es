---
title: Análisis de registros y métricas en Azure Spring Cloud | Microsoft Docs
description: Aprenda a analizar datos de diagnóstico en Azure Spring Cloud
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: ebe438bd2dc5b4921ce733001f3c9df19bc592fe
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607856"
---
# <a name="analyze-logs-and-metrics-with-diagnostics-settings"></a>Análisis de registros y métricas con la configuración de diagnóstico

Mediante la funcionalidad de diagnóstico de Azure Spring Cloud puede analizar los registros y las métricas con uno de los siguientes servicios:

* Use Azure Log Analytics, donde los datos se escriben inmediatamente sin tener que escribirlos primero en el almacenamiento.
* Guárdelos en una cuenta de almacenamiento para auditarlos o para inspeccionarlos manualmente. Puede especificar el tiempo de retención (en días).
* Transmítalos al centro de eventos para la ingesta en un servicio de terceros o una solución de análisis personalizada.

Para empezar, habilite uno de estos servicios para recibir los datos. Para más información sobre la configuración de Log Analytics, consulte [Introducción a los análisis de registros de Azure Monitor](../azure-monitor/log-query/get-started-portal.md). 

## <a name="configure-diagnostics-settings"></a>Configuración de valores de diagnóstico

1. En Azure Portal, vaya a la instancia de Azure Spring Cloud.
1. Seleccione la opción **Configuración de diagnóstico** y, después, seleccione **Agregar configuración de diagnóstico**.
1. Escriba un nombre para la configuración y elija dónde desea enviar los registros. También puede seleccionar cualquier combinación de las tres opciones siguientes:
    * **Archivar en una cuenta de almacenamiento**
    * **Transmisión a un centro de eventos**
    * **Enviar a Log Analytics**

1. Elija la categoría de registro y de métrica que desea supervisar y especifique el tiempo de retención (en días). El tiempo de retención solo se aplica a la cuenta de almacenamiento.
1. Seleccione **Guardar**.

> [!NOTE]
> Puede transcurrir un intervalo de hasta 15 minutos desde que se emiten los registros o las métricas hasta que aparecen en la cuenta de almacenamiento, el centro de eventos o Log Analytics.

## <a name="view-the-logs"></a>Visualización de los registros

### <a name="use-log-analytics"></a>Uso de Log Analytics

1. En Azure Portal, en el panel izquierdo, seleccione **Log Analytics**.
1. Seleccione el área de trabajo de Log Analytics que eligió al agregar la configuración de diagnóstico.
1. Para abrir el panel **Búsqueda de registros**, seleccione **Registros**.
1. En el cuadro Búsqueda de **registros**, escriba una consulta simple como:

    ```sql
    AppPlatformLogsforSpring
    | limit 50
    ```

1. Para ver el resultado de la búsqueda, seleccione **Ejecutar**.
1. Puede buscar los registros de la aplicación o instancia específicas con una condición de filtro:

    ```sql
    AppPlatformLogsforSpring
    | where ServiceName == "YourServiceName" and AppName == "YourAppName" and InstanceName == "YourInstanceName"
    | limit 50
    ```

Para más información sobre el lenguaje de consulta que se utiliza en Log Analytics, consulte [Consultas de registros de Azure Monitor](../azure-monitor/log-query/query-language.md).

### <a name="use-your-storage-account"></a>Uso de la cuenta de almacenamiento 

1. En Azure Portal, en el panel izquierdo, seleccione **Cuentas de almacenamiento**.

1. Seleccione la cuenta de almacenamiento que eligió al agregar la configuración de diagnóstico.
1. Para abrir el panel **Contenedor de blobs**, seleccione **Blobs**.
1. Para revisar los registros de aplicaciones, busque un contenedor llamado **insights-logs-applicationconsole**.
1. Para revisar las métricas de aplicaciones, busque un contenedor llamado **insights-metrics-pt1m**.

Para más información sobre cómo enviar información de diagnóstico a una cuenta de almacenamiento, consulte [Almacenamiento y visualización de los datos de diagnóstico en Azure Storage](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-to-storage).

### <a name="use-your-event-hub"></a>Uso del centro de eventos

1. En Azure Portal, en el panel izquierdo, seleccione **Centros de eventos**.

1. Busque y seleccione el centro de eventos que eligió al agregar la configuración de diagnóstico.
1. Para abrir el panel **Event Hub List** (Lista de centros de eventos), seleccione **Centros de eventos**.
1. Para revisar los registros de aplicaciones, busque un centro de eventos llamado **insights-logs-applicationconsole**.
1. Para revisar las métricas de aplicaciones, busque un centro de eventos llamado **insights-metrics-pt1m**.

Para más información sobre cómo enviar información de diagnóstico a un centro de eventos, consulte [Transmisión de datos de Azure Diagnostics en la ruta de acceso activa mediante Event Hubs](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-stream-event-hubs).

## <a name="analyze-the-logs"></a>Análisis de los registros

Azure Log Analytics proporciona Kusto para que pueda consultar los registros para el análisis. Revise el [tutorial de Log Analytics](../azure-monitor/log-query/get-started-portal.md) para una introducción rápida a la consulta de registros con Kusto.

Los registros de aplicaciones proporcionan información crítica sobre el estado de la aplicación, el rendimiento y mucho más. En las siguientes secciones se muestran algunas consultas sencillas que le ayudarán a comprender los estados actual y anterior de la aplicación.

### <a name="show-application-logs-from-azure-spring-cloud"></a>Visualización de los registros de aplicaciones de Azure Spring Cloud

Para revisar una lista de los registros de aplicaciones de Azure Spring Cloud ordenados por tiempo donde los más recientes se muestran en primer lugar, ejecute la siguiente consulta:

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| sort by TimeGenerated desc
```

### <a name="show-logs-entries-containing-errors-or-exceptions"></a>Visualización de las entradas de registro con errores o excepciones

Para revisar las entradas de registro sin ordenar que mencionan un error o una excepción, ejecute la siguiente consulta:

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| where Log contains "error" or Log contains "exception"
```

Use esta consulta para buscar errores o modificar los términos de la consulta para encontrar códigos de error específicos o excepciones. 

### <a name="show-the-number-of-errors-and-exceptions-reported-by-your-application-over-the-last-hour"></a>Visualización del número de errores y excepciones que ha notificado la aplicación durante la última hora

Para crear un gráfico circular que muestre el número de errores y excepciones registrados por la aplicación en la última hora, ejecute la siguiente consulta:

```sql
AppPlatformLogsforSpring
| where TimeGenerated > ago(1h)
| where Log contains "error" or Log contains "exception"
| summarize count_per_app = count() by AppName
| sort by count_per_app desc
| render piechart
```

### <a name="learn-more-about-querying-application-logs"></a>Más información sobre la consulta de registros de aplicaciones

Azure Monitor proporciona buen soporte técnico para consultar los registros de aplicaciones mediante Log Analytics. Para más información sobre este servicio, consulte [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/log-query/get-started-queries.md). Para más información sobre la compilación de consultas para analizar los registros de aplicaciones, consulte [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/log-query/log-query-overview.md).
