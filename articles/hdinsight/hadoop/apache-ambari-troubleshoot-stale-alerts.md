---
title: Alertas obsoletas de Apache Ambari en Azure HDInsight
description: Debate y análisis de posibles causas y soluciones para las alertas obsoletas de Apache Ambari en HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: f19d499b5e50fbb5030a0f396296eed46fc6eee3
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722668"
---
# <a name="scenario-apache-ambari-stale-alerts-in-azure-hdinsight"></a>Escenario: Alertas obsoletas de Apache Ambari en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Desde la interfaz de usuario de Apache Ambari, es posible que vea una alerta similar a la siguiente:

![Ejemplo de alertas obsoletas de Apache Ambari](./media/apache-ambari-troubleshoot-stale-alerts/ambari-stale-alerts-example.png)

## <a name="cause"></a>Causa

Los agentes de Ambari ejecutan continuamente comprobaciones para supervisar el estado de muchos recursos. Cada alerta se configura para que se ejecute a intervalos predefinidos. Después de la ejecución de cada alerta, los agentes de Ambari informan del estado al servidor de Ambari. En este punto, si el servidor de Ambari detecta que alguna de las alertas no se ejecutó a tiempo, desencadena una "alerta de servidor de Ambari". Hay varios motivos por los que es posible que una comprobación de estado no se ejecute en su intervalo definido:

* Cuando se somete a los hosts a un uso intensivo (uso elevado de la CPU), existe la posibilidad de que el agente de Ambari no pueda obtener suficientes recursos del sistema para ejecutar las alertas de forma oportuna.

* El clúster está ocupado ejecutando muchos trabajos o servicios durante una carga intensiva.

* Algunos hosts del clúster pueden hospedar muchos componentes y, por lo tanto, serán necesarios para ejecutar muchas alertas. Si el número de componentes es grande, es posible que los trabajos de alerta pierdan los intervalos programados.

## <a name="resolution"></a>Solución

### <a name="increase-alert-interval-time"></a>Aumento del tiempo de intervalo de alerta

Puede optar por aumentar el valor de un intervalo de alerta en función del tiempo de respuesta del clúster y de su carga.

1. En la interfaz de usuario de Apache Ambari, seleccione la pestaña **Alerts** (Alertas).
1. Seleccione el nombre de la definición de alerta que desee.
1. En la definición, seleccione **Edit** (Editar).
1. Modifique el valor de **Check Interval** (Intervalo de comprobación) según sea necesario y, después, seleccione **Save** (Guardar).

### <a name="increase-alert-interval-time-for-ambari-server-alerts"></a>Aumento del tiempo de intervalo de alerta para las alertas del servidor Ambari

1. En la interfaz de usuario de Apache Ambari, seleccione la pestaña **Alerts** (Alertas).
1. En la lista desplegable **Groups** (Grupos), seleccione **AMBARI Default** (Valor predeterminado de AMBARI).
1. Seleccione **Ambari Server Alerts** (Alertas del servidor Ambari).
1. En la definición, seleccione **Edit** (Editar).
1. Modifique el valor de **Check Interval** (Intervalo de comprobación) según sea necesario.
1. Modifique el valor de **Interval Multiplier** (Multiplicador de intervalo) según sea necesario y, después, seleccione **Save** (Guardar).

### <a name="disable-and-enable-the-alert"></a>Deshabilitación y habilitación de la alerta

Puede deshabilitar y volver a habilitar la alerta para descartar las alertas obsoletas.

1. En la interfaz de usuario de Apache Ambari, seleccione la pestaña **Alerts** (Alertas).
1. Seleccione el nombre de la definición de alerta que desee.
1. En la definición, seleccione **Enabled** (Habilitado), que se encuentra en el extremo derecho.
1. En la ventana emergente **Confirmation** (Confirmación), seleccione **Confirm Disable** (Confirmar deshabilitación).
1. Espere unos segundos a que se borren todas las "instancias" de la alerta que se muestran en la página.
1. En la definición, seleccione **Disabled** (Deshabilitado), que se encuentra en el extremo derecho.
1. En la ventana emergente **Confirmation** (Confirmación), seleccione **Confirm Enable** (Confirmar habilitación).

### <a name="increase-alert-grace-time"></a>Aumento del tiempo de gracia de la alerta

Antes de que el agente de Ambari informe de que una alerta configurada no ha seguido su programación, se aplica un período de gracia. Incluso si la alerta no siguió su programación pero se desencadenó en el periodo de gracia de la alerta, la alerta obsoleta no se activa.

El valor predeterminado de `alert_grace_period` es 5 segundos. Este valor de `alert_grace_period` puede configurarse en `/etc/ambari-agent/conf/ambari-agent.ini`. En los hosts desde los que se desencadenen alertas obsoletas a intervalos regulares, intente aumentar el valor a 10. Después, reinicie el agente de Ambari.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
