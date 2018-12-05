---
title: Depuración y análisis de servicios de Apache Hadoop con volcados de memoria en Azure
description: Recopile automáticamente volcados de memoria para servicios de Apache Hadoop y coloque dentro de la cuenta de almacenamiento de blobs de Azure para depuración y análisis.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 1b4ca22faf8ef01cab4b2e7231fea8ed49f0fcb3
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494585"
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-apache-hadoop-services"></a>Recopilar volcados de memoria en el almacenamiento de blobs para depurar y analizar servicios de Apache Hadoop
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Los volcados de montón contienen una instantánea de la memoria de la aplicación, incluidos los valores de variables en el momento en el que se creó el volcado de memoria. Por ello, estos volcados resultan útiles a la hora de diagnosticar cualquier problema que ocurra en tiempo de ejecución. Los volcados de montón pueden recopilarse automáticamente para servicios de [Apache Hadoop](https://hadoop.apache.org/) y se colocan en la cuenta de Azure Blob Storage de un usuario en HDInsightHeapDumps/.

La recopilación de volcados de memoria para los distintos servicios debe habilitarse para los servicios en clústeres individuales. De forma predeterminada, esta característica está desactivada para un clúster. Los volcados de memoria pueden ser de gran tamaño, por lo que se recomienda supervisar la cuenta de almacenamiento de blobs en la que se van a guardar tras habilitar la recopilación.

> [!IMPORTANT]
> Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). La información de este artículo solo se aplica al HDInsight basado en Windows. Para obtener más información sobre HDInsight basado en Linux, consulte [Habilitar los volcados de montón de los servicios de Apache Hadoop en HDInsight basado en Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md).


## <a name="eligible-services-for-heap-dumps"></a>Servicios de volcados de memoria aptos
Puede habilitar los volcados de montón en los siguientes servicios:

* **Apache HCatalog**: tempelton
* **Apache Hive**: hiveserver2, metastore, derbyserver
* **mapreduce** - jobhistoryserver
* **Apache Yarn**: resourcemanager, nodemanager, timelineserver
* **Apache HDFS**: datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Elementos de configuración que habilitan los volcados de memoria
Para activar el volcado de memoria para un servicio, el usuario deberá definir los elementos de configuración adecuados en la sección de dicho servicio, que se especifica mediante **nombre_servicio**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

El valor de **nombre_servicio** puede ser cualquiera de los servicios enumerados aquí: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode o namenode.

## <a name="enable-using-azure-powershell"></a>Habilitar con Azure PowerShell
Por ejemplo, para activar los volcados de memoria mediante Azure PowerShell para jobhistoryserver, puede usar el siguiente script:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Habilitar con SDK para .NET
Por ejemplo, para activar los volcados de memoria mediante el SDK de .NET de Azure HDInsight para jobhistoryserver, puede usar el siguiente código:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
