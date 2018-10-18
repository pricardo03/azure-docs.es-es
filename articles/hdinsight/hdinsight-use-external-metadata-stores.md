---
title: Uso de almacenes de metadatos externos en Azure HDInsight
description: Use repositorios de metadatos externos con clústeres de HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: 7c58162048de341468b69a29c55edf346b376e9b
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733821"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Uso de repositorios de metadatos externos en Azure HDInsight

Hive Metastore en HDInsight es una parte fundamental de la arquitectura de Hadoop. Una tienda de metadatos es el repositorio del esquema central que otras herramientas de acceso de macrodatos pueden usar, como Spark, Interactive Query (LLAP), Presto o Pig. HDInsight utiliza una base de datos de Azure SQL Database como Hive Metastore.

![Arquitectura del repositorio de metadatos de Hive en HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Hay dos formas de configurar una tienda de metadatos para los clústeres de HDInsight:

* [Tienda de metadatos predeterminada](#default-metastore)
* [Tienda de metadatos personalizada](#custom-metastore)

## <a name="default-metastore"></a>Tienda de metadatos predeterminada

De forma predeterminada, HDInsight crea una tienda de metadatos con cada tipo de clúster. Pero en vez de esto, puede especificar una tienda de metadatos personalizada. La tienda de metadatos predeterminada incluye las siguientes consideraciones:
- No implica costes adicionales. HDInsight crea una tienda de metadatos con cada tipo de clúster sin ningún coste adicional para el usuario.
- Cada tienda de metadatos predeterminada forma parte del ciclo de vida del clúster. Al eliminar un clúster, también se eliminan la tienda de metadatos y los metadatos correspondientes.
- No puede compartir la tienda de metadatos predeterminada con otros clústeres.
- La tienda de metadatos predeterminada utiliza una base de datos Azure SQL DB básica, que tiene un límite de cinco DTU (unidad de transacción de base de datos).
Esta tienda de metadatos predeterminada se utiliza normalmente para las cargas de trabajo relativamente sencillas que no requieren varios clústeres y no necesitan que los metadatos se conserven más allá del ciclo de vida del clúster.


## <a name="custom-metastore"></a>Tienda de metadatos personalizada

HDInsight también admite tiendas de metadatos personalizadas, que se recomiendan para clústeres de producción:
- Como tienda de metadatos, se especifica su propia base de datos de Azure SQL Database.
- El ciclo de vida de la tienda de metadatos no está asociado a un ciclo de vida de los clústeres, por lo que puede crear y eliminar clústeres sin pérdida de metadatos. Los metadatos, como los esquemas de Hive, se conservarán incluso después de eliminar y volver a crear el clúster de HDInsight.
- Una tienda de metadatos personalizada le permite adjuntar varios clústeres y tipos de clústeres a esa tienda de metadatos. Por ejemplo, una tienda de metadatos sencilla puede compartirse a través de clústeres de Interactive Query, Hive y Spark en HDInsight.
- Se paga por el costo de una tienda de metadatos (Azure SQL DB) de acuerdo con el nivel de rendimiento que seleccione.
- Es posible escalar la tienda de metadatos según sea necesario.

![Caso de uso de repositorio de metadatos de Hive en HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)


### <a name="select-a-custom-metastore-during-cluster-creation"></a>Selección de una tienda de metadatos personalizada durante la creación del clúster

Puede apuntar el clúster a una instancia de Azure SQL Database generada anteriormente durante la creación del clúster, o puede configurar la instancia de SQL Database una vez creado el clúster. Esta opción se especifica en Almacenamiento > Tienda de metadatos al crear un nuevo clúster de Hadoop, Spark o Hive interactivo desde Azure Portal.

![Repositorio de metadatos de Hive en HDInsight: Azure Portal](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

También puede agregar más clústeres a una tienda de metadatos personalizada desde Azure Portal o desde la configuración de Ambari (Hive > Advanced [Opciones avanzadas]).

![Repositorio de metadatos de Hive en HDInsight: Ambari](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Procedimientos recomendados de Hive Metastore

Estos son algunos procedimientos recomendados para Hive Metastore en HDInsight:

- Use una tienda de metadatos personalizada siempre que sea posible para ayudarle a separar los recursos de proceso (el clúster de ejecución) y los metadatos (almacenados en la tienda de metadatos).
- Comience con un nivel S2, que proporciona 50 DTU y 250 GB de almacenamiento. Si percibe un cuello de botella, puede escalar la base de datos.
- Si tiene previsto que varios clústeres de HDInsight tengan acceso a datos separados, use una base de datos independiente para la tienda de metadatos en cada clúster. Si comparte una tienda de metadatos entre varios clústeres de HDInsight, entonces los clústeres utilizan los mismos metadatos y los mismos archivos de datos de usuario subyacentes.
- Realice una copia de la tienda de metadatos personalizada periódicamente. Azure SQL Database genera las copias de seguridad automáticamente, pero el período de tiempo de retención de dichas copias de seguridad varía. Para más información, consulte [más información sobre copias de seguridad automáticas de SQL Database](../sql-database/sql-database-automated-backups.md).
- Coloque la tienda de metadatos y el clúster de HDInsight en la misma región para obtener el mayor rendimiento y los cargos de salida de red más bajos.
- Supervise el rendimiento y la disponibilidad de su tienda de metadatos mediante herramientas de supervisión de Azure SQL Database, como Azure Portal o Azure Log Analytics.
- Cuando se crea una nueva versión de Azure HDInsight en una base de datos de tienda de metadatos personalizada ya existente, el sistema actualiza el esquema de la tienda de metadatos; esta acción es irreversible sin tener que restaurar la base de datos de la copia de seguridad.
- Si comparte una misma tienda de metadatos entre varios clústeres, asegúrese de que todos ellos cuentan con la misma versión de HDInsight. Diferentes versiones de Hive utilizan diferentes esquemas de base de datos de tienda de metadatos. Por ejemplo, no se puede compartir una tienda de metadatos con los clústeres de las versiones Hive 1.2 y 2.1. 

## <a name="oozie-metastore"></a>Oozie Metastore

Apache Oozie es un sistema de coordinación de flujos de trabajo que administra trabajos de Hadoop.  Oozie es compatible con los trabajos de Hadoop para Apache MapReduce, Pig, Hive y otros.  Oozie utiliza una tienda de metadatos para almacenar los detalles acerca de los flujos de trabajo actuales y completados. Para aumentar el rendimiento al usar Oozie, puede utilizar una instancia de Azure SQL Database como tienda de metadatos personalizada. La tienda de metadatos también puede proporcionar acceso a datos de trabajo de Oozie después de eliminar el clúster.

Para obtener instrucciones sobre cómo crear una tienda de metadatos de Oozie con Azure SQL Database, consulte este artículo sobre el [uso de Oozie para flujos de trabajo](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de clústeres en HDInsight con Hadoop, Spark, Kafka, etc.](./hdinsight-hadoop-provision-linux-clusters.md)
