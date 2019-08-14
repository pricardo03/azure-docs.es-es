---
title: Tiempos de espera con el comando "hbase hbck" en Azure HDInsight
description: Problema de tiempo de espera con el comando "hbase hbck" al corregir las asignaciones de región
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/01/2019
ms.openlocfilehash: 4f3f1c22fa1dc05a66a8b6bf0179903a44cef9b6
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2019
ms.locfileid: "68737434"
---
# <a name="scenario-timeouts-with-hbase-hbck-command-in-azure-hdinsight"></a>Escenario: Tiempos de espera con el comando "hbase hbck" en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Se producen tiempos de espera con el comando `hbase hbck` al corregir las asignaciones de regiones.

## <a name="cause"></a>Causa

Aquí la posible causa podría ser que varias regiones estén en el estado "en transición" durante mucho tiempo. Esas regiones pueden verse como sin conexión desde la interfaz de usuario de Apache HBase Master. Debido al gran número de regiones que tratan de realizar la transición, HBase Master podría superar el tiempo de espera y no ser capaz de volver a poner esas regiones en estado en línea.

## <a name="resolution"></a>Resolución

1. Inicie sesión a través de SSH en el clúster de HBase para HDInsight.

1. Ejecute el comando `hbase zkcli` para conectarse al shell de Zookeeper.

1. Ejecute el comando `rmr /hbase/regions-in-transition` o `rmr /hbase-unsecure/regions-in-transition`.

1. Salga del shell `hbase zkcli` mediante el comando `exit`.

1. Abra la interfaz de usuario de Ambari y reinicie el servicio Active HBase Master desde Ambari.

1. Supervise la "región en transición" de la interfaz de usuario de HBase Master para asegurarse de que no se bloquee ninguna región.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener más información, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
