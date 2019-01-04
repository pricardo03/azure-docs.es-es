---
title: 'MapReduce y conexión de SSH con Apache Hadoop en HDInsight: Azure'
description: Obtenga más información sobre cómo usar SSH para ejecutar trabajos de MapReduce mediante Apache Hadoop en HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: hrasheed
ms.openlocfilehash: bce79aed49b94071b4e83524de2d599fca182256
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438578"
---
# <a name="use-mapreduce-with-apache-hadoop-on-hdinsight-with-ssh"></a>Uso de MapReduce con Apache Hadoop en HDInsight con SSH

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Aprenda a enviar trabajos de MapReduce desde una conexión Secure Shell (SSH) a HDInsight.

> [!NOTE]
> Si ya está familiarizado con el uso de servidores de Apache Hadoop basados en Linux, pero no conoce HDInsight, consulte [Información sobre el uso de HDInsight en Linux](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Requisitos previos

* Un clúster de HDInsight basado en Linux (Hadoop en HDInsight)

  > [!IMPORTANT]
  > Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Un cliente SSH. Para más información, consulte [Uso de SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="ssh"></a>Conexión con SSH

Conéctese al clúster con SSH. Por ejemplo, el siguiente comando establece conexión con un clúster llamado **myhdinsight** utilizando la cuenta **sshuser**:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

**Si usa una clave de certificado para la autenticación SSH**, puede que deba especificar la ubicación de la clave privada en su sistema cliente, por ejemplo:

```bash
ssh -i ~/mykey.key sshuser@myhdinsight-ssh.azurehdinsight.net
```

**Si usa una contraseña para la autenticación SSH**, deberá proporcionar la contraseña cuando se le solicite.

Para obtener más información sobre cómo utilizar SSH con HDInsight, consulte [Uso de SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Uso de comandos Hadoop

1. Después de conectarse al clúster de HDInsight, use el siguiente comando para iniciar un trabajo de MapReduce:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Este comando inicia la clase `wordcount`, que está contenido en el archivo `hadoop-mapreduce-examples.jar`. Emplea como entrada el documento `/example/data/gutenberg/davinci.txt` y la salida se almacena en `/example/data/WordCountOutput`.

    > [!NOTE]
    > Para obtener más información sobre este trabajo de MapReduce y los datos de ejemplo, vea [Uso de MapReduce en Apache Hadoop en HDInsight](hdinsight-use-mapreduce.md).

2. El trabajo emite detalles a medida que se procesa y devuelve información similar al siguiente texto cuando finaliza el trabajo:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Una vez completado el trabajo, use el siguiente comando para enumerar los archivos de salida:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Este comando muestra dos archivos, `_SUCCESS` y `part-r-00000`. El archivo `part-r-00000` contiene la salida de este trabajo.

    > [!NOTE]  
    > Algunos trabajos de MapReduce pueden dividir los resultados entre varios archivos **part-r-####** . Si es así, utilice el sufijo #### para indicar el orden de los archivos.

4. Para ver la salida, use el comando siguiente:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Este comando muestra una lista de las palabras contenidas en el archivo **wasb://example/data/gutenberg/davinci.txt**, junto con el número de veces que aparecía cada palabra. El texto siguiente es un ejemplo de los datos contenidos en el archivo:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Resumen

Como se puede ver, los comando Hadoop proporcionan una manera fácil de ejecutar trabajos de MapReduce en un clúster de HDInsight y, a continuación, ver la salida del trabajo.

## <a id="nextsteps"></a>Pasos siguientes

Para obtener información general sobre los trabajos de MapReduce en HDInsight:

* [Uso de MapReduce en Hadoop de HDInsight](hdinsight-use-mapreduce.md)

Para obtener información sobre otras maneras de trabajar con Hadoop en HDInsight:

* [Uso de Apache Hive con Apache Hadoop en HDInsight](hdinsight-use-hive.md)
* [Uso de Apache Pig con Apache Hadoop en HDInsight](hdinsight-use-pig.md)
