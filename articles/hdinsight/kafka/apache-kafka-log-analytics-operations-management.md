---
title: Registros de Azure Monitor para Apache Kafka - Azure HDInsight
description: Obtenga información sobre cómo usar los registros de Azure Monitor para analizar los registros de clúster de Apache Kafka en HDInsight de Azure.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: b4eced461f798f3cbf3ce968dae59cfb8f1a0363
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64697612"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Análisis de registros para Apache Kafka en HDInsight

Obtenga información sobre cómo usar los registros de Azure Monitor para analizar los registros generados por Apache Kafka en HDInsight.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="enable-azure-monitor-logs-for-apache-kafka"></a>Habilitar los registros de Azure Monitor para Apache Kafka

Los pasos para habilitar los registros de Azure Monitor para HDInsight son los mismos para todos los clústeres de HDInsight. Use los vínculos siguientes para aprender a crear y configurar los servicios necesarios:

1. Cree un área de trabajo de Log Analytics. Para obtener más información, consulte el documento [Introducción a un área de trabajo de Log Analytics](https://docs.microsoft.com/azure/log-analytics).

2. Cree Kafka en un clúster de HDInsight. Para obtener más información, consulte el documento [Introducción a Apache Kafka (versión preliminar) en HDInsight](apache-kafka-get-started.md).

3. Configurar el clúster de Kafka para usar los registros de Azure Monitor. Para obtener más información, consulte el [los registros de uso de Azure Monitor para supervisar HDInsight](../hdinsight-hadoop-oms-log-analytics-tutorial.md) documento.

    > [!NOTE]  
    > También puede configurar el clúster para usar los registros de Azure Monitor mediante la `Enable-AzHDInsightOperationsManagementSuite` cmdlet. Este cmdlet requiere la información siguiente:
    >
    > * El nombre del clúster de HDInsight.
    > * El identificador de área de trabajo para los registros de Azure Monitor. Puede encontrar el identificador de área de trabajo de su área de trabajo de Log Analytics.
    > * La clave principal para la conexión a log analytics. Para encontrar la clave principal, abra el área de trabajo en Azure Portal, seleccione __Configuración avanzada__ en el menú de la izquierda. En Configuración avanzada, seleccione __Orígenes conectados__>__Servidores Linux__.


> [!IMPORTANT]  
> Puede tardar unos 20 minutos antes de datos estén disponibles para los registros de Azure Monitor.

## <a name="query-logs"></a>Registros de consultas

1. En [Azure Portal](https://portal.azure.com), seleccione el área de trabajo de Log Analytics.

2. Seleccione __Búsqueda de registros__. Aquí podrá buscar los datos recopilados de Kafka. A continuación se presentan algunos ejemplos de búsqueda:

   * Uso de disco: `Perf | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

   * Uso de CPU: `Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

   * Mensajes entrantes por segundo: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)`

   * Bytes entrantes por segundo: `metrics_kafka_CL | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

   * Bytes salientes por segundo: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

     > [!IMPORTANT]  
     > Reemplace los valores de consulta por la información específica de su clúster. Por ejemplo, `ClusterName_s` debe establecerse en el nombre del clúster. `HostName_s` debe establecerse en el nombre de dominio de un nodo de trabajo en el clúster.

     También puede escribir `*` para buscar todos los tipos registrados. Actualmente están disponibles los registros siguientes para las consultas:

     | Tipo de registro | DESCRIPCIÓN |
     | ---- | ---- |
     | log\_kafkaserver\_CL | Kafka broker server.log |
     | log\_kafkacontroller\_CL | Kafka broker controller.log |
     | metrics\_kafka\_CL | Kafka JMX metrics |

     ![Imagen de la búsqueda de uso de CPU](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Azure Monitor, consulte el [información general de Azure Monitor](../../log-analytics/log-analytics-get-started.md) documento.

Para información sobre cómo trabajar con Apache Kafka, consulte los documentos siguientes:

 * [Uso de MirrorMaker para replicar temas de Apache Kafka con Kafka en HDInsight](apache-kafka-mirroring.md)
 * [Configuración del almacenamiento y la escalabilidad de Apache Kafka en HDInsight](apache-kafka-scalability.md)
 * [Ejemplo de streaming de Apache Spark (DStream) con Apache Kafka en HDInsight](../hdinsight-apache-spark-with-kafka.md)
 * [Tutorial: Uso de Apache Spark Structured Streaming con Apache Kafka en HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
