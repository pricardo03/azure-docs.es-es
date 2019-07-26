---
title: 'Configuración de la programación de aplicación de revisiones del SO para clústeres de HDInsight basado en Linux: Azure'
description: Aprenda a configurar la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: efe74618b269000749f7ba6c24d35903e540dcfb
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657053"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Configuración de la programación de aplicación de revisiones del SO para clústeres de HDInsight basado en Linux 

> [!IMPORTANT]
> Las imágenes de Ubuntu estarán disponibles para la creación del clúster de HDInsight tres meses después de publicarse. A partir de enero de 2019, la ejecución de clústeres no se revisa automáticamente. Los clientes deben usar acciones de script u otros mecanismos para revisar un clúster en ejecución. Los clústeres creados recientemente siempre tendrán las últimas actualizaciones disponibles, incluidas las revisiones de seguridad más recientes.

En ocasiones, debe reiniciar las máquinas virtuales (VM) de un clúster de HDInsight para instalar revisiones de seguridad importantes.

Con las acciones de script que se describen en este artículo, puede modificar la programación de aplicación de revisiones del SO de la siguiente manera:

1. Instale todas las actualizaciones, instale las actualizaciones del kernel más las de seguridad o solo las actualizaciones del kernel.
2. Realice un reinicio inmediato o programe un reinicio en la máquina virtual.

> [!NOTE]  
> Las acciones de script que se describen en este artículo solo funcionarán con clústeres de HDInsight basado en Linux creados después del 1 de agosto de 2016. Las revisiones solo son efectivas después de reiniciar las máquinas virtuales.
> Las acciones de script no aplicarán automáticamente actualizaciones en todos los ciclos futuros de actualización. Ejecute los scripts cada vez que se deban aplicar nuevas actualizaciones para instalar las actualizaciones y, luego, reinicie la máquina virtual.

## <a name="add-information-to-the-script"></a>Adición de información al script

Para usar este script, se requiere la información siguiente:

- Ubicación del script install-updates-schedule-reboots: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight usa este URI para encontrar y ejecutar el script en todas las máquinas virtuales del clúster. Este script proporciona opciones para instalar actualizaciones y reiniciar la máquina virtual.
  
- La ubicación del script schedule-reboots: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight usa este URI para encontrar y ejecutar el script en todas las máquinas virtuales del clúster. Este script reinicia la máquina virtual.
  
- Los tipos de nodo del clúster a los que se aplica el script son headnode, workernode y zookeeper. Aplique el script a todos los tipos de nodo del clúster. Si el script no se aplica a un tipo de nodo, las máquinas virtuales de ese tipo de nodo no se actualizarán ni se reiniciarán.

- El script install-updates-schedule-reboots acepta dos parámetros numéricos:

    | Parámetro | Definición |
    | --- | --- |
    | Instalar solo las actualizaciones del kernel, instalar todas las actualizaciones o instalar solo las actualizaciones del kernel más las de seguridad|0, 1 o 2 Un valor de 0 instala solo las actualizaciones del kernel. Un valor de 1 instala todas las actualizaciones y un valor de 2 solo instala las actualizaciones del kernel más las de seguridad. Si no se proporciona ningún parámetro, el predeterminado es 0. |
    | No reiniciar/Habilitar reinicio programado/Habilitar reinicio inmediato |0, 1 o 2 Un valor de 0 deshabilita el reinicio. Un valor de 1 habilita el reinicio programado y un valor de 2 habilita el reinicio inmediato. Si no se proporciona ningún parámetro, el predeterminado es 0. El usuario debe cambiar el parámetro de entrada 1 por el parámetro de entrada 2. |
   
 - El script schedule-reboots acepta un parámetro numérico:

    | Parámetro | Definición |
    | --- | --- |
    | Habilitar el reinicio programado/Habilitar el reinicio inmediato |1 o 2. Un valor de 1 habilita el reinicio programado (en 12 o 24 horas). Un valor de 2 habilita el reinicio inmediato (en 5 minutos). Si no se especifica ningún parámetro, el valor predeterminado es 1. |  

> [!NOTE]
> Debe marcar un script como persistente después de aplicarlo a un clúster existente. De lo contrario, en todos los nodos que se creen mediante operaciones de escalado se usará la programación de aplicación de revisión predeterminada. Si aplica el script como parte del proceso de creación del clúster, se marcará como persistente de forma automática.


## <a name="next-steps"></a>Pasos siguientes

Para conocer los pasos específicos sobre cómo usar las acciones de script, consulte las secciones siguientes en [Personalización de los clústeres de HDInsight basado en Linux mediante acciones de script](hdinsight-hadoop-customize-cluster-linux.md):

* [Uso de una acción de script durante la creación de un clúster](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Aplicación de una acción de script a un clúster en ejecución](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
