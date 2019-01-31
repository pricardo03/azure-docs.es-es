---
title: Uso de la CLI de HDFS con Azure Data Lake Storage Gen2 (versión preliminar)
description: Introducción a la CLI de HDFS para Data Lake Storage Gen2 (versión preliminar)
services: storage
author: artemuwka
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: artek
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 2691346433b3cc29b22446152ea797cefb02a380
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241277"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Uso de la CLI de HDFS con Data Lake Storage Gen2

La versión preliminar de Azure Data Lake Storage Gen2 le permite administrar y acceder a los datos igual que lo haría con un [sistema de archivos distribuido de Hadoop (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Tanto si usa un clúster de HDInsight conectado como si ejecuta un trabajo de Apache Spark mediante Azure Databricks para realizar el análisis de los datos almacenados en una cuenta de Azure Storage, puede usar la interfaz de la línea de comandos (CLI) para recuperar y manipular los datos cargados.

## <a name="hdfs-cli-with-hdinsight"></a>CLI de HDFS con HDInsight

HDInsight brinda acceso al sistema de archivos distribuidos que se adjunta localmente a los nodos de ejecución. Se puede acceder a este sistema de archivos mediante el shell que interactúa directamente con HDFS y con otros sistemas de archivos que admiten Hadoop. A continuación puede ver comandos que se usan habitualmente y vínculos a recursos útiles.

>[!IMPORTANT]
>La facturación del clúster de HDInsight se inicia una vez creado el clúster y se detiene cuando se elimina. Se facturan por minuto realizando una prorrata, por lo que siempre debe eliminar aquellos que ya no se estén utilizando. Para obtener información sobre cómo eliminar un clúster, consulte nuestro [artículo sobre el tema](../../hdinsight/hdinsight-delete-cluster.md). Sin embargo, los datos almacenados en una cuenta de almacenamiento que tiene Data Lake Storage Gen2 habilitado continúan incluso después de eliminar un clúster de HDInsight.

### <a name="get-a-list-of-files-or-directories"></a>Obtener una lista de archivos o directorios

    hdfs dfs -ls <args>

### <a name="create-a-directory"></a>Creación de directorios

    hdfs dfs -mkdir [-p] <paths>

### <a name="delete-a-file-or-a-directory"></a>Eliminar un archivo o un directorio

    hdfs dfs -rm [-skipTrash] URI [URI ...]

### <a name="use-the-hdfs-cli-with-an-hdinsight-hadoop-cluster-on-linux"></a>Usar la CLI de HDFS con un clúster de HDInsight Hadoop en Linux

En primer lugar, establezca el [acceso remoto a los servicios](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Si selecciona [SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) el ejemplo de código de PowerShell tendría el siguiente aspecto:

```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```
La cadena de conexión se encuentra en la sección "SSH e inicio de sesión del clúster" de la hoja del clúster de HDInsight en Azure Portal. Las credenciales SSH se especificaron en el momento de la creación del clúster

Para más información sobre la CLI de HDFS CLI, consulte la [documentación oficial](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) y la [Guía de permisos de HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html). Para obtener más información sobre las listas de control de acceso en Databricks, consulte la [CLI de Secrets](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#secrets-cli). 

## <a name="hdfs-cli-with-azure-databricks"></a>CLI de HDFS con Azure Databricks

Databricks proporciona una CLI fácil de usar que se ha compilado sobre la API REST de Databricks. El proyecto de código abierto se hospeda en [GitHub](https://github.com/databricks/databricks-cli). A continuación se muestran los comandos usados.

### <a name="get-a-list-of-files-or-directories"></a>Obtener una lista de archivos o directorios

    dbfs ls [-l]

### <a name="create-a-directory"></a>Creación de directorios

    dbfs mkdirs

### <a name="delete-a-file"></a>Eliminación de un archivo

    dbfs rm [-r]

Otra manera de interactuar con Databricks son los cuadernos. Aunque un cuaderno tiene un lenguaje principal, puede combinar lenguajes si especifica el comando mágico del lenguaje %language al principio de una celda. En concreto, %sh le permite ejecutar código de shell en el cuaderno de forma parecida a la del ejemplo de HDInsight que se mostró anteriormente en este artículo.

### <a name="get-a-list-of-files-or-directories"></a>Obtener una lista de archivos o directorios

    %sh ls <args>

### <a name="create-a-directory"></a>Creación de directorios

    %sh mkdir [-p] <paths>

### <a name="delete-a-file-or-a-directory"></a>Eliminar un archivo o un directorio

    %sh rm [-skipTrash] URI [URI ...]

Después de iniciar el clúster de Spark en Azure Databricks, podrá crear un nuevo cuaderno. El script del cuaderno de ejemplo tiene el siguiente aspecto:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder
    #Get the ACL of the newly created directory.
    hdfs dfs -getfacl /samplefolder

Para más información sobre la CLI de Databricks, consulte la [documentación oficial](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Para más información sobre cuadernos, consulte la sección de [cuadernos](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) de la documentación.

## <a name="set-file-and-directory-level-permissions"></a>Establecer permisos en el nivel de directorio y archivo

Establezca y obtenga permisos de directorio a nivel de archivo y directorio. Le presentamos algunos comandos para ayudarle a comenzar. 

Para obtener más información sobre los permisos a nivel de archivo y directorio para el sistema de archivos de Azure Data Lake Gen2, vea [Control de acceso en Azure Data Lake Storage Gen2](storage-data-lake-storage-access-control.md).

### <a name="display-the-access-control-lists-acls-of-files-and-directories"></a>Mostrar las listas de control de acceso (ACL) de archivos y directorios

    hdfs dfs -getfacl [-R] <path>

Ejemplo:

`hdfs dfs -getfacl -R /dir`

Consulte [getfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#getfacl).

### <a name="set-acls-of-files-and-directories"></a>Definir listas de control de acceso de archivos y directorios

    hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]

Ejemplo:

`hdfs dfs -setfacl -m user:hadoop:rw- /file`

Consulte [setfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#setfacl).

### <a name="change-the-owner-of-files"></a>Cambiar el propietario de los archivos

    hdfs dfs -chown [-R] <new_owner>:<users_group> <URI>

Consulte [chown](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chown).

### <a name="change-group-association-of-files"></a>Cambiar la asociación del grupo de archivos

    hdfs dfs -chgrp [-R] <group> <URI>

Consulte [chgrp](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chgrp).

### <a name="change-the-permissions-of-files"></a>Cambiar los permisos de archivos

    hdfs dfs -chmod [-R] <mode> <URI>

Consulte [chmod](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chmod).

Puede consultar una lista completa de comandos en el sitio web de la [guía de comandos shell del sistema de archivos de Apache Hadoop 2.4.1](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html).

## <a name="next-steps"></a>Pasos siguientes

[Uso de una cuenta compatible de Azure Data Lake Storage Gen2 en Azure Databricks](./data-lake-storage-quickstart-create-databricks-account.md) 
