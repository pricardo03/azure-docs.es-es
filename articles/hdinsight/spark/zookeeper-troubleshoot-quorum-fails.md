---
title: El servidor Apache ZooKeeper no puede formar un cuórum en Azure HDInsight
description: El servidor Apache ZooKeeper no puede formar un cuórum en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/20/2019
ms.openlocfilehash: 3fec745413e8f27571789fb6191c1d949e7b9900
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69901284"
---
# <a name="apache-zookeeper-server-fails-to-form-a-quorum-in-azure-hdinsight"></a>El servidor Apache ZooKeeper no puede formar un cuórum en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

El estado del servidor Apache ZooKeeper no es correcto. Los causas podrían ser las siguientes: los nodos de nombre o los administradores de recursos se encuentran en modo de espera; las operaciones de HDFS simples no funcionan; `zkFailoverController` está detenido y no se puede iniciar; o bien los trabajos de Yarn, Spark o Livy generan errores debido a los errores de ZooKeeper. Es posible que vea un mensaje de error similar al siguiente:

```
19/06/19 08:27:08 ERROR ZooKeeperStateStore: Fatal Zookeeper error. Shutting down Livy server.
19/06/19 08:27:08 INFO LivyServer: Shutting down Livy server.
```

## <a name="cause"></a>Causa

Cuando el volumen de archivos de instantáneas es grande o los archivos de instantáneas están dañados, el servidor de ZooKeeper no podrá formar un cuórum, lo que provocará que el estado de los servicios relacionados con ZooKeeper sea incorrecto. El servidor de ZooKeeper no quitará los archivos de instantáneas antiguos de su directorio de datos, sino que se trata de una tarea periódica que realizarán los usuarios para mantener ZooKeeper en buen estado. Para obtener más información, consulte [Ventajas y limitaciones de ZooKeeper](https://zookeeper.apache.org/doc/r3.3.5/zookeeperAdmin.html#sc_strengthsAndLimitations).

## <a name="resolution"></a>Resolución

Compruebe los directorios de datos de ZooKeeper `/hadoop/zookeeper/version-2` y `/hadoop/hdinsight-zookeepe/version-2` para averiguar si el tamaño del archivo de instantáneas es grande. Siga los pasos siguientes si existen instantáneas de gran tamaño:

1. Realice copias de seguridad de las instantáneas en `/hadoop/zookeeper/version-2` y `/hadoop/hdinsight-zookeepe/version-2`.

1. Limpie las instantáneas en `/hadoop/zookeeper/version-2` y `/hadoop/hdinsight-zookeepe/version-2`.

1. Reinicie todos los servidores de ZooKeeper desde la interfaz de usuario de Apache Ambari.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta conecta a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

- Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
