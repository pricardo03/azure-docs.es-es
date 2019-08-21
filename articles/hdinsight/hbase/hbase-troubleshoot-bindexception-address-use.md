---
title: 'BindException: la dirección ya está en uso en Azure HDInsight'
description: 'BindException: la dirección ya está en uso en Azure HDInsight'
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/08/2019
ms.openlocfilehash: 8851a4dfb7deafab7ad77ef80619dd49ca46ed71
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947931"
---
# <a name="scenario-bindexception---address-already-in-use-in-azure-hdinsight"></a>Escenario: BindException: la dirección ya está en uso en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

La operación de reinicio en un servidor regional de Apache HBase no se completa. Desde `region-server.log` en el directorio `/var/log/hbase` en los nodos de trabajo en los que se produce un error al iniciar el servidor regional, es posible que vea un mensaje de error parecido al siguiente:

```
Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
...

Caused by: java.net.BindException: Address already in use
...
```

## <a name="cause"></a>Causa

Reinicio de los servidores regionales de HBase durante una actividad de carga de trabajo intensa. Esto es lo que ocurre en segundo plano cuando un usuario inicia la operación de reinicio en el servidor regional de HBase desde la interfaz de usuario de Ambari:

1. El agente de Ambari enviará una solicitud de detención al servidor de regiones.

1. Después, el agente de Ambari espera 30 segundos a que el servidor regional se cierre correctamente.

1. Si la aplicación continúa conectándose con el servidor de regiones, no se apagará inmediatamente y, por tanto, el tiempo de espera de 30 segundos expira antes.

1. Tras la expiración de los 30 segundos, el agente de Ambari envía una terminación forzada (kill -9) al servidor de regiones.

1. Debido a este cierre inesperado, no se puede liberar el puerto asociado con el proceso, aunque se termine el proceso del servidor regional, lo cual conduce a `AddressBindException`.

## <a name="resolution"></a>Resolución

Reduzca la carga en los servidores regionales de HBase antes de iniciar un reinicio.

Como alternativa, pruebe a reiniciar manualmente los servidores regionales en los nodos de trabajo con los siguientes comandos:

```bash
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"
```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure, para mejorar la experiencia del cliente. Esta cuenta conecta a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
