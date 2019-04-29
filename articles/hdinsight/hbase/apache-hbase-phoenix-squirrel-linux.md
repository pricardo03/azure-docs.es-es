---
title: Uso de Apache Phoenix y SQLLine con HBase en Azure HDInsight
description: Aprenda a usar Apache Phoenix en HDInsight. Además, conozca cómo instalar y configurar SQLLine en el equipo para conectarse a un clúster de HBase en HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/03/2018
ms.author: hrasheed
ms.openlocfilehash: 0bef586540635ee1bada3475f6316c84dea4308c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123168"
---
# <a name="use-apache-phoenix-with-linux-based-apache-hbase-clusters-in-hdinsight"></a>Uso de Apache Phoenix con clústeres de Apache HBase basados en Linux en HDInsight
Aprenda a usar [Apache Phoenix](https://phoenix.apache.org/) en Azure HDInsight y también a usar SQLLine. Para obtener más información acerca de Phoenix, consulte [Apache Phoenix en 15 minutos o menos](https://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Para conocer la gramática de Apache Phoenix, consulte [el siguiente vínculo](https://phoenix.apache.org/language/index.html).

> [!NOTE]  
> Para obtener más información sobre la versión de Phoenix en HDInsight, consulte las [novedades de las versiones de clúster de Apache Hadoop proporcionadas por HDInsight](../hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Uso de SQLLine
[SQLLine](http://sqlline.sourceforge.net/) es una utilidad de línea de comandos para ejecutar SQL.

### <a name="prerequisites"></a>Requisitos previos
Antes de poder usar SQLLine, debe disponer de los siguientes elementos:

* **Un clúster de Apache HBase en HDInsight**. Para crear uno, consulte [Introducción a Apache HBase en HDInsight](./apache-hbase-tutorial-get-started-linux.md).

Cuando se conecte a un clúster de HBase, la conexión debe establecerse con una de las máquinas virtuales de [Apache ZooKeeper](https://zookeeper.apache.org/). Cada clúster de HDInsight tiene tres máquinas virtuales Zookeeper.

**Para obtener el nombre de host de ZooKeeper**

1. Abra [Apache Ambari](https://ambari.apache.org/); para ello, vaya a **https://\<nombre del clúster\>.azurehdinsight.net**.
2. Para iniciar sesión, escriba el nombre de usuario HTTP (clúster) y la contraseña.
3. En el menú de la izquierda, seleccione **ZooKeeper**. Se muestran tres instancias de **ZooKeeper Server**.
4. Seleccione una de las instancias de **ZooKeeper Server**. En el panel **Resumen**, busque el **nombre de host**. Es parecido a *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Para usar SQLLine**

1. Conéctese al clúster mediante SSH. Para más información, consulte [Uso SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. En SSH, escriba los siguientes comandos para ejecutar SQLLine:

        cd /usr/hdp/current/phoenix-client/bin
        ./sqlline.py <ZOOKEEPER SERVER FQDN>:2181:/hbase-unsecure
3. Para crear una tabla de HBase e insertar algunos datos, ejecute los siguientes comandos:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Para más información, consulte el [manual de SQLLine](http://sqlline.sourceforge.net/#manual) y la [gramática de Apache Phoenix](https://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a usar Apache Phoenix en HDInsight. Para más información, consulte estos artículos:

* [Información general de HBase de HDInsight][hdinsight-hbase-overview].
  Apache HBase es una base de datos NoSQL de código abierto de Apache basada en Apache Hadoop que proporciona acceso aleatorio y un elevado nivel de coherencia para grandes cantidades de datos no estructurados y semiestructurados.
* [Aprovisionamiento de clústeres de Apache HBase en Azure Virtual Network][hdinsight-hbase-provision-vnet].
  Con la integración de las redes virtuales, los clústeres de Apache HBase pueden implementarse en la misma red que las aplicaciones, de forma que estas puedan comunicarse directamente con HBase.
* [Configuración de la replicación de Apache HBase en HDInsight](apache-hbase-replication.md). Aprenda a configurar la replicación de Apache HBase entre dos centros de datos de Azure.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-hbase-overview]:apache-hbase-overview.md


