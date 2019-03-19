---
title: Transformación de datos mediante Hadoop Streaming Activity en Azure Data Factory | Microsoft Docs
description: Se explica cómo usar Hadoop Streaming Activity en Azure Data Factory para transformar datos mediante la ejecución de programas de Hadoop Streaming en un clúster de Hadoop.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 0d8267f1cd65f78d5e98ae9d288d5fa5c4214420
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57576436"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Transformación de datos mediante Hadoop Streaming Activity en Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-hadoop-streaming-activity.md)
> * [Versión actual](transform-data-using-hadoop-streaming.md)

La actividad de HDInsight Streaming en una [canalización](concepts-pipelines-activities.md) de Data Factory ejecuta programas de Hadoop Streaming en [su propio](compute-linked-services.md#azure-hdinsight-linked-service) clúster de HDInsight o en un clúster [a petición](compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Este artículo se basa en el artículo sobre [actividades de transformación de datos](transform-data.md) , que presenta información general de la transformación de datos y las actividades de transformación admitidas.

Si no está familiarizado con Azure Data Factory, vea [Introduction to Azure Data Factory](introduction.md) (Introducción a Azure Data Factory) y siga el [tutorial de transformación de datos](tutorial-transform-data-spark-powershell.md) antes de leer este artículo. 

## <a name="json-sample"></a>Ejemplo de JSON
```json
{
    "name": "Streaming Activity",
    "description": "Description",
    "type": "HDInsightStreaming",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mapper": "MyMapper.exe",
        "reducer": "MyReducer.exe",
        "combiner": "MyCombiner.exe",
        "fileLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "filePaths": [
            "<containername>/example/apps/MyMapper.exe",
            "<containername>/example/apps/MyReducer.exe",
            "<containername>/example/apps/MyCombiner.exe"
        ],
        "input": "wasb://<containername>@<accountname>.blob.core.windows.net/example/input/MapperInput.txt",
        "output": "wasb://<containername>@<accountname>.blob.core.windows.net/example/output/ReducerOutput.txt",
        "commandEnvironment": [
            "CmdEnvVarName=CmdEnvVarValue"
        ],
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```

## <a name="syntax-details"></a>Detalles de la sintaxis

| Propiedad          | DESCRIPCIÓN                              | Obligatorio |
| ----------------- | ---------------------------------------- | -------- |
| Nombre              | Nombre de la actividad                     | Sí      |
| description       | Texto que describe para qué se usa la actividad. | Sin        |
| Tipo              | En Hadoop Streaming Activity, el tipo de actividad es HDInsightStreaming | Sí      |
| linkedServiceName | Referencia al clúster de HDInsight registrado como servicio vinculado en Data Factory. Para obtener más información sobre este servicio vinculado, vea el artículo [Compute linked services](compute-linked-services.md) (Servicios vinculados de procesos). | Sí      |
| mapper            | Especifica el nombre del archivo ejecutable del asignador | Sí      |
| reducer           | Especifica el nombre del archivo ejecutable del reductor | Sí      |
| combiner          | Especifica el nombre del archivo ejecutable del combinador | Sin        |
| fileLinkedService | Referencia a un servicio vinculado de Azure Storage que se usa para almacenar los programas Asignador, Combinador y Reductor que se van a ejecutar. Si no se especifica este servicio vinculado, se usará el servicio vinculado de Azure Storage definido en el servicio vinculado de HDInsight. | Sin        |
| filePath          | Proporcione una matriz de ruta de acceso a los programas Asignador, Combinador y Reductor almacenados en el almacenamiento de Azure Storage al que fileLinkedService hace referencia. La ruta de acceso distingue mayúsculas de minúsculas. | Sí      |
| input             | Especifica la ruta de acceso de WASB al archivo de entrada para el asignador. | Sí      |
| output            | Especifica la ruta de acceso de WASB al archivo de salida para el reductor. | Sí      |
| getDebugInfo      | Especifica si se copian los archivos de registro en el almacenamiento de Azure Storage que usa el clúster de HDInsight o que está especificado por scriptLinkedService. Valores permitidos: Ninguno, Siempre o Error. Valor predeterminado: Ninguno. | Sin        |
| argumentos         | Especifica una matriz de argumentos para un trabajo de Hadoop. Los argumentos se pasan a cada tarea como argumentos de la línea de comandos. | Sin        |
| define los campos           | Especifique parámetros como pares clave-valor para hacer referencia en el script de Hive. | Sin        | 

## <a name="next-steps"></a>Pasos siguientes
Vea los siguientes artículos, en los que se explica cómo transformar datos de otras maneras: 

* [Actividad de U-SQL](transform-data-using-data-lake-analytics.md)
* [Actividad de Hive](transform-data-using-hadoop-hive.md)
* [Actividad de Pig](transform-data-using-hadoop-pig.md)
* [Actividad de MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Actividad de Spark](transform-data-using-spark.md)
* [Actividad personalizada de .NET](transform-data-using-dotnet-custom-activity.md)
* [Actividad de ejecución de Batch de Machine Learning](transform-data-using-machine-learning.md)
* [Actividad de procedimiento almacenado](transform-data-using-stored-procedure.md)
