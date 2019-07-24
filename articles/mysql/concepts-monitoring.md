---
title: Supervisión en Azure Database for MySQL
description: En este artículo se describen las métricas de supervisión y alerta de Azure Database for MySQL, incluidas las estadísticas de CPU, almacenamiento y conexión.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/05/2019
ms.openlocfilehash: 0122f952e586d0535fc2e482c7b78266f8809272
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062438"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>Supervisión en Azure Database for MySQL
La supervisión de los datos sobre los servidores le permite solucionar problemas y optimizar la carga de trabajo. Azure Database for MySQL proporciona diversas métricas que proporcionan información sobre el comportamiento del servidor.

## <a name="metrics"></a>Métricas
Todas las métricas de Azure tienen una frecuencia de un minuto y cada métrica proporciona 30 días de historial. Puede configurar alertas en las métricas. Para obtener instrucciones paso a paso, consulte [How to set up alerts](howto-alert-on-metric.md) (Configuración de alertas). Otras tareas incluyen la configuración de acciones automatizadas, la realización de análisis avanzados y el archivo del historial. Para obtener más información, consulte [Información general sobre las métricas en Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lista de métricas
Estas métricas están disponibles para Azure Database for MySQL:

|Métrica|Nombre de métrica para mostrar|Unidad|DESCRIPCIÓN|
|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Porcentaje de CPU en uso.|
|memory_percent|Porcentaje de memoria|Percent|Porcentaje de memoria en uso.|
|io_consumption_percent|Porcentaje de E/S|Percent|Porcentaje de E/S en uso.|
|storage_percent|Porcentaje de almacenamiento|Percent|Porcentaje de almacenamiento que se usa más allá del límite máximo del servidor.|
|storage_used|Almacenamiento utilizado|Bytes|Cantidad de almacenamiento en uso. El almacenamiento que usa el servicio puede incluir los archivos de base de datos, los registros de transacciones y los registros de servidor.|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|El porcentaje usado del almacenamiento máximo de registro del servidor.|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|La cantidad de almacenamiento de registro del servidor en uso.|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|El almacenamiento máximo de registro de este servidor.|
|storage_limit|Límite de almacenamiento|Bytes|Almacenamiento máximo de este servidor.|
|active_connections|Conexiones activas|Recuento|Número de conexiones activas al servidor.|
|connections_failed|Conexiones con errores|Recuento|Número de conexiones con errores al servidor.|
|seconds_behind_master|Intervalo de replicación en segundos|Recuento|El número de segundos que el servidor de réplica se retrasa en relación con el servidor maestro.|
|network_bytes_egress|Red interna|Bytes|Red externa a través de conexiones activas.|
|network_bytes_ingress|Red interna|Bytes|Red interna a través de conexiones activas.|
|backup_storage_used|Almacenamiento de copia de seguridad utilizado|Bytes|Cantidad de almacenamiento de copia de seguridad utilizado.|

## <a name="server-logs"></a>Registros del servidor
Puede habilitar el registro de consultas lentas y el registro de auditoría en el servidor. Estos registros también están disponibles mediante los registros de diagnóstico de Azure en registros de Azure Monitor, Event Hubs y la cuenta de almacenamiento. Para obtener más información sobre el registro, visite los artículos sobre  [registros de auditoría](concepts-audit-logs.md) y [registros de consultas lentas](concepts-server-logs.md).

## <a name="query-store"></a>Almacén de consultas
[Almacén de consultas](concepts-query-store.md) es una característica en versión preliminar pública que realiza un seguimiento del rendimiento de las consultas a lo largo del tiempo, lo que incluye estadísticas en tiempo de ejecución y eventos de espera de consultas. La característica conserva la información de rendimiento de consulta en tiempo de ejecución en el esquema **mysql**. Puede controlar la recopilación y el almacenamiento de datos mediante diversos botones de configuración.

## <a name="query-performance-insight"></a>Información de rendimiento de consultas
[Información de rendimiento de consultas](concepts-query-performance-insight.md) funciona en combinación con el Almacén de consultas para proporcionar visualizaciones accesibles desde Azure Portal. Estos gráficos le permiten identificar las consultas clave que afectan al rendimiento. Información de rendimiento de consultas está en versión preliminar pública y es accesible en la sección **Rendimiento inteligente** de la página del portal del servidor de Azure Database for MySQL.

## <a name="performance-recommendations"></a>Recomendaciones de rendimiento
La característica [Recomendaciones de rendimiento](concepts-performance-recommendations.md) identifica oportunidades para mejorar el rendimiento de la carga de trabajo. La versión preliminar pública de Recomendaciones de rendimiento le proporciona recomendaciones para crear nuevos índices que tienen el potencial de mejorar el rendimiento de las cargas de trabajo. Para generar recomendaciones de índices, la característica tiene en cuenta diversas funcionalidades de la base de datos, como su esquema y la carga de trabajo devueltos por el Almacén de consultas. Después de implementar cualquier recomendación de rendimiento, los clientes deben probar el rendimiento para evaluar el efecto de estos cambios.

## <a name="next-steps"></a>Pasos siguientes
- Consulte [How to set up alerts](howto-alert-on-metric.md) (Configuración de alertas) para obtener instrucciones sobre cómo crear una alerta en una métrica.
- Para obtener más información sobre cómo acceder a las métricas y exportarlas con Azure Portal, la API de REST o la CLI, consulte [Información general sobre las métricas en Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Lea nuestro blog sobre [procedimientos recomendados para supervisar el servidor](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-mysql-monitoring/).
