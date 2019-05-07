---
title: HDInsight de Azure Accelerated escrituras de HBase de Apache
description: Proporciona información general de la característica escribe acelerado de Azure HDInsight, que utiliza discos administrados premium para mejorar el rendimiento del registro de escritura anticipada de Apache HBase.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 4/29/2019
ms.openlocfilehash: f9314355d3dce8d96b794cdb81ab9c3d77faab45
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159306"
---
# <a name="azure-hdinsight-accelerated-writes-for-apache-hbase"></a>HDInsight de Azure Accelerated escrituras de HBase de Apache

En este artículo proporciona información general acerca del **Accelerated escribe** característica para Apache HBase en HDInsight de Azure y cómo se puede usar eficazmente para mejorar el rendimiento de escritura. **Accelerated escrituras** usa [discos administrados premium de Azure SSD](../../virtual-machines/linux/disks-types.md#premium-ssd) para mejorar el rendimiento de la Apache HBase de escritura anticipada Log (WAL). Para obtener más información acerca de Apache HBase, consulte [¿qué es Apache HBase en HDInsight](apache-hbase-overview.md).

## <a name="overview-of-hbase-architecture"></a>Información general de arquitectura de HBase

En HBase, una **fila** consta de uno o varios **columnas** y se identifica mediante un **clave de fila**. Varias filas constituyen un **tabla**. Las columnas contienen **celdas**, que son versiones con marca de tiempo del valor de esa columna. Las columnas se agrupan en **familias de columnas**, y todas las columnas de una familia de columnas se almacenan juntos en archivos de almacenamiento denominados **HFiles**.

**Regiones** en HBase se usan para equilibrar la carga de procesamiento de datos. En primer lugar, HBase almacena las filas de una tabla en una sola región. Las filas se distribuyen entre varias regiones, como la cantidad de datos en la que aumenta la tabla. **Los servidores de regiones** puede controlar las solicitudes de varias regiones.

## <a name="write-ahead-log-for-apache-hbase"></a>Registro de escritura previa para Apache HBase

HBase escribe primero las actualizaciones de datos en un tipo de registro de confirmación denominado una escritura anticipada Log (WAL). Después de la actualización se almacena en el WAL, se escribe en la memoria **MemStore**. Cuando los datos en memoria alcanza su capacidad máxima, ha escrito en el disco como un **HFile**.

Si un **RegionServer** se bloquea o deja de estar disponible antes de que se vacía, se puede usar el registro de escritura anticipada para reproducir las actualizaciones. Sin el WAL, si un **RegionServer** se bloquea antes del vaciado de las actualizaciones de un **HFile**, se pierden todas esas actualizaciones.

## <a name="accelerated-writes-feature-in-azure-hdinsight-for-apache-hbase"></a>Característica de escrituras acelerado en Azure HDInsight para Apache HBase

La característica Accelerated escribe resuelve el problema de mayores latencias de escritura deberse al uso de registros de escritura previa que se encuentran en el almacenamiento en la nube.  Clústeres de la característica escribe acelerado para HDInsight Apache HBase, adjunta SSD-managed disks premium para cada RegionServer (nodo de trabajo). Escribir que registros con antelación, a continuación, se escriben en el sistema de archivos de Hadoop (HDFS) montado en estos discos administrados premium: en lugar de almacenamiento en la nube.  Discos administrados Premium utilizan discos de estado sólido (SSD) y ofrecen un excelente rendimiento de E/S con tolerancia a errores.  A diferencia de los discos no administrados, si una unidad de almacenamiento se bloquea, lo no afectará a otras unidades de almacenamiento en el mismo conjunto de disponibilidad.  Como resultado, discos administrados proporcionan una mejor resistencia para las aplicaciones y baja latencia de escritura. Para más información acerca de los discos administrados de Azure, consulte [de introducción a Azure managed disks](../../virtual-machines/windows/managed-disks-overview.md). 

## <a name="how-to-enable-accelerated-writes-for-hbase-in-hdinsight"></a>Cómo habilitar Accelerated escribe para HBase en HDInsight

Para crear un nuevo clúster de HBase con la característica Accelerated escribe, siga los pasos de [configuración de clústeres en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) hasta llegar a **paso 3, almacenamiento**. En **configuración de Metastore**, haga clic en la casilla de verificación junto a **habilitar Accelerated escribe (versión preliminar)**. A continuación, continúe con los pasos restantes para crear un clúster.

![Habilitar la opción de escrituras acelerado para Apache HBase de HDInsight](./media/apache-hbase-accelerated-writes/accelerated-writes-cluster-creation.png)

## <a name="other-considerations"></a>Otras consideraciones

Para conservar la durabilidad de los datos, crear un clúster con un mínimo de tres nodos de trabajo. Una vez creado, no se puede escalar verticalmente el clúster a menos de tres nodos de trabajo.

Vaciar o deshabilitar las tablas de HBase antes de eliminar el clúster, para que no pierda datos escribir directamente el registro.

```
flush 'mytable'
```

```
disable 'mytable'
```

## <a name="next-steps"></a>Pasos siguientes

* Documentación de Apache HBase oficial sobre el [característica escribir directamente el registro](https://hbase.apache.org/book.html#wal)
* Para actualizar el clúster de Apache HBase de HDInsight para usar Accelerated escribe, vea [migrar un clúster de Apache HBase a una nueva versión](apache-hbase-migrate-new-version.md).
