---
title: Los trabajos de Apache Spark Streaming tardan más de lo habitual en procesarse en HDInsight de Azure
description: Los trabajos de Apache Spark Streaming tardan más de lo habitual en procesarse en HDInsight de Azure
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 7c7baaaa5fd08a102b3b55c38fb2c4bf892480c5
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679409"
---
# <a name="scenario-apache-spark-streaming-jobs-take-longer-than-usual-to-process-in-azure-hdinsight"></a>Escenario: Los trabajos de Apache Spark Streaming tardan más de lo habitual en procesarse en HDInsight de Azure

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Apache Spark en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Ve que algunos de los trabajos de Apache Spark Streaming son lentos o tardan más de lo habitual en procesarse. En el caso de las aplicaciones de Spark Streaming, cada lote de mensajes corresponde a un trabajo enviado a Spark. Si un trabajo tarda normalmente X segundos en procesarse, en ocasiones puede tardar de 2 a 3 minutos más de lo habitual.

## <a name="cause"></a>Causa

Una posible causa es que el productor de Apache Kafka tarde más de 2 minutos en terminar de escribir en el clúster de Kafka. Para depurar aún más el problema de Kafka, puede agregar algunos registros al código que usa un productor de Kafka para enviar mensajes y correlacionarlos con los registros del clúster de Kafka.

Otra posible causa es que las lecturas y escrituras frecuentes en WASB provoquen que se retrasen los microlotes posteriores. La implementación de WASB de `Filesystem.listStatus` es muy lenta debido a un algoritmo `O(n!)` para quitar duplicados. Usa demasiada memoria debido a la conversión adicional de `BlobListItem` a `FileMetadata` a `FileStatus`. Por ejemplo, el algoritmo tarda más de 30 minutos en enumerar 700 000 archivos. Por lo tanto, si cada microlote de SparkSQL llama agresivamente a `ListBlobs`, provocará que los microlotes posteriores se retrasen, lo que resultará en lo que se produce como un alto retraso en la programación. [Esta revisión](https://issues.apache.org/jira/browse/HADOOP-15547) corrige el problema, pero si falta en su entorno, `ListBlobs` experimentará una latencia elevada. Además, aunque elimine archivos cada hora, la lista en el back-end tiene que iterar todas las filas (incluidas las eliminadas), porque el proceso de recolección de elementos no utilizados todavía no se ha completado. Aunque es posible que la revisión solucione parte del problema, el problema de recolección de elementos no utilizados podría seguir provocando un retraso en el procesamiento de streaming.

## <a name="resolution"></a>Resolución

Aplique la corrección [HADOOP-15547](https://issues.apache.org/jira/browse/HADOOP-15547). Si eso no es posible, puede usar HDFS como ubicación del punto de comprobación. Establezca `checkpointDirectory` en algo parecido a `hdfs://mycluster/checkpoint`.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para más información, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
