---
title: Configuración de la programación de revisión del SO para clústeres de Azure HDInsight
description: Aprenda a configurar la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: a97a03f7ef20ae56cec04341fe76b79ee657547b
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748485"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Configuración de la programación de aplicación de revisiones del SO para clústeres de HDInsight basado en Linux

> [!IMPORTANT]
> Las imágenes de Ubuntu estarán disponibles para la creación del clúster de HDInsight tres meses después de publicarse. A partir de enero de 2019, la ejecución de clústeres no se revisa automáticamente. Los clientes deben usar acciones de script u otros mecanismos para revisar un clúster en ejecución. Los clústeres creados recientemente siempre tendrán las últimas actualizaciones disponibles, incluidas las revisiones de seguridad más recientes.

HDInsight permite realizar tareas comunes en el clúster, como instalar revisiones del sistema operativo, aplicar actualizaciones de seguridad y reiniciar nodos. Estas tareas se realizan mediante los dos scripts siguientes que se pueden ejecutar como [acciones de script](hdinsight-hadoop-customize-cluster-linux.md) y que se pueden configurar con parámetros:

- `schedule-reboots.sh` - Realice un reinicio inmediato o programe un reinicio en los nodos de clúster.
- `install-updates-schedule-reboots.sh` - Instale todas las actualizaciones, solo las actualizaciones del kernel más las de seguridad o solo las actualizaciones del kernel.

> [!NOTE]  
> Las acciones de script no aplicarán automáticamente actualizaciones en todos los ciclos futuros de actualización. Ejecute los scripts cada vez que se deban aplicar nuevas actualizaciones para instalar las actualizaciones y, luego, reinicie la máquina virtual.

## <a name="restart-nodes"></a>Reinicio de nodos
  
El script [schedule-reboots](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh) establece el tipo de reinicio que se realizará en las máquinas en el clúster. Al enviar la acción de script, configúrela para que se aplique en los tres tipos de nodo: nodo principal, nodo de trabajo y ZooKeeper. Si el script no se aplica a un tipo de nodo, las máquinas virtuales de ese tipo de nodo no se actualizarán ni se reiniciarán.

El script `schedule-reboots script` acepta un parámetro numérico:

| Parámetro | Valores aceptados | Definición |
| --- | --- | --- |
| Tipo de reinicio que se va a realizar | 1 o 2 | Un valor de 1 habilita el reinicio programado (en 12 o 24 horas). Un valor de 2 habilita el reinicio inmediato (en 5 minutos). Si no se especifica ningún parámetro, el valor predeterminado es 1. |  

## <a name="install-updates-and-restart-nodes"></a>Instalación de actualizaciones y reinicio de nodos

El script [install-updates-schedule-reboots.sh](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh) proporciona opciones para instalar distintos tipos de actualizaciones y reiniciar la máquina virtual.

El script `install-updates-schedule-reboots` acepta dos parámetros numéricos, como se describe en la siguiente tabla:

| Parámetro | Valores aceptados | Definición |
| --- | --- | --- |
| Tipo de actualizaciones que se van a instalar | 0, 1 o 2 | Un valor de 0 instala solo las actualizaciones del kernel. Un valor de 1 instala todas las actualizaciones y un valor de 2 solo instala las actualizaciones del kernel más las de seguridad. Si no se proporciona ningún parámetro, el predeterminado es 0. |
| Tipo de reinicio que se va a realizar | 0, 1 o 2 | Un valor de 0 deshabilita el reinicio. Un valor de 1 habilita el reinicio programado y un valor de 2 habilita el reinicio inmediato. Si no se proporciona ningún parámetro, el predeterminado es 0. El usuario debe cambiar el parámetro de entrada 1 por el parámetro de entrada 2. |

> [!NOTE]
> Debe marcar un script como persistente después de aplicarlo a un clúster existente. De lo contrario, en todos los nodos que se creen mediante operaciones de escalado se usará la programación de aplicación de revisión predeterminada. Si aplica el script como parte del proceso de creación del clúster, se marcará como persistente de forma automática.

## <a name="next-steps"></a>Pasos siguientes

Para conocer los pasos específicos sobre cómo usar las acciones de script, consulte las secciones siguientes en [Personalización de los clústeres de HDInsight basado en Linux mediante acciones de script](hdinsight-hadoop-customize-cluster-linux.md):

* [Uso de una acción de script durante la creación de un clúster](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Aplicación de una acción de script a un clúster en ejecución](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
