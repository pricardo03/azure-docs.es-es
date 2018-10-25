---
title: Supervisión y ajuste en Azure Database for PostgreSQL
description: En este artículo se describen las características de supervisión y ajuste en Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 0794c1573c2eaa951b805573cf33f05923da83b4
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867789"
---
# <a name="monitor-and-tune"></a>Supervisión y ajuste
La supervisión de los datos sobre los servidores le permite solucionar problemas y optimizar la carga de trabajo. Azure Database for PostgreSQL proporciona diversas opciones de supervisión para ofrecer información detallada sobre el comportamiento del servidor.

## <a name="metrics"></a>Métricas
Azure Database for PostgreSQL proporciona diversas métricas que proporcionan información sobre el comportamiento de los recursos que admiten el servidor PostgreSQL. Cada métrica se emite con una frecuencia de un minuto y tiene un historial de hasta 30 días. Puede configurar alertas en las métricas. Para obtener instrucciones paso a paso, consulte [How to set up alerts](howto-alert-on-metric.md) (Configuración de alertas). Otras tareas incluyen la configuración de acciones automatizadas, la realización de análisis avanzados y el archivo del historial. Para obtener más información, consulte [Información general sobre las métricas en Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lista de métricas
Estas métricas están disponibles para Azure Database for PostgreSQL:

|Métrica|Nombre de métrica para mostrar|Unidad|DESCRIPCIÓN|
|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Porcentaje de CPU en uso.|
|memory_percent|Porcentaje de memoria|Percent|Porcentaje de memoria en uso.|
|io_consumption_percent|Porcentaje de E/S|Percent|Porcentaje de E/S en uso.|
|storage_percent|Porcentaje de almacenamiento|Percent|Porcentaje de almacenamiento que se usa más allá del límite máximo del servidor.|
|storage_used|Almacenamiento utilizado|Bytes|Cantidad de almacenamiento en uso. El almacenamiento que usa el servicio puede incluir los archivos de base de datos, los registros de transacciones y los registros de servidor.|
|storage_limit|Límite de almacenamiento|Bytes|Almacenamiento máximo de este servidor.|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|El porcentaje usado del almacenamiento máximo de registro del servidor.|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|La cantidad de almacenamiento de registro del servidor en uso.|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|El almacenamiento máximo de registro de este servidor.|
|active_connections|Conexiones activas|Recuento|Número de conexiones activas al servidor.|
|connections_failed|Conexiones con errores|Recuento|Número de conexiones con errores al servidor.|
|network_bytes_egress|Red interna|Bytes|Red externa a través de conexiones activas.|
|network_bytes_ingress|Red interna|Bytes|Red interna a través de conexiones activas.|

## <a name="server-logs"></a>Registros del servidor
Puede habilitar el registro en el servidor. Estos registros también están disponibles mediante los registros de diagnóstico de Azure en [Log Analytics](../log-analytics/log-analytics-queries.md), Event Hubs y la cuenta de almacenamiento. Para más información sobre el registro, visite la página [Registros de servidor](concepts-server-logs.md).

## <a name="query-store"></a>Almacén de consultas
[Almacén de consultas](concepts-query-store.md) es una característica en versión preliminar pública que realiza un seguimiento del rendimiento de las consultas a lo largo del tiempo, lo que incluye estadísticas en tiempo de ejecución y eventos de espera de consultas. La característica conserva información del rendimiento en tiempo de ejecución de las consultas en una base de datos del sistema denominada **azure_sys** bajo el esquema query_store. Puede controlar la recopilación y el almacenamiento de datos mediante diversos botones de configuración.

## <a name="query-performance-insight"></a>Información de rendimiento de consultas
[Información de rendimiento de consultas](concepts-query-performance-insight.md) funciona en combinación con el Almacén de consultas para proporcionar visualizaciones accesibles desde Azure Portal. Estos gráficos le permiten identificar las consultas clave que afectan al rendimiento. Información de rendimiento de consultas está en versión preliminar pública y es accesible en la sección **Support + troubleshooting** (Soporte y solución de problemas) de la página del portal del servidor de Azure Database for PostgreSQL.

## <a name="performance-recommendations"></a>Recomendaciones de rendimiento
La característica [Recomendaciones de rendimiento](concepts-performance-recommendations.md) identifica oportunidades para mejorar el rendimiento de la carga de trabajo. La versión preliminar pública de Recomendaciones de rendimiento le proporciona recomendaciones para crear nuevos índices que tienen el potencial de mejorar el rendimiento de las cargas de trabajo. Para generar recomendaciones de índices, la característica tiene en cuenta diversas funcionalidades de la base de datos, como su esquema y la carga de trabajo devueltos por el Almacén de consultas. Después de implementar cualquier recomendación de rendimiento, los clientes deben probar el rendimiento para evaluar el efecto de estos cambios. 

## <a name="next-steps"></a>Pasos siguientes
- Consulte [Configuración de alertas](howto-alert-on-metric.md) para obtener instrucciones sobre cómo crear una alerta en una métrica.
- Para obtener más información sobre cómo acceder a las métricas y exportarlas con Azure Portal, la API de REST o la CLI, consulte [Información general sobre las métricas en Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
