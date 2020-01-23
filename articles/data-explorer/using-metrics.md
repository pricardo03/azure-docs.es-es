---
title: Supervisión del rendimiento, el mantenimiento y el uso de Azure Data Explorer con métricas
description: Aprenda a usar métricas de Azure Data Explorer para supervisar el rendimiento, el mantenimiento y el uso del clúster.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: 7ff504a466224594c0098bc9d80557d45e4197a6
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76027890"
---
# <a name="monitor-azure-data-explorer-performance-health-and-usage-with-metrics"></a>Supervisión del rendimiento, el mantenimiento y el uso de Azure Data Explorer con métricas

Azure Data Explorer es un servicio de análisis de datos rápido y totalmente administrado para analizar en tiempo real grandes volúmenes de datos de que se transmiten desde aplicaciones, sitios web, dispositivos IoT, etc. Para usar Azure Data Explorer, cree primero un clúster y una o varias bases de datos en ese clúster. A continuación, ingerirá (cargará) los datos en una base de datos para que pueda ejecutar consultas en ella. Las métricas de Azure Data Explorer proporcionan indicadores clave sobre el mantenimiento y el rendimiento de los recursos del clúster. Use las métricas que se detallan en este artículo para supervisar el mantenimiento y el rendimiento del clúster de Azure Data Explorer en su escenario específico como métricas independientes. También puede usar como métricas como base operacional para los [paneles de Azure](/azure/azure-portal/azure-portal-dashboards) y las [alertas de Azure](/azure/azure-monitor/platform/alerts-metric-overview).

## <a name="prerequisites"></a>Prerequisites

* Si no tiene una suscripción a Azure, cree una [cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* Creación de un [clúster y una base de datos](create-cluster-database-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="using-metrics"></a>Uso de métricas

En el clúster de Azure Data Explorer, seleccione **Métricas** para abrir el panel de métricas y comenzar el análisis del clúster.

![Seleccione Métricas](media/using-metrics/select-metrics.png)

En el panel de métricas:

![Panel de métricas](media/using-metrics/metrics-pane.png)

1. Para crear un gráfico de métricas, seleccione el nombre de la **métrica** y la **agregación** de interés por métrica, como se detalla a continuación. Los selectores **Recurso** y **Espacio de nombres de métrica** están seleccionados previamente con el clúster de Azure Data Explorer.

    **Métrica** | **Unidad** | **Agregación** | **Descripción de la métrica**
    |---|---|---|---|
    | Uso de la caché | Percent | Media, máxima, mínima | Porcentaje de recursos de caché asignados actualmente en uso por el clúster. La memoria caché tiene el tamaño de SSD asignado para la actividad del usuario según la directiva de caché definida. Una utilización media de la caché del 80 % o menos es un estado sostenible para un clúster. Si el porcentaje es superior, el clúster se debe [escalar verticalmente](manage-cluster-vertical-scaling.md) hasta un plan de tarifa optimizado para almacenamiento, u [horizontalmente](manage-cluster-horizontal-scaling.md) a más instancias. Como alternativa, adapte la directiva de caché (menos días en caché). Si la utilización de la caché es superior al 100 %, el tamaño de los datos para almacenarse en caché, según la directiva de almacenamiento en caché, es superior al tamaño total de la caché del clúster. |
    | CPU | Percent | Media, máxima, mínima | Porcentaje de recursos de proceso asignados actualmente en uso por las máquinas del clúster. Una CPU media del 80 % o menos es sostenible para un clúster. El valor máximo de CPU es 100 %, lo que significa que no hay otros recursos de proceso para procesar los datos. Cuando un clúster funciona bien, compruebe el valor máximo de la CPU para determinar si existen CPU específicas que están bloqueadas. |
    | Eventos procesados (para Event Hubs) | Count | Máximo, mínimo, suma | Número total de eventos leídos de los centros de eventos y procesados por el clúster. Los eventos se dividen en eventos rechazados y aceptados por el motor del clúster. |
    | Latencia de la ingesta | Segundos | Media, máxima, mínima | La latencia de datos ingeridos desde el momento en que se recibieron los datos en el clúster hasta que está preparado para la consulta. El periodo de latencia de ingesta depende del escenario de ingesta. |
    | Resultado de la ingesta | Count | Count | Número total de operaciones de ingesta correctas e incorrectas. Use **Apply splitting** (Aplicar separación) crear depósitos de resultados correctos e incorrectos y analizar las dimensiones (**Valor** > **Estado**).|
    | Uso de la ingesta | Percent | Media, máxima, mínima | Porcentaje de recursos reales usados para ingerir datos de los recursos totales asignados en la directiva de capacidad para realizar la ingesta. La directiva de capacidad predeterminada es no más de 512 operaciones simultáneas de ingesta o el 75 % de los recursos del clúster invertidos en la ingesta. La utilización media de la ingesta de un 80 % o menos es un estado sostenible para un clúster. El valor máximo de utilización de la ingesta es 100 %, lo que significa que se usa toda la capacidad de ingesta del clúster y, como resultado, se puede generar una cola de ingesta. |
    | Volumen de ingesta (en MB) | Count | Máximo, mínimo, suma | El tamaño total de datos ingeridos en el clúster (en MB) antes de la compresión. |
    | Mantener conexión | Count | Avg | Realiza un seguimiento de la capacidad de respuesta del clúster. Un clúster que responde totalmente devuelve el valor 1 y un clúster desconectado o bloqueado devuelve 0. |
    | Duración de la consulta | Segundos | Recuento, media, mínimo, máximo, suma | Tiempo total hasta que se reciben los resultados de la consulta (no incluye la latencia de red). |
    | Número total de consultas simultáneas | Count | Media, máxima, mínima | Número de consultas que se ejecutan en paralelo en el clúster. Esta métrica es una buena manera de calcular la carga en el clúster. |
    | Número total de consultas limitadas | Count | Media, máxima, mínima | Número de consultas limitadas (rechazadas) en el clúster. El número máximo permitido de consultas simultáneas (en paralelo) se define en la directiva de consulta simultánea. |
    | Número total de comandos limitados | Count | Media, máxima, mínima | Número de comandos limitados (rechazados) en el clúster, ya que se alcanzó el número máximo permitido de comandos simultáneos (en paralelo). |
    | Número total de extensiones | Count | Media, máxima, mínima | Número total de extensiones de datos del clúster. Los cambios en esta métrica pueden implicar cambios masivos en la estructura de datos y una carga elevada en el clúster, ya que la combinación de extensiones de datos es una actividad que consume gran cantidad de CPU. |
    | | | | |

    Información adicional sobre [métricas del clúster de Azure Data Explorer admitidas](/azure/azure-monitor/platform/metrics-supported#microsoftkustoclusters)

2. Seleccione el botón **Agregar métrica** para ver varias métricas trazadas en el mismo gráfico.
3. Seleccione el botón **+ Nuevo gráfico** para ver varios gráficos en una vista.
4. Use el selector de tiempo para cambiar el intervalo de tiempo (valor predeterminado: últimas 24 horas).
5. Use [**Agregar filtro** y **Apply splitting**](/azure/azure-monitor/platform/metrics-getting-started#apply-dimension-filters-and-splitting) (Aplicar separación) con métricas que tengan dimensiones.
6. Seleccione **Anclar al panel** para agregar la configuración del gráfico a los paneles para que pueda verla de nuevo.
7. Establezca **Nueva regla de alertas** para visualizar las métricas mediante los criterios establecidos. La nueva regla de alerta incluirá su recurso de destino, métrica, división y dimensiones de filtro del gráfico. Modifique esta configuración en el [panel de creación de reglas de alerta](/azure/azure-monitor/platform/metrics-charts#create-alert-rules).

Información adicional sobre el uso del [Explorador de métricas](/azure/azure-monitor/platform/metrics-getting-started).


## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Ingesta y consulta de datos de supervisión en Azure Data Explorer](/azure/data-explorer/ingest-data-no-code)
* [Supervisión de las operaciones de ingesta de Azure Data Explorer mediante registros de diagnóstico](/azure/data-explorer/using-diagnostic-logs)
* [Inicio rápido: Consulta de datos en Azure Data Explorer](web-query-data.md)
