---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 1c8bdd0ca715b6dfd00dc9876a074c61711990af
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67117819"
---
# <a name="release-notes-for-azure-hdinsight"></a>Notas de la versión de Azure HDInsight.

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

> [!IMPORTANT]  
> Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Para obtener más información, consulte el [artículo de control de versiones de HDInsight](hdinsight-component-versioning.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto de Apache Hadoop y Apache Spark en Azure.

## <a name="new-features"></a>Nuevas características

Para más información sobre cambios importantes en HDInsight 4.0., vea [Novedades de HDInsight 4.0](../hdinsight/hdinsight-version-release.md).

## <a name="component-versions"></a>Versiones de componentes

Las versiones oficiales de Apache de todos los componentes de HDInsight 4.0 se indican a continuación. Todos los componentes enumerados aquí son lanzamientos de las versiones estables más recientes disponibles.

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
- Apache Spark 2.3.2
- Apache Sqoop 1.4.7
- Apache TEZ 0.9.1
- Apache Zeppelin 0.8.0
- Apache ZooKeeper 3.4.6

A veces, se agrupan versiones posteriores de componentes de Apache en la distribución de HDP, además de las versiones indicadas anteriormente. En este caso, estas versiones posteriores se muestran en la tabla de versiones Technical Preview y no deben sustituir a las versiones de componentes de Apache de la lista anterior en un entorno de producción.

## <a name="apache-patch-information"></a>Información sobre la revisión de Apache

Para más información sobre las revisiones disponibles en HDInsight 4.0, consulte la lista de revisiones de cada producto en la tabla siguiente.

| Nombre de producto | Información sobre la revisión |
|---|---|
| Ambari | [Información sobre la revisión de Ambari](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/bk_ambari-release-notes/content/ambari_relnotes-2.7.1.0-patch-information.html) |
| Hadoop | [Información sobre la revisión de Hadoop](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hadoop.html) |
| HBase | [Información sobre la revisión de HBase](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hbase.html) |
| Hive  | En esta versión se proporciona Hive 3.1.0 sin ninguna revisión de Apache adicional.  |
| Kafka | En esta versión se proporciona Kafka 1.1.1 sin ninguna revisión de Apache adicional. |
| Oozie | [Información sobre la revisión de Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_oozie.html) |
| Phoenix | [Información sobre la revisión de Phoenix](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_phoenix.html) |
| Pig | [Información sobre la revisión de Pig](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_pig.html) |
| Ranger | [Información sobre la revisión de Ranger](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_ranger.html) |
| Spark | [Información sobre la revisión de Spark](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_spark.html) |
| Sqoop | En esta versión se proporciona Sqoop 1.4.7 sin ninguna revisión de Apache adicional. |
| Tez | En esta versión se proporciona Tez 0.9.1 sin ninguna revisión de Apache adicional. |
| Zeppelin | En esta versión se proporciona Zeppelin 0.8.0 sin ninguna revisión de Apache adicional. |
| Zookeeper | [Información sobre la revisión de Zookeeper](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_zookeeper.html) |

## <a name="fixed-common-vulnerabilities-and-exposures"></a>Se han corregido las vulnerabilidades y exposiciones comunes

Para más información sobre los problemas de seguridad resueltos en esta versión, vea el documento de Hortonworks sobre las [vulnerabilidades y exposiciones comunes resueltas en HDP 3.0.1](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/cve.html).

## <a name="known-issues"></a>Problemas conocidos

### <a name="replication-is-broken-for-secure-hbase-with-default-installation"></a>Replicación incompleta de HBase seguro con la instalación predeterminada

En HDInsight 4.0, realice los pasos siguientes:

1. Habilite la comunicación entre clústeres.
1. Inicie sesión en el nodo principal activo.
1. Descargue un script para habilitar la replicación con el siguiente comando:

    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Escriba el comando `sudo kinit <domainuser>`.
1. Escriba el siguiente comando para ejecutar el script:

    ```
    sudo bash hdi_enable_replication.sh -m <hn0> -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```
En HDInsight 3.6, realice los pasos siguientes:

1. Inicie sesión en HMaster ZK activo.
1. Descargue un script para habilitar la replicación con el siguiente comando:
    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Escriba el comando `sudo kinit -k -t /etc/security/keytabs/hbase.service.keytab hbase/<FQDN>@<DOMAIN>`.
1. Escriba el siguiente comando:

    ```bash
    sudo bash hdi_enable_replication.sh -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```

### <a name="phoenix-sqlline-stops-working-after-migrating-hbase-cluster-to-hdinsight-40"></a>Phoenix Sqlline deja de funcionar después de migrar el clúster de HBase a HDInsight 4.0

Siga estos pasos:

1. Elimine las siguientes tablas de Phoenix:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.MUTEX`
    1. `SYSTEM.CATALOG`
1. Si alguna de las tablas no se puede eliminar, reinicie HBase para borrar las conexiones a las tablas.
1. Vuelva a ejecutar `sqlline.py`. Phoenix volverá a crear todas las tablas que se eliminaron en el paso 1.
1. Vuelva a generar las tablas y vistas de Phoenix de los datos de HBase.

### <a name="phoenix-sqlline-stops-working-after-replicating-hbase-phoenix-metadata-from-hdinsight-36-to-40"></a>Phoenix Sqlline deja de funcionar después de la replicación de metadatos de HBase Phoenix de HDInsight 3.6 a 4.0

Siga estos pasos:

1. Antes de realizar la replicación, vaya al clúster de 4.0 de destino y ejecute `sqlline.py`. Este comando generará tablas de Phoenix como `SYSTEM.MUTEX` y `SYSTEM.LOG`, que solo existen en 4.0.
1. Elimine las tablas siguientes:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.CATALOG`
1. Inicie la replicación de HBase.

## <a name="deprecation"></a>Desuso

Apache Storm y los servicios de ML no están disponibles en HDInsight 4.0.
