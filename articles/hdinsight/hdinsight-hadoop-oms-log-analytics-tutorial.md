---
title: Los registros de uso de Azure Monitor para supervisar clústeres de HDInsight de Azure
description: Aprenda a usar los registros de Azure Monitor para supervisar trabajos que se ejecutan en un clúster de HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 16659a335ef6126e75f5a9a99784e71afa056bef
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479254"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Los registros de uso de Azure Monitor para supervisar clústeres de HDInsight

Obtenga información sobre cómo habilitar los registros de Azure Monitor supervisar las operaciones de clúster de Hadoop en HDInsight y cómo agregar una solución de supervisión de HDInsight.

[Registros de Azure Monitor](../log-analytics/log-analytics-overview.md) es un servicio en Azure Monitor que supervisa y la nube en entornos locales para mantener su disponibilidad y rendimiento. Recopila los datos generados por los recursos en los entornos local y de nube y mediante otras herramientas de supervisión, para proporcionar análisis entre varios orígenes.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* **Un área de trabajo de Log Analytics**. Esta área de trabajo se puede considerar como un entorno de los registros de Azure Monitor único con su propio repositorio de datos, orígenes de datos y soluciones. Para más instrucciones, consulte [Creación de un área de trabajo de Log Analytics](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace).

* **Un clúster de HDInsight de Azure**. Actualmente, puede usar los registros de Azure Monitor con los siguientes tipos de clúster de HDInsight:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Para instrucciones sobre cómo crear un clúster en HDInsight, consulte el artículo de [introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).  

* **Módulo de Azure PowerShell Az**.  Consulte [introducir el nuevo módulo de Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

> [!NOTE]  
> Se recomienda colocar el clúster de HDInsight y el área de trabajo de Log Analytics en la misma región para mejorar el rendimiento. Registros de Azure Monitor no está disponible en todas las regiones de Azure.

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>Habilitar los registros de Azure Monitor mediante el portal

En esta sección, configurará un clúster de HDInsight Hadoop existente para usar un área de trabajo de Azure Log Analytics para supervisar trabajos, depurar registros, etc.

1. Desde el [portal Azure](https://portal.azure.com/), seleccione el clúster.  Consulte [Enumeración y visualización de clústeres](./hdinsight-administer-use-portal-linux.md#showClusters) para obtener instrucciones. El clúster se abre en una nueva página del portal.

1. Desde la izquierda, en **Supervisión**, seleccione **Operations Management Suite**.

1. En la vista principal, en **Supervisión de OMS**, seleccione **Habilitar**.

1. En la lista desplegable **Seleccionar un área de trabajo**, seleccione un área de trabajo de Log Analytics existente.

1. Seleccione **Guardar**.  Guardar la configuración tarda unos minutos.

    ![Habilitar la supervisión para clústeres de HDInsight](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Enable monitoring for HDInsight clusters")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>Habilitar los registros de Azure Monitor con Azure PowerShell

Puede habilitar los registros de Azure Monitor con el módulo Azure PowerShell Az [Enable AzHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightoperationsmanagementsuite) cmdlet.

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace -ResourceGroupName $resourceGroup -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace -ResourceGroupName $resourceGroup -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables Operations Management Suite
Enable-AzHDInsightOperationsManagementSuite -ResourceGroupName $resourceGroup -Name $cluster -WorkspaceId $WorkspaceId -PrimaryKey $PrimaryKey
```

Para deshabilitar el uso del [Disable AzHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightoperationsmanagementsuite) cmdlet:

```powershell
Disable-AzHDInsightOperationsManagementSuite -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>Instalación de soluciones de administración de clústeres de HDInsight

HDInsight proporciona soluciones de administración específicas de clúster que se pueden agregar para los registros de Azure Monitor. [Las soluciones de administración](../log-analytics/log-analytics-add-solutions.md) agregar funcionalidad a los registros de Azure Monitor, que proporciona herramientas de análisis y datos adicionales. Estas soluciones recopilan importantes métricas de rendimiento de los clústeres de HDInsight y proporcionan las herramientas para buscar las métricas. También proporcionan visualizaciones y paneles para la mayoría de los tipos de clúster admitidos en HDInsight. Mediante el uso de las métricas que se recopilan con la solución, puede crear alertas y reglas de supervisión personalizadas.

Estas son las soluciones de HDInsight disponibles:

* Supervisión de HDInsight Hadoop
* Supervisión de HDInsight HBase
* Supervisión de HDInsight Interactive Query
* Supervisión de HDInsight Kafka
* Supervisión de HDInsight Spark
* Supervisión de HDInsight Storm

Para instrucciones para instalar una solución de administración, consulte [Soluciones de administración en Azure](../azure-monitor/insights/solutions.md#install-a-monitoring-solution). Para experimentar, instalar una solución de supervisión de Hadoop de HDInsight. Cuando termine, verá un icono **HDInsightHadoop** en **Resumen**. Seleccione el icono **HDInsightHadoop**. La solución HDInsightHadoop tiene el siguiente aspecto:

![Vista de solución de supervisión de HDInsight](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Dado que el clúster es un clúster nuevo de marca, el informe no muestra ninguna actividad.

## <a name="next-steps"></a>Pasos siguientes

* [Los registros de consulta de Azure Monitor para supervisar clústeres de HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)
