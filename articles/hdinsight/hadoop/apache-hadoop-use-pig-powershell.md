---
title: 'Uso de Apache Pig con PowerShell en HDInsight: Azure'
description: Obtenga información sobre cómo enviar trabajos de Apache Pig a un clúster de Apache Hadoop en HDInsight con Azure PowerShell.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 9ad788989273f28f38ee95f8d669fdf17f1fd785
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105035"
---
# <a name="use-azure-powershell-to-run-apache-pig-jobs-with-hdinsight"></a>Uso de Azure PowerShell para ejecutar trabajos de Apache Pig con HDInsight

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Este documento proporciona un ejemplo del uso de Azure PowerShell para enviar trabajos de Apache Pig a un Hadoop de Apache en clúster de HDInsight. Pig permite escribir trabajos de MapReduce mediante un lenguaje (Pig Latin) que modela las transformaciones de datos, en lugar de asignar y reducir las funciones.

> [!NOTE]  
> Este documento no ofrece una descripción detallada de cómo funcionan las instrucciones de Pig Latin que se usan en los ejemplos. Para obtener información sobre Pig Latin utilizado en este ejemplo, consulte [Uso de Apache Pig con Apache Hadoop en HDInsight](hdinsight-use-pig.md).

## <a id="prereq"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* **Un clúster de Azure HDInsight**.

  > [!IMPORTANT]  
  > Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Una estación de trabajo con Azure PowerShell**.

## <a id="powershell"></a>Ejecución de un trabajo de Apache Pig

Azure PowerShell proporciona *cmdlets* que le permiten ejecutar de manera remota trabajos de Pig en HDInsight. Internamente, PowerShell realiza llamadas de REST a la instancia de [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) que se ejecuta en el clúster de HDInsight.

Los cmdlets siguientes se utilizan cuando se ejecutan trabajos de Pig en un clúster de HDInsight remoto:

* **Connect-AzAccount**: autentica a Azure PowerShell en la suscripción de Azure.
* **New-AzHDInsightPigJobDefinition**: crea una *definición de trabajo* con las instrucciones de HiveQL especificadas.
* **Start-AzHDInsightJob**: envía la definición del trabajo a HDInsight e inicia el trabajo. Se devuelve un objeto *job*.
* **Wait-AzHDInsightJob**: usa el objeto del trabajo para comprobar el estado del trabajo. Esperará hasta que el trabajo se haya completado o haya superado el tiempo de espera.
* **Get-AzHDInsightJobOutput**: se utiliza para recuperar la salida del trabajo.

Los pasos siguientes muestran cómo usar estos cmdlets para ejecutar un trabajo en el clúster de HDInsight.

1. Mediante un editor, guarde el código siguiente como **pigjob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Abra un nuevo símbolo del sistema de Windows PowerShell. Cambie los directorios a la ubicación del archivo **pigjob.ps1** y, a continuación, utilice el siguiente comando para ejecutar el script:

        .\pigjob.ps1

    Se le indica que inicie sesión en su suscripción de Azure. Luego, se le pedirá que proporcione el nombre de cuenta y la contraseña de HTTPS/Admin para el clúster de HDInsight.

2. Cuando el trabajo se complete, debe devolver información de manera similar al siguiente texto:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>Solución de problemas

Si una vez completado el trabajo no se devuelve información, consulte los registros de errores. Para ver la información de error para este trabajo, agregue lo siguiente al final del archivo **pigjob.ps1** , guárdelo y, a continuación, ejecútelo de nuevo.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Este cmdlet devuelve la información que se escribió en STDERR al procesar el trabajo.

## <a id="summary"></a>Resumen
Como puede ver, Azure PowerShell proporciona una manera fácil de ejecutar trabajos de Pig en un clúster de HDInsight, de supervisar el estado del trabajo y de recuperar el resultado.

## <a id="nextsteps"></a>Pasos siguientes
Para obtener información general sobre Pig en HDInsight, siga estos pasos:

* [Uso de Apache Pig con Apache Hadoop en HDInsight](hdinsight-use-pig.md)

Para obtener información sobre otras maneras de trabajar con Hadoop en HDInsight:

* [Uso de Apache Hive con Apache Hadoop en HDInsight](hdinsight-use-hive.md)
* [Uso de MapReduce con Apache Hadoop en HDInsight](hdinsight-use-mapreduce.md)
