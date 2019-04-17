---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtener sugerencias sobre desarrollo y detalles de Hadoop, Spark, R Server, Hive y mucho más.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 6ba9e77faeb297f9862b39384d397b478dc7cf36
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617740"
---
# <a name="release-notes-for-azure-hdinsight"></a>Notas de la versión de Azure HDInsight.

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

> [!IMPORTANT]  
> Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Para obtener más información, consulte el [artículo de control de versiones de HDInsight](hdinsight-component-versioning.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto de Apache Hadoop y Apache Spark en Azure.

## <a name="new-features"></a>Nuevas características

Para obtener más información sobre los cambios importantes con HDInsight 4.0., vea [cuáles son las novedades en 4.0 HDI?](../hdinsight/hdinsight-version-release.md#whats-new-in-hdi-40).

## <a name="component-versions"></a>Versiones de componentes

Las versiones de Apache oficiales de todos los componentes de HDInsight 4.0 se indican a continuación. Los componentes enumerados son versiones de las versiones estables más recientes disponibles.

- Apache Ambari 2.7.1
- Apache Hadoop 3.1.1
- Apache HBase 2.0.0
- Apache Hive 3.1.0
- Apache Kafka 1.1.1
- Apache Mahout 0.9.0+
- Apache Oozie 4.2.0
- Apache Phoenix 4.7.0
- Apache Pig 0.16.0
- Apache Ranger 0.7.0
- Apache Slider 0.92.0
- Apache Spark 2.3.1
- Apache Sqoop 1.4.7
- Apache TEZ 0.9.1
- Apache Zeppeling 0.8.0
- Apache ZooKeeper 3.4.6

Las versiones posteriores de los componentes de Apache a veces están incluidas en la distribución HDP además de las versiones anteriores. En este caso, estas versiones posteriores se muestran en la tabla de versiones Technical Preview y no deben sustituir a las versiones de componentes de Apache de la lista anterior en un entorno de producción.

## <a name="apache-patch-information"></a>Información sobre la revisión de Apache

Para obtener más información sobre las revisiones disponibles en HDInsight 4.0, consulte la revisión de lista para cada producto en la tabla siguiente.

| Nombre de producto | Información de la revisión |
|---|---|
| Ambari | [Información acerca de la revisión de Ambari](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/bk_ambari-release-notes/content/ambari_relnotes-2.7.1.0-patch-information.html) |
| Hadoop | [Información de revisiones de Hadoop](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hadoop.html) |
| HBase | [Información acerca de la revisión de HBase](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hbase.html) |
| Hive  | Esta versión proporciona 3.1.0 con ningún revisiones adicionales de Apache Hive.  |
| Kafka | Esta versión proporciona a 1.1.1 con ningún revisiones adicionales de Apache Kafka. |
| Oozie | [Información acerca de la revisión de Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_oozie.html) |
| Phoenix | [Información acerca de la revisión de Phoenix](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_phoenix.html) |
| Pig | [Información acerca de la revisión de pig](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_pig.html) |
| Ranger | [Información acerca de la revisión de Ranger](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_ranger.html) |
| Spark | [Información acerca de la revisión de Spark](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_spark.html) |
| Sqoop | Esta versión proporciona 1.4.7 con ningún revisiones adicionales de Apache Sqoop. |
| Tez | Esta versión proporciona 0.9.1 con ningún revisiones adicionales de Apache Tez. |
| Zeppelin | Esta versión proporciona Zeppelin 0.8.0 con ninguna revisión de Apache adicional. |
| Zookeeper | [Información acerca de la revisión de zookeeper](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_zookeeper.html) |

## <a name="fixed-common-vulnerabilities-and-exposures"></a>Se han corregido las vulnerabilidades y exposiciones comunes

Para obtener más información sobre la seguridad de los problemas resueltos en esta versión, vea 'Hortonworks [fijo vulnerabilidades y exposiciones comunes para HDP 3.0.1](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/cve.html).

## <a name="known-issues"></a>Problemas conocidos

### <a name="replication-is-broken-for-secure-hbase-with-default-installation"></a>La replicación se interrumpe para proteger HBase con la instalación predeterminada

Para HDInsight 4.0, realice los pasos siguientes:

1. Habilitar la comunicación entre clúster.
1. Inicie sesión en el nodo principal activo.
1. Descargar un script para habilitar la replicación con el siguiente comando:

    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Escriba el comando `sudo kinit <domainuser>`.
1. Escriba el siguiente comando para ejecutar el script:

    ```
    sudo bash hdi_enable_replication.sh -m <hn0> -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```
Para HDInsight 3.6, realice lo siguiente:

1. Inicie sesión en active ZK HMaster.
1. Descargar un script para habilitar la replicación con el siguiente comando:
    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Escriba el comando `sudo kinit -k -t /etc/security/keytabs/hbase.service.keytab hbase/<FQDN>@<DOMAIN>`.
1. Escriba el siguiente comando: 

    ```bash
    sudo bash hdi_enable_replication.sh -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```

### <a name="phoenix-sqlline-stops-working-after-migrating-hbase-cluster-to-hdinsight-40"></a>Phoenix Sqlline deja de funcionar después de migrar el clúster de HBase para HDInsight 4.0

Siga estos pasos:

1. Quitar las siguientes tablas de Phoenix:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.MUTEX`
    1. `SYSTEM.CATALOG`
1. Si no se puede eliminar cualquiera de las tablas, reinicie HBase para borrar las conexiones a las tablas.
1. Vuelva a ejecutar `sqlline.py`. Phoenix volverá a crear todas las tablas que se eliminaron en el paso 1.
1. Volver a generar las tablas de Phoenix y vistas para los datos de HBase.

### <a name="phoenix-sqlline-stops-working-after-replicating-hbase-phoenix-metadata-from-hdinsight-36-to-40"></a>Phoenix Sqlline deja de funcionar después de la replicación de metadatos de HBase Phoenix de HDInsight 3.6 a 4.0

Siga estos pasos:

1. Antes de realizar la replicación, vaya al clúster de destino 4.0 y ejecute `sqlline.py`. Este comando generará las tablas de Phoenix como `SYSTEM.MUTEX` y `SYSTEM.LOG` que solo existen en 4.0.
1. Quitar las tablas siguientes:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.CATALOG`
1. Iniciar la replicación de HBase

## <a name="deprecation"></a>Desuso

Apache Storm y aprendizaje automático de servicios no están disponibles en HDInsight 4.0.
