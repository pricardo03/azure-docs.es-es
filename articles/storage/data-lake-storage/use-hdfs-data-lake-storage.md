---
title: Uso de la CLI de HDFS con Azure Data Lake Storage Gen2 (versión preliminar)
description: Introducción a la CLI de HDFS para Data Lake Storage Gen2 (versión preliminar)
services: storage
documentationcenter: ''
author: artemuwka
manager: jahogg
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: artek
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 591d8ea7670bf9b29450695ee7cbee5fa39baaac
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344729"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Uso de la CLI de HDFS con Data Lake Storage Gen2

La versión preliminar de Azure Data Lake Storage Gen2 le permite administrar y acceder a los datos igual que lo haría con un [sistema de archivos distribuido de Hadoop (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Tanto si usa un clúster de HDInsight como si ejecuta un trabajo de Apache Spark mediante Azure Databricks para realizar el análisis de los datos almacenados en Azure Data Lake Storage Gen2 puede usar la interfaz de la línea de comandos (CLI) para recuperar y manipular los datos cargados. El resto del artículo describe las opciones que tiene mientras que el [equipo de Azure Storage sigue trabajando para agregar compatibilidad con el Explorador de Azure Storage y Azure Portal](https://azure.microsoft.com/roadmap/).

## <a name="hdfs-cli-with-hdinsight"></a>CLI de HDFS con HDInsight

HDInsight brinda acceso al sistema de archivos distribuidos que se adjunta localmente a los nodos de ejecución. Se puede acceder a este sistema de archivos mediante el shell que interactúa directamente con HDFS y con otros sistemas de archivos que admiten Hadoop. A continuación puede ver comandos que se usan habitualmente y vínculos a recursos útiles.

>[!IMPORTANT]
>La facturación del clúster de HDInsight se inicia una vez creado el clúster y solo se detiene cuando se elimina. Se facturan por minuto realizando una prorrata, por lo que siempre debe eliminar aquellos que ya no se estén utilizando (aprenda a [eliminar un clúster](../../hdinsight/hdinsight-delete-cluster.md)). No obstante, los datos almacenados en Azure Data Lake Storage Gen2 continúan incluso después de eliminar un clúster de HDInsight.

Para obtener una lista de archivos o directorios:

    hdfs dfs -ls <args>
Para crear un directorio:

    hdfs dfs -mkdir [-p] <paths>
Para eliminar un archivo o un directorio:

    hdfs dfs -rm [-skipTrash] URI [URI ...]


Vamos a usar un clúster de Hadoop de HDInsight en Linux como ejemplo. Para usar la CLI de HDFS debe establecer antes un [acceso remoto a los servicios](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Si selecciona [SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) el ejemplo de código de PowerShell tendría el siguiente aspecto:
```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```

La cadena de conexión se encuentra en la sección "SSH e inicio de sesión del clúster" de la hoja del clúster de HDInsight en Azure Portal. Las credenciales SSH se especificaron en el momento de la creación del clúster

Para más información sobre la CLI de HDFS CLI, consulte la [documentación oficial](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) y la [Guía de permisos de HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="hdfs-cli-with-azure-databricks"></a>CLI de HDFS con Azure Databricks

Databricks proporciona una CLI fácil de usar que se ha compilado sobre la API REST de Databricks. El proyecto de código abierto se hospeda en [GitHub](https://github.com/databricks/databricks-cli). A continuación se muestran los comandos usados.

Para obtener una lista de archivos o directorios:

    dbfs ls [-l]
Para crear un directorio:

    dbfs mkdirs
Para eliminar un archivo:

    dbfs rm [-r]

Otra manera de interactuar con Databricks son los cuadernos. Aunque un cuaderno tiene un lenguaje principal, puede combinar lenguajes si especifica el comando mágico del lenguaje %language al principio de una celda. En concreto, %sh le permite ejecutar código de shell en el cuaderno de forma parecida a la del ejemplo de HDInsight que se mostró anteriormente en este artículo.

Para obtener una lista de archivos o directorios:

    %sh ls <args>
Para crear un directorio:

    %sh mkdir [-p] <paths>
Para eliminar un archivo o un directorio:

    %sh rm [-skipTrash] URI [URI ...]

Después de iniciar el clúster de Spark en Azure Databricks, podrá crear un nuevo cuaderno. El script del cuaderno de ejemplo tiene el siguiente aspecto:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder

Para más información sobre la CLI de Databricks, consulte la [documentación oficial](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Para más información sobre cuadernos, consulte la sección de [cuadernos](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) de la documentación.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de un clúster de HDInsight con Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
- [Uso de una cuenta compatible de Azure Data Lake Storage Gen2 en Azure Databricks](./quickstart-create-databricks-account.md) 