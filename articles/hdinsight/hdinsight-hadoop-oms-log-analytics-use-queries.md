---
title: Los registros de consulta de Azure Monitor para supervisar clústeres de HDInsight de Azure
description: Obtenga información sobre cómo ejecutar consultas en los registros de Azure Monitor para supervisar trabajos que se ejecutan en un clúster de HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: e1187867fc9da9a89f92d7b321c8703ee7a8a407
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889263"
---
# <a name="query-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Los registros de consulta de Azure Monitor para supervisar clústeres de HDInsight

Conozca algunos escenarios básicos sobre cómo usar los registros de Azure Monitor para supervisar clústeres de HDInsight de Azure:

* [Análisis de métricas de clúster de HDInsight](#analyze-hdinsight-cluster-metrics)
* [Búsqueda de mensajes de registro específicos](#search-for-specific-log-messages)
* [Creación de alertas de eventos](#create-alerts-for-tracking-events)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Requisitos previos

* Debe haber configurado un clúster de HDInsight para usar los registros de Azure Monitor y agrega los registros de Azure Monitor específicos del clúster de HDInsight soluciones al área de trabajo de supervisión. Para obtener instrucciones, consulte [los registros de uso de Azure Monitor con clústeres de HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>Análisis de métricas de clúster de HDInsight

Aprenda a buscar métricas específicas de su clúster de HDInsight.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
2. Seleccione el icono **Búsqueda de registros**.
3. Escriba la siguiente consulta en el cuadro de búsqueda para buscar todas las métricas de todas las métricas disponibles para todos los clústeres de HDInsight configurados para usar los registros de Azure Monitor y, a continuación, seleccione **ejecutar**.

        search *

    ![Buscar todas las métricas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Search all metrics")

    La salida debe parecerse a esta:

    ![Salida de búsqueda de todas las métricas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "Search all metrics output")

5. En el panel izquierdo, en **Tipo**, seleccione una métrica en la que quiera profundizar y luego seleccione **Aplicar**. La siguiente captura de pantalla muestra que se ha seleccionado el tipo `metrics_resourcemanager_queue_root_default_CL`.

    > [!NOTE]  
    > Puede que deba seleccionar el botón **[+]Más** para encontrar la métrica que busca. Además, el botón **Aplicar** se encuentra en la parte inferior de la lista por lo que debe desplazarse hacia abajo para verlo.

    Tenga en cuenta que la consulta del cuadro de texto cambia a la que se muestra en el cuadro resaltado en la captura de pantalla siguiente:

    ![Buscar métricas específicas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "Search for specific metrics")

6. Para profundizar en esta métrica específica, puede, por ejemplo, restringir la salida existente en función de los recursos medios usados en un intervalo de 10 minutos, clasificados por nombre del clúster mediante la siguiente consulta:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

7. En lugar de restringir en función de los recursos medios usados, puede usar la siguiente consulta para restringir los resultados según el número máximo de recursos que se usaron (así como los percentiles 90 y 95) en una ventana de 10 minutos:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Búsqueda de mensajes de registro específicos

Aprenda a buscar mensajes de error durante una ventana de tiempo específica. Estos pasos son un ejemplo de cómo puede llegar al mensaje de error en el que está interesado. Para encontrar los errores que busca, puede usar cualquier propiedad que esté disponible.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
2. Seleccione el icono **Búsqueda de registros**.
3. Escriba la siguiente consulta para buscar todos los mensajes de error para todos los clústeres de HDInsight configurados para usar los registros de Azure Monitor y, a continuación, seleccione **ejecutar**. 

         search "Error"

    Debería ver una salida similar a la siguiente:

    ![Salida de búsqueda de todos los errores](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "Search all errors output")

4. En el panel izquierdo, en la categoría **Tipo**, seleccione un tipo de error en el que quiera profundizar y luego seleccione **Aplicar**.  Observe que los resultados se restringen para mostrar únicamente el error del tipo seleccionado.
5. Puede profundizar en esta lista específica de errores mediante las opciones disponibles en el panel izquierdo. Por ejemplo: 

    - Para ver los mensajes de error de un nodo de trabajo específico:

        ![Salida de búsqueda de errores específicos](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "Search for specific errors output")

    - Para ver que un error producido en un momento determinado:

        ![Salida de búsqueda de errores específicos](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "Search for specific errors output")

6. Para ver el error específico: Puede seleccionar **[+] Mostrar más** para examinar el mensaje de error real.

    ![Salida de búsqueda de errores específicos](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "Search for specific errors output")

## <a name="create-alerts-for-tracking-events"></a>Creación de alertas para el seguimiento de eventos

El primer paso para crear una alerta es llegar a una consulta en función de la cual se desencadena la alerta. Puede usar cualquier consulta que desee para crear una alerta.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
2. Seleccione el icono **Búsqueda de registros**.
3. Ejecute la siguiente consulta en la que quiere crear una alerta y luego seleccione **ENTRAR**.

        metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

    La consulta proporciona una lista de aplicaciones con errores que se ejecutan en clústeres de HDInsight.

4. Seleccione **Nueva regla de alertas** en la parte superior de la página.

    ![Escribir una consulta para crear una alerta](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Enter query to create an alert")

5. En la ventana **Crear regla**, escriba la consulta y otros detalles para crear una alerta; a continuación, haga clic en **Crear regla de alertas**.

    ![Escribir una consulta para crear una alerta](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Enter query to create an alert")

Para editar o eliminar una alerta existente:

1. Abra el área de trabajo de Log Analytics desde Azure Portal.
2. En el menú izquierdo, seleccione **Alerta**.
3. Seleccione la alerta que desea editar o eliminar.
4. Tiene las siguientes opciones: **Guardar**, **Descartar**, **Deshabilitar** y **Eliminar**.

    ![Edición de alerta de eliminación de registros de HDInsight de Azure Monitor](media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png)

Para más información, vea [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../azure-monitor/platform/alerts-metric.md).

## <a name="see-also"></a>Vea también

* [Log Analytics de OMS: Diseñador de vistas](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Crear, ver y administrar alertas de métricas con Azure Monitor](../azure-monitor/platform/alerts-metric.md)
