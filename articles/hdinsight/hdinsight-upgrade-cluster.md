---
title: Actualización de clústeres de HDInsight a una versión más reciente en Azure
description: Conozca las directrices para actualizar el clúster de Azure HDInsight a una versión más reciente.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/04/2017
ms.openlocfilehash: 808e592a01bca2066b7d3d37a5fdfa697d739d4b
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71122442"
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>Actualización del clúster de HDInsight a una versión más reciente

Para aprovechar las ventajas de las características más recientes de HDInsight, se recomienda que los clústeres de HDInsight se actualicen a la versión más reciente. Siga las directrices que aparecen a continuación para actualizar las versiones del clúster de HDInsight.

> [!NOTE]  
> Para más información sobre las versiones admitidas de HDInsight, consulte [Versiones de los componentes de HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="upgrade-tasks"></a>Tareas de actualización

El flujo de trabajo para actualizar el clúster de HDInsight es el siguiente.
![Diagrama de flujo de trabajo de actualización de HDInsight](./media/hdinsight-upgrade-cluster/upgrade-workflow-diagram.png)

1. Lea cada sección de este documento para entender los cambios que pueden ser necesarios al actualizar el clúster de HDInsight.
2. Cree un clúster como entorno de control de calidad o de pruebas. Para más información sobre cómo crear un clúster, consulte [Más información sobre cómo crear clústeres de HDInsight basados en Linux](hdinsight-hadoop-provision-linux-clusters.md)
3. Copie los trabajos, los orígenes de datos y los receptores existentes en el nuevo entorno.
4. Realice pruebas de validación para asegurarse de que los trabajos funcionan como se esperaba en el nuevo clúster.

Cuando haya comprobado que todo funciona según lo esperado, programe el tiempo de inactividad para la migración. Durante este tiempo de inactividad, realice las acciones siguientes:

1.  Haga copia de seguridad de todos los datos transitorios almacenados localmente en los nodos del clúster. Por ejemplo, si tiene datos que se almacenan directamente en un nodo principal.
2.  Elimine el clúster existente.
3.  Cree un clúster en la misma subred de red virtual con la versión de HDI más reciente (o compatible) con el mismo almacén de datos predeterminado que usaba el clúster anterior. Esto permitirá que el nuevo clúster siga trabajando con los datos de producción existentes.
4.  Importe los datos transitorios cuya copia de seguridad realizó.
5.  Inicie trabajos o continúe el procesamiento con el nuevo clúster.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre cómo crear clústeres de HDInsight basado en Linux](hdinsight-hadoop-provision-linux-clusters.md)
* [Conexión a HDInsight mediante SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Administración de un clúster basado en Linux mediante Apache Ambari](hdinsight-hadoop-manage-ambari.md)
