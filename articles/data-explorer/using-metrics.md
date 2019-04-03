---
title: Supervisar el rendimiento, mantenimiento y uso con métricas de explorador de datos de Azure
description: Obtenga información sobre cómo usar las métricas del explorador de datos de Azure para supervisar el rendimiento, mantenimiento y el uso del clúster.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/01/2019
ms.openlocfilehash: 5252ca8898439b63a8819f6abfd634de0786932b
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851937"
---
# <a name="monitor-azure-data-explorer-performance-health-and-usage-with-metrics"></a>Supervisar el rendimiento, mantenimiento y uso con métricas de explorador de datos de Azure

Azure Data Explorer es un servicio de análisis de datos rápido y totalmente administrado para analizar en tiempo real grandes volúmenes de datos de que se transmiten desde aplicaciones, sitios web, dispositivos IoT, etc. Para usar Azure Data Explorer, cree primero un clúster y una o varias bases de datos en ese clúster. A continuación, ingerirá (cargará) los datos en una base de datos para que pueda ejecutar consultas en ella. Métricas de explorador de datos de Azure proporcionan los indicadores clave sobre el estado y rendimiento de los recursos de clúster. Usar las métricas que se detallan en este artículo para supervisar el estado del clúster de explorador de datos de Azure y el rendimiento en su situación específica como métricas independientes. También puede usar como base para las métricas operacionales [paneles de Azure](/azure/azure-portal/azure-portal-dashboards) y [alertas de Azure](/azure/azure-monitor/platform/alerts-metric-overview).

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción de Azure, cree un [cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Crear un [base de datos y clúster](create-cluster-database-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="using-metrics"></a>El uso de métricas

En el clúster del explorador de datos de Azure, seleccione **métricas** para abrir el panel de métricas y comenzar el análisis en el clúster.

![Seleccione Métricas](media/using-metrics/select-metrics.png)

En el panel de métricas:

![Panel de métricas](media/using-metrics/metrics-pane.png)

1. Para crear un gráfico de métricas, seleccione **métrica** pertinentes y nombre **agregación** por métrica como se detalla a continuación. El **recursos** y **métrica Namespace** selectores están preseleccionados en el clúster del explorador de datos de Azure.

    **Métrica** | **Unidad** | **Agregación** | **Descripción de la métrica**
    |---|---|---|---|
    | Utilización de la caché | Percent | AVG, Max, Min | Relación entre el tamaño de caché necesarias (según la directiva de caché definido) y el tamaño de caché total del clúster (el tamaño total de SSD para la actividad del usuario). Una utilización media de memoria caché de 80% o menos es un estado sostenible para un clúster. Si la utilización media de memoria caché es superior al 80%, el clúster debe ser [escalar verticalmente](manage-cluster-scale-up.md) a un almacenamiento optimizado para plan de tarifa o [escalar horizontalmente](manage-cluster-scale-out.md) de varias instancias. Como alternativa, adaptar la directiva de caché (menos días en caché). Si la utilización de la caché es más de 100%, el tamaño de los datos en caché, según la directiva de caché es mayor que el tamaño total de memoria caché en el clúster. |
    | CPU | Percent | AVG, Max, Min | Proporción entre total uso de CPU y la CPU disponible en todo el clúster. Un promedio de CPU del 80% o menos es sostenible para un clúster. El valor máximo de CPU es 100%, lo que significa que no hay ningún recurso de proceso adicionales para procesar los datos. Cuando un clúster no está funcionando correctamente, compruebe el valor máximo de la CPU para determinar si existen las CPU específicas que están bloqueadas. |
    | Eventos procesados (para Event Hubs) | Número | MAX, Min, Sum | Número total de eventos enviados por Event Hubs y recibidos por el clúster. Los eventos se dividen en eventos rechazados y aceptados por el motor de clúster. |
    | Latencia de ingesta | Segundos | AVG, Max, Min | Latencia de datos recibidos desde el momento en que se recibieron los datos en el clúster hasta que esté preparado para la consulta. Latencia de ingesta se mide en segundos. El periodo de latencia de ingesta depende del escenario de ingesta. |
    | Resultado de ingesta | Número | Número | Número total de operaciones de ingesta de errores y se ha realizado correctamente. Use **aplicar dividir** para crear cubos de éxito y producirá un error en los resultados.|
    | Uso de ingesta | Percent | AVG, Max, Min | Proporción entre los recursos reales que se usa para la ingesta de datos y los recursos totales asignados en la directiva de capacidad para realizar la ingesta. La directiva de capacidad predeterminada no es más de 512 operaciones simultáneas de ingesta o el 75% de los recursos de clúster invertidos en la ingesta. Utilización de la ingesta promedio de 80% o menos es un estado sostenible para un clúster. Valor máximo de utilización de ingesta es 100%, lo que significa que se usa la capacidad de ingesta de todos los clúster y puede dar lugar a una cola de ingesta. |
    | Volumen de ingesta (en MB) | Número | MAX, Min, Sum | El tamaño total de datos ingeridos en el clúster (en MB). Las unidades son el número de megabytes de datos introducidos antes de la compresión. |
    | Mantenimiento de conexión | Número | Media | Realiza un seguimiento de la capacidad de respuesta del clúster. Un clúster totalmente responde devuelve el valor 1 y un clúster desconectado o bloqueado devuelve 0. |
    | Duración de la consulta | Segundos | Count, Avg, Min, Max, Sum | Tiempo total hasta que se reciben los resultados de la consulta. |
    | | | |

    Información adicional relacionada con [métricas de clúster del explorador de datos de Azure compatibles](/azure/azure-monitor/platform/metrics-supported#microsoftkustoclusters)

2. Seleccione el **agregar métrica** botón para ver varias métricas trazadas en el mismo gráfico.
3. Seleccione el **+ nuevo gráfico** botón para ver varios gráficos en una vista.
4. Utilice el selector de tiempo para cambiar el intervalo de tiempo (valor predeterminado: últimas 24 horas).
5. Use [ **Agregar filtro** y **aplicar dividir** ](/azure/azure-monitor/platform/metrics-getting-started#apply-dimension-filters-and-splitting) para métricas con dimensiones.
6. Seleccione **Anclar al panel** para agregar la configuración del gráfico a los paneles, por lo que puede volver a verlo.
7. Establecer **nueva regla de alerta** para visualizar las métricas mediante el conjunto de criterios. La nueva regla de alerta incluirá su recurso de destino, métrica, división y dimensiones de filtro del gráfico. Modificar estos valores en el [panel de creación de regla de alerta](/azure/azure-monitor/platform/metrics-charts#create-alert-rules).

Información adicional sobre el uso de la [Explorador de métricas](/azure/azure-monitor/platform/metrics-getting-started).


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: Consulta de datos en Azure Data Explorer](web-query-data.md)
