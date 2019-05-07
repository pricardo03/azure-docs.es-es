---
title: Configuración de la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux para Azure
description: Aprenda a configurar la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux.
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: 5b8ed75863087e077d483c792ac4134a0c3e1eb0
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65203638"
---
# <a name="os-patching-for-hdinsight"></a>Aplicación de revisión del SO para HDInsight 

> [!IMPORTANT]
> Las imágenes de Ubuntu estarán disponibles para la creación del nuevo clúster de HDInsight tres meses después de publicarse. A partir de enero de 2019, la ejecución de clústeres **no** se revisa automáticamente. Los clientes deben usar acciones de script u otros mecanismos para revisar un clúster en ejecución. Los clústeres creados recientemente siempre tendrán las últimas actualizaciones disponibles, incluidas las revisiones de seguridad más recientes.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Configuración de la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux
Las máquinas virtuales de un clúster de HDInsight se deben reiniciar de forma ocasional para poder instalar las revisiones de seguridad importantes. 

Con la acción de script que se describe en este artículo, puede modificar la programación de aplicación de revisión del SO de la siguiente manera:
1. Instalar actualizaciones del sistema operativo completas o instalar las actualizaciones de seguridad
2. Reinicie la máquina virtual

> [!NOTE]  
> Esta acción de script solo funcionará con clústeres de HDInsight basado en Linux que se hayan creado después del 1 de agosto de 2016. Las revisiones solo entrarán en vigor una vez que se reinicien las máquinas virtuales. Este script no aplicará automáticamente actualizaciones para todos los ciclos de actualización futura. Ejecute el script que cada nuevas actualizaciones de tiempo deben aplicarse para poder instalar las actualizaciones y reinicie la máquina virtual.

## <a name="how-to-use-the-script"></a>Uso del script 

Cuando se usa este script, se requiere la información siguiente:
1. Ubicación del script: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/os-patching-reboot-config.sh.  HDInsight usa este URI para encontrar y ejecutar el script en todas las máquinas virtuales del clúster.
  
2. Los tipos de nodo de clúster al que se aplica el script: headnode, workernode, zookeeper. Este script se debe aplicar a todos los tipos de nodo del clúster. Si no se aplica a un tipo de nodo, no se actualizará las máquinas virtuales para ese tipo de nodo.


3.  Parámetro: Este script acepta un parámetro numérico:

    | Parámetro | Definición |
    | --- | --- |
    | Sistema operativo completo de instalar las actualizaciones o instalar las actualizaciones de seguridad |0 o 1. Un valor de 0 instala las actualizaciones de seguridad mientras que 1 instala actualizaciones del sistema operativo completas. Si no se proporciona ningún parámetro, que el valor predeterminado es 0. |

> [!NOTE]  
> Debe marcar este script como resistente cuando lo aplique en un clúster existente. De lo contrario, en todos los nodos que se creen mediante operaciones de escalado se usará la programación de aplicación de revisión predeterminada.  Si aplica el script como parte del proceso de creación de clústeres, se marcará como persistente de forma automática.

## <a name="next-steps"></a>Pasos siguientes

Para los pasos específicos sobre cómo usar la acción de script, consulte las secciones siguiente en [Personalización de clústeres de HDInsight basado en Linux mediante la acción de scripts](hdinsight-hadoop-customize-cluster-linux.md):

* [Uso de una acción de script durante la creación de un clúster](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Aplicación de una acción de script a un clúster en ejecución](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
