---
title: 'MapReduce y Escritorio remoto con Apache Hadoop en HDInsight: Azure'
description: Obtenga información acerca de cómo usar Escritorio remoto para conectarse a Apache Hadoop en HDInsight y ejecutar trabajos de MapReduce.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: c0040958cbf748d3eafb3ee60806b064e4e0b1ba
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435793"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight-with-remote-desktop"></a>Uso de MapReduce de Apache Hadoop en HDInsight con Escritorio remoto
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

En este artículo, obtendrá información sobre cómo conectarse a un clúster de Apache Hadoop en HDInsight mediante Escritorio remoto y, a continuación, ejecutar trabajos de MapReduce mediante el comando de Hadoop.

> [!IMPORTANT]  
> Escritorio remoto solo está disponible en los clústeres de HDInsight basados en Windows. Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Para HDInsight 3.4 o superior, consulte [Uso de MapReduce con SSH](apache-hadoop-use-mapreduce-ssh.md) para más información sobre cómo conectarse al clúster de HDInsight y ejecutar trabajos de MapReduce.

## <a id="prereq"></a>Requisitos previos
Para completar los pasos de este artículo, necesitará lo siguiente:

* Un clúster de HDInsight basado en Windows (Hadoop en HDInsight)
* Un equipo cliente con Windows 10, Windows 8 o Windows 7

## <a id="connect"></a>Conexión con el Escritorio remoto
Habilite el Escritorio remoto para el clúster de HDInsight y conéctese a él siguiendo las instrucciones dadas en [Conexión a los clústeres de HDInsight con RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Uso del comando Hadoop
Una vez conectado al escritorio para el clúster de HDInsight, siga estos pasos para ejecutar un trabajo de MapReduce mediante el comando de Hadoop:

1. Desde el escritorio de HDInsight, inicie la **línea de comandos de Hadoop**. Se abrirá un nuevo símbolo del sistema en el directorio **c:\apps\dist\hadoop-&lt;número de versión>**.

   > [!NOTE]  
   > El número de versión cambia cuando se actualiza Hadoop. La variable de entorno **HADOOP_HOME** puede usarse para encontrar la ruta de acceso. Por ejemplo, `cd %HADOOP_HOME%` cambiará los directorios al directorio de Hadoop, sin que sea necesario conocer el número de versión.
   >
   >
2. Para usar el comando de **Hadoop** para ejecutar un trabajo de MapReduce de ejemplo, utilice el siguiente comando:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Se inicia la clase **wordcount**, incluida en el archivo **hadoop-mapreduce-examples.jar** del directorio actual. Como entrada, usa el documento **wasb://example/data/gutenberg/davinci.txt** y la salida se almacena en **wasb:///ejemplo/data/WordCountOutput**.

   > [!NOTE]
   > Para obtener más información acerca de este trabajo de MapReduce y los datos de ejemplo, consulte <a href="hdinsight-use-mapreduce.md">Uso de MapReduce en Hadoop de HDInsight</a>.
   >
   >
3. El trabajo emite detalles cuando se procesa y devuelve información similar a la siguiente cuando finaliza el trabajo:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. Una vez completado, use el comando siguiente para enumerar los archivos de salida almacenados en **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Se deberían mostrar dos archivos, **_SUCCESS** y **part-r-00000**. El archivo **part-r-00000** contiene la salida de este trabajo.

   > [!NOTE]
   > Algunos trabajos de MapReduce pueden dividir los resultados entre varios archivos **part-r-####** . Si es así, utilice el sufijo #### para indicar el orden de los archivos.
   >
   >
5. Para ver la salida, use el comando siguiente:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Se mostrará una lista de las palabras contenidas en el archivo **wasb://example/data/gutenberg/davinci.txt**, junto con el número de veces que aparecía cada palabra. El siguiente es un ejemplo de los datos que estarán contenidos en el archivo:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Resumen
Como se puede ver, el comando de Hadoop proporciona una manera fácil de ejecutar trabajos de MapReduce en un clúster de HDInsight y, a continuación, ver la salida del trabajo.

## <a id="nextsteps"></a>Pasos siguientes
Para obtener información general sobre los trabajos de MapReduce en HDInsight:

* [Uso de MapReduce en Hadoop de HDInsight](hdinsight-use-mapreduce.md)

Para obtener información sobre otras maneras de trabajar con Hadoop en HDInsight:

* [Uso de Apache Hive con Apache Hadoop en HDInsight](hdinsight-use-hive.md)
* [Uso de Apache Pig con Apache Hadoop en HDInsight](hdinsight-use-pig.md)
