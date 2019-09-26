---
title: Consulta a los registros de Azure Monitor para supervisar clústeres de Azure HDInsight
description: Aprenda a ejecutar consultas en los registros de Azure Monitor para supervisar los trabajos que se ejecutan en un clúster de HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.openlocfilehash: 51344ff7381b6392870b1fd0e331eed38a33915d
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71103521"
---
# <a name="query-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Consulta a los registros de Azure Monitor para supervisar clústeres de HDInsight

Conozca algunos escenarios básicos de uso de los registros de Azure Monitor para supervisar clústeres de Azure HDInsight:

* [Análisis de métricas de clúster de HDInsight](#analyze-hdinsight-cluster-metrics)
* [Búsqueda de mensajes de registro específicos](#search-for-specific-log-messages)
* [Creación de alertas de eventos](#create-alerts-for-tracking-events)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Requisitos previos

Debe haber configurado un clúster de HDInsight para usar registros de Azure Monitor y agregado las soluciones de supervisión de registros de Azure Monitor específicas del clúster de HDInsight al área de trabajo. Puede encontrar instrucciones para ello en [Uso de los registros de Azure Monitor con clústeres de HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>Análisis de métricas de clúster de HDInsight

Aprenda a buscar métricas específicas de su clúster de HDInsight.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
1. Seleccione el icono **Búsqueda de registros**.
1. Escriba la siguiente consulta en el cuadro de texto para buscar todas las métricas disponibles para todos los clústeres de HDInsight que están configurados para usar los registros de Azure Monitor y, luego, seleccione **EJECUTAR**.

        search *

    ![Análisis de Apache Ambari: buscar todas las métricas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Buscar todas las métricas")

    La salida debe parecerse a esta:

    ![Log Analytics: buscar todas las métricas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "Salida de Buscar todas las métricas")

1. En el panel izquierdo, en **Tipo**, seleccione una métrica en la que quiera profundizar y luego seleccione **Aplicar**. La siguiente captura de pantalla muestra que se ha seleccionado el tipo `metrics_resourcemanager_queue_root_default_CL`.

    > [!NOTE]  
    > Puede que deba seleccionar el botón **[+]Más** para encontrar la métrica que busca. Además, el botón **Aplicar** se encuentra en la parte inferior de la lista por lo que debe desplazarse hacia abajo para verlo.

    Tenga en cuenta que la consulta del cuadro de texto cambia a la que se muestra en el cuadro resaltado en la captura de pantalla siguiente:

    ![Log Analytics: buscar métricas específicas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "Buscar métricas específicas")

1. Para profundizar en esta métrica específica, puede, por ejemplo, restringir la salida existente en función de los recursos medios usados en un intervalo de 10 minutos, clasificados por nombre del clúster mediante la siguiente consulta:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

1. En lugar de restringir en función de los recursos medios usados, puede usar la siguiente consulta para restringir los resultados según el número máximo de recursos que se usaron (así como los percentiles 90 y 95) en una ventana de 10 minutos:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Búsqueda de mensajes de registro específicos

Aprenda a buscar mensajes de error durante una ventana de tiempo específica. Estos pasos son un ejemplo de cómo puede llegar al mensaje de error en el que está interesado. Para encontrar los errores que busca, puede usar cualquier propiedad que esté disponible.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
2. Seleccione el icono **Búsqueda de registros**.
3. Escriba la siguiente consulta para buscar todos los mensajes de error de todos los clústeres de HDInsight configurados para usar los registros de Azure Monitor y luego seleccione **EJECUTAR**.

         search "Error"

    Debería ver una salida similar a la siguiente:

    ![Azure Portal: errores de búsqueda de registros](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "Salida de Buscar todos los errores")

4. En el panel izquierdo, en la categoría **Tipo**, seleccione un tipo de error en el que quiera profundizar y luego seleccione **Aplicar**.  Observe que los resultados se restringen para mostrar únicamente el error del tipo seleccionado.

5. Puede profundizar en esta lista específica de errores mediante las opciones disponibles en el panel izquierdo. Por ejemplo:

    - Para ver los mensajes de error de un nodo de trabajo específico:

        ![Salida1 de búsqueda de errores específicos](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "Search for specific errors output1")

    - Para ver que un error producido en un momento determinado:

        ![Salida2 de búsqueda de errores específicos](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "Search for specific errors output2")

6. Para ver el error específico: Puede seleccionar **[+] Mostrar más** para examinar el mensaje de error real.

    ![Salida3 de búsqueda de errores específicos](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "Search for specific errors output3")

## <a name="create-alerts-for-tracking-events"></a>Creación de alertas para el seguimiento de eventos

El primer paso para crear una alerta es llegar a una consulta en función de la cual se desencadena la alerta. Puede usar cualquier consulta que desee para crear una alerta.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
2. Seleccione el icono **Búsqueda de registros**.
3. Ejecute la siguiente consulta en la que quiere crear una alerta y luego seleccione **ENTRAR**.

        metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

    La consulta proporciona una lista de aplicaciones con errores que se ejecutan en clústeres de HDInsight.

4. Seleccione **Nueva regla de alertas** en la parte superior de la página.

    ![Escribir una consulta para crear una alerta1](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Enter query to create an alert1")

5. En la ventana **Crear regla**, escriba la consulta y otros detalles para crear una alerta; a continuación, haga clic en **Crear regla de alertas**.

    ![Escribir una consulta para crear una alerta2](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Enter query to create an alert2")

Para editar o eliminar una alerta existente:

1. Abra el área de trabajo de Log Analytics desde Azure Portal.
2. En el menú izquierdo, seleccione **Alerta**.
3. Seleccione la alerta que desea editar o eliminar.
4. Tiene las siguientes opciones: **Guardar**, **Descartar**, **Deshabilitar** y **Eliminar**.

    ![Alerta para editar o eliminar registros de Azure Monitor para HDInsight](media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png)

Para más información, vea [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../azure-monitor/platform/alerts-metric.md).

## <a name="see-also"></a>Otras referencias

* [Creación de vistas personalizadas mediante el Diseñador de vistas en Azure Monitor](../azure-monitor/platform/view-designer.md)
* [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../azure-monitor/platform/alerts-metric.md)
