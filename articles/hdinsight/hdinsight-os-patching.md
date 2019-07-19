---
title: Configuración de la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux para Azure
description: Aprenda a configurar la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: a73866a8898042b546fa47d9c3d14ab4e58e9a12
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503241"
---
# <a name="os-patching-for-hdinsight"></a>Aplicación de revisión del SO para HDInsight 

> [!IMPORTANT]
> Las imágenes de Ubuntu estarán disponibles para la creación del nuevo clúster de HDInsight tres meses después de publicarse. A partir de enero de 2019, la ejecución de clústeres **no** se revisa automáticamente. Los clientes deben usar acciones de script u otros mecanismos para revisar un clúster en ejecución. Los clústeres creados recientemente siempre tendrán las últimas actualizaciones disponibles, incluidas las revisiones de seguridad más recientes.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Configuración de la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux
Las máquinas virtuales de un clúster de HDInsight se deben reiniciar de forma ocasional para poder instalar las revisiones de seguridad importantes. 

Con las acciones de script que se describen en este artículo, puede modificar la programación de aplicación de revisión del SO de la siguiente manera:
1. Instale todas las actualizaciones, solo las actualizaciones del kernel+seguridad o solo las actualizaciones del kernel.
2. Reinicie inmediatamente o programe un reinicio en la máquina virtual.

> [!NOTE]  
> Estas acciones de script solo funcionarán con clústeres de HDInsight basado en Linux que se hayan creado después del 1 de agosto de 2016. Las revisiones solo entrarán en vigor una vez que se reinicien las máquinas virtuales. Estos scripts no aplicarán automáticamente actualizaciones para todos los ciclos de actualización futuros. Ejecute los scripts cada vez que sea necesario aplicar nuevas actualizaciones para instalar las actualizaciones y reiniciar la máquina virtual.

## <a name="how-to-use-the-script"></a>Uso del script 

El uso de este script requiere la información siguiente:
1. Ubicación del script install-updates-schedule-reboots: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight usa este URI para encontrar y ejecutar el script en todas las máquinas virtuales del clúster. Este script proporciona opciones para instalar las actualizaciones y reiniciar la máquina virtual.
  
2. La ubicación del script schedule-reboots: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight usa este URI para encontrar y ejecutar el script en todas las máquinas virtuales del clúster. Este script reinicia la máquina virtual.
  
3. Los tipos de nodo de clúster al que se aplica el script: headnode, workernode, zookeeper. Este script se debe aplicar a todos los tipos de nodo del clúster. Si no se aplica a un tipo de nodo, las máquinas virtuales de ese tipo de nodo no se actualizarán o reiniciarán.

4. Parámetro: El script install-updates-schedule-reboots acepta dos parámetros numéricos:

    | Parámetro | Definición |
    | --- | --- |
    | Instalar solo las actualizaciones del kernel/Instalar todas las actualizaciones/Instalar solo las actualizaciones del kernel+seguridad |0, 1 o 2. El valor 0 instala solo las actualizaciones del kernel, el valor 1 instala todas las actualizaciones y el valor 2 instala solo las actualizaciones del kernel+seguridad. Si no se proporciona ningún parámetro, el predeterminado es 0. |
    | No reiniciar/Habilitar reinicio programado/Habilitar reinicio inmediato |0, 1 o 2. El valor 0 deshabilita el reinicio, 1 habilita el reinicio programado y 2 habilita el reinicio inmediato. Si no se proporciona ningún parámetro, el predeterminado es 0. El usuario debe escribir el parámetro 1 para especificar el parámetro 2. |
   
 5. Parámetro: El script schedule-reboots acepta un parámetro numérico:

    | Parámetro | Definición |
    | --- | --- |
    | Habilitar el reinicio programado/Habilitar el reinicio inmediato |1 o 2. El valor 1 habilita el reinicio programado (el reinicio se programa en las próximas 12 a 24 horas) y el valor 2 habilita el reinicio inmediato (en 5 minutos). Si no se proporciona ningún parámetro, el predeterminado es 1. |  

> [!NOTE] 
> Debe marcar el script como resistente cuando lo aplique en un clúster existente. De lo contrario, en todos los nodos que se creen mediante operaciones de escalado se usará la programación de aplicación de revisión predeterminada.  Si aplica el script como parte del proceso de creación de clústeres, se marcará como persistente de forma automática.


## <a name="next-steps"></a>Pasos siguientes

Para los pasos específicos sobre cómo usar la acción de script, consulte las secciones siguientes en [Personalización de clústeres de HDInsight de Azure mediante el uso de las acciones de script](hdinsight-hadoop-customize-cluster-linux.md):

* [Uso de una acción de script durante la creación de un clúster](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Aplicación de una acción de script a un clúster en ejecución](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
