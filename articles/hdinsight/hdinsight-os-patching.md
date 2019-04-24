---
title: Configuración de la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux para Azure
description: Aprenda a configurar la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux.
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: ef57608d092c05b30be63a54bb41ba87558eabc3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60333696"
---
# <a name="os-patching-for-hdinsight"></a>Aplicación de revisión del SO para HDInsight 

> [!IMPORTANT]
> Las imágenes de Ubuntu estarán disponibles para la creación del nuevo clúster de HDInsight tres meses después de publicarse. A partir de enero de 2019, la ejecución de clústeres **no** se revisa automáticamente. Los clientes deben usar acciones de script u otros mecanismos para revisar un clúster en ejecución. Los clústeres creados recientemente siempre tendrán las últimas actualizaciones disponibles, incluidas las revisiones de seguridad más recientes.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Configuración de la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux
Las máquinas virtuales de un clúster de HDInsight se deben reiniciar de forma ocasional para poder instalar las revisiones de seguridad importantes. 

Con la acción de script que se describe en este artículo, puede modificar la programación de aplicación de revisión del SO de la siguiente manera:
1. Habilite o deshabilite los reinicios automáticos.
2. Establezca la frecuencia de los reinicios (días entre reinicios).
3. Establezca el día de la semana en que se realizará un reinicio.

> [!NOTE]  
> Esta acción de script solo funcionará con clústeres de HDInsight basado en Linux que se hayan creado después del 1 de agosto de 2016. Las revisiones solo entrarán en vigor una vez que se reinicien las máquinas virtuales. 

## <a name="how-to-use-the-script"></a>Uso del script 

Cuando se usa este script, se requiere la información siguiente:
1. Ubicación del script: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight usa este URI para encontrar y ejecutar el script en todas las máquinas virtuales del clúster.
  
2. Los tipos de nodo de clúster al que se aplica el script: headnode, workernode, zookeeper. Este script se debe aplicar a todos los tipos de nodo del clúster. Si no se aplica a un tipo de nodo, las máquinas virtuales de ese tipo de nodo seguirán usando la programación de aplicación de revisión anterior.


3.  Parámetro: Este script acepta tres parámetros numéricos:

    | Parámetro | Definición |
    | --- | --- |
    | Habilite o deshabilite los reinicios automáticos |0 o 1. El valor 0 deshabilita los reinicios automáticos, mientras que 1 los habilita. |
    | Frecuencia |7 a 90 (incluido). La cantidad de días que se esperará antes de reiniciar las máquinas virtuales para las revisiones que requieren un reinicio. |
    | Día de la semana |1 a 7 (incluido). El valor 1 indica que el reinicio debe realizarse un lunes y el valor 7 indica que se debe realizar el domingo. Por ejemplo, si usa los parámetros 1 60 2, los reinicios automáticos se realizan cada 60 días (como máximo) los martes. |
    | Persistencia |Cuando aplica una acción de script en un clúster existente, puede marcar el script como persistente. Los scripts persistentes se aplican cuando los nuevos nodos workernode se agregan al clúster a través de operaciones de escalado. |

> [!NOTE]  
> Debe marcar este script como resistente cuando lo aplique en un clúster existente. De lo contrario, en todos los nodos que se creen mediante operaciones de escalado se usará la programación de aplicación de revisión predeterminada.  Si aplica el script como parte del proceso de creación de clústeres, se marcará como persistente de forma automática.

## <a name="next-steps"></a>Pasos siguientes

Para los pasos específicos sobre cómo usar la acción de script, consulte las secciones siguiente en [Personalización de clústeres de HDInsight basado en Linux mediante la acción de scripts](hdinsight-hadoop-customize-cluster-linux.md):

* [Uso de una acción de script durante la creación de un clúster](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Aplicación de una acción de script a un clúster en ejecución](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
