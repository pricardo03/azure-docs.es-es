---
title: Uso de Log Analytics para supervisar clústeres de HDInsight
description: Aprenda a usar Azure Log Analytics para supervisar trabajos que se ejecutan en un clúster de HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: jasonh
ms.openlocfilehash: 87d1593327ab45f064c6f6b19dfbe3882baaa446
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43109523"
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Uso de Azure Log Analytics para supervisar clústeres de Azure HDInsight

Obtenga información sobre cómo habilitar Azure Log Analytics para supervisar las operaciones de clúster de Hadoop en HDInsight y cómo agregar una solución de supervisión de HDInisght.

[Log Analytics](../log-analytics/log-analytics-overview.md) es un servicio que supervisa los entornos local y en la nube para mantener su disponibilidad y rendimiento. Recopila los datos generados por los recursos en los entornos local y de nube y mediante otras herramientas de supervisión, para proporcionar análisis entre varios orígenes.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* **Un área de trabajo de Log Analytics**. Considere el área de trabajo como un entorno de Log Analytics único, con su propio repositorio de datos, sus propios orígenes de datos y sus propias soluciones. Para instrucciones, consulte [Crear un área de trabajo](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

* **Un clúster de HDInsight de Azure**. Actualmente, puede usar Log Analytics con los siguientes tipos de clúster de HDInsight:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Para instrucciones sobre cómo crear un clúster en HDInsight, consulte el artículo de [introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

> [!NOTE]
> Se recomienda colocar el clúster de HDInsight y el área de trabajo de Log Analytics en la misma región para mejorar el rendimiento. Tenga en cuenta que Azure Log Analytics no está disponible en todas las regiones de Azure.

## <a name="enable-log-analytics-by-using-the-portal"></a>Habilitación de Log Analytics mediante el portal

En esta sección, configurará un clúster de HDInsight Hadoop existente para usar un área de trabajo de Azure Log Analytics para supervisar trabajos, depurar registros, etc.

1. Abra un clúster de HDInsight en Azure Portal.
2. En el panel izquierdo, seleccione **Supervisión**.
3. En el panel derecho, seleccione **Habilitar**, elija un área de trabajo de Log Analytics existente y seleccione **Guardar**.

    ![Habilitar la supervisión para clústeres de HDInsight](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Enable monitoring for HDInsight clusters")

    Guardar la configuración tarda unos minutos.

## <a name="enable-log-analytics-by-using-azure-powershell"></a>Habilitación de Log Analytics mediante Azure PowerShell

Puede habilitar Log Analytics con Azure PowerShell. El cmdlet es el siguiente:

```powershell
Enable-AzureRmHDInsightOperationsManagementSuite
      [-Name] <CLUSTER NAME>
      [-WorkspaceId] <LOG ANALYTICS WORKSPACE NAME>
      [-PrimaryKey] <LOG ANALYTICS WORKSPACE PRIMARY KEY>
      [-ResourceGroupName] <RESOURCE GROUIP NAME>
```

Consulte [Enable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/Enable-AzureRmHDInsightOperationsManagementSuite?view=azurermps-5.0.0).

Este es el cmdlet para deshabilitarlo:

```powershell
Disable-AzureRmHDInsightOperationsManagementSuite
       [-Name] <CLUSTER NAME>
       [-ResourceGroupName] <RESOURCE GROUP NAME>
```

Consulte [Disable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/disable-azurermhdinsightoperationsmanagementsuite?view=azurermps-5.0.0).

## <a name="install-hdinsight-cluster-management-solutions"></a>Instalación de soluciones de administración de clústeres de HDInsight

HDInsight proporciona soluciones de administración específicas de clúster que se pueden agregar a Azure Log Analytics. Las [soluciones de administración](../log-analytics/log-analytics-add-solutions.md) agregan funcionalidad a Log Analytics, que proporcionan datos y herramientas de análisis adicionales. Estas soluciones recopilan importantes métricas de rendimiento de los clústeres de HDInsight y proporcionan las herramientas para buscar las métricas. También proporcionan visualizaciones y paneles para la mayoría de los tipos de clúster admitidos en HDInsight. Mediante el uso de las métricas que se recopilan con la solución, puede crear alertas y reglas de supervisión personalizadas.

Estas son las soluciones de HDInsight disponibles:

* Supervisión de HDInsight Hadoop
* Supervisión de HDInsight HBase
* Supervisión de HDInsight Interactive Query
* Supervisión de HDInsight Kafka
* Supervisión de HDInsight Spark
* Supervisión de HDInsight Storm

Para instrucciones para instalar una solución de administración, consulte [Soluciones de administración en Azure](../monitoring/monitoring-solutions.md#install-a-management-solution). Para experimentar, instale una solución de supervisión de HDInsight Hadoop. Cuando termine, verá un icono **HDInsightHadoop** en **Resumen**. Seleccione el icono **HDInsightHadoop**. La solución HDInsightHadoop tiene el siguiente aspecto:

![Vista de solución de supervisión de HDInsight OMS Hadoop](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Dado que el clúster es un clúster nuevo de marca, el informe no muestra ninguna actividad.

## <a name="next-steps"></a>Pasos siguientes

* [Consulta de Azure Log Analytics para supervisar clústeres de Azure HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)
