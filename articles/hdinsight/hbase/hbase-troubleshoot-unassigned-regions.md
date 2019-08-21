---
title: Problemas con los servidores regionales en Azure HDInsight
description: Problemas con los servidores regionales en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/07/2019
ms.openlocfilehash: e75f2fdd0530b92e8c8405b74c2a364ff9e9e28e
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935463"
---
# <a name="issues-with-region-servers-in-azure-hdinsight"></a>Problemas con los servidores regionales en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="scenario-unassigned-regions"></a>Escenario: Regiones sin asignar

### <a name="issue"></a>Problema

Al ejecutar el comando `hbase hbck`, recibe un mensaje de error parecido a este:

```
multiple regions being unassigned or holes in the chain of regions
```

En la interfaz de usuario de Apache HBase Master se puede ver que el recuento de regiones está desequilibrado entre todos los servidores regionales.

### <a name="cause"></a>Causa

Los marcadores pueden ser el resultado de regiones sin conexión.

### <a name="resolution"></a>Resolución

Corrija las asignaciones. Siga los pasos siguientes para volver a poner las regiones sin asignar en el estado normal:

1. Inicie sesión a través de SSH en el clúster de HBase para HDInsight.

1. Ejecute el comando `hbase zkcli` para conectarse al shell de Zookeeper.

1. Ejecute el comando `rmr /hbase/regions-in-transition` o `rmr /hbase-unsecure/regions-in-transition`.

1. Salga del shell de ZooKeeper mediante el comando `exit`.

1. Abra la interfaz de usuario de Ambari y reinicie el servicio Active HBase Master desde Ambari.

1. Ejecute de nuevo el comando `hbase hbck` (sin ninguna opción adicional). Compruebe la salida del comando para asegurarse de que se asignan todas las regiones.

---

## <a name="scenario-dead-region-servers"></a>Escenario: Servidores regionales que no responden

### <a name="issue"></a>Problema

Los servidores regionales no se pueden iniciar.

### <a name="cause"></a>Causa

Varios directorios de registros de escritura previa (WAL) de división.

1. Obtenga la lista actual de estos registros: `hadoop fs -ls -R /hbase/WALs/ > /tmp/wals.out`.

1. Inspeccione el archivo `wals.out`. Si hay demasiados directorios de división (aquellos que empiezan por *-splitting), es probable que el servidor regional no funcione debido a estos directorios.

### <a name="resolution"></a>Resolución

1. Detenga HBase en el portal de Ambari.

1. Ejecute `hadoop fs -ls -R /hbase/WALs/ > /tmp/wals.out` para obtener una lista actualizada de los directorios de WAL.

1. Mueva los directorios *-splitting a una carpeta temporal, `splitWAL`, y elimine los directorios *-splitting.

1. Ejecute el comando `hbase zkcli` para conectarse al shell de ZooKeeper.

1. Ejecute `rmr /hbase-unsecure/splitWAL`.

1. Reinicie el servicio de HBase.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure, para mejorar la experiencia del cliente. Esta cuenta conecta a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
