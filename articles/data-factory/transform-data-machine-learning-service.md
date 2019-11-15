---
title: Ejecución de canalizaciones de Azure Machine Learning en canalizaciones de Azure Data Factory
description: Aprenda a ejecutar canalizaciones de Azure Machine Learning en las canalizaciones de Azure Data Factory.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2019
author: djpmsft
ms.author: daperlov
ms.openlocfilehash: a257c7d3f288f9a2e1998bf0679b7146b089cb50
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73683892"
---
# <a name="execute-azure-machine-learning-pipelines-in-azure-data-factory"></a>Ejecución de canalizaciones de Azure Machine Learning en Azure Data Factory

Ejecute canalizaciones de Azure Machine Learning en las canalizaciones de Azure Data Factory como un paso más. La actividad de ejecución de canalización de Machine Learning posibilita escenarios de predicción por lotes, como la identificación de posibles impagos de préstamos, la determinación de opiniones y el análisis de los patrones de comportamiento de los clientes.

## <a name="syntax"></a>Sintaxis

```json
{
    "name": "Machine Learning Execute Pipeline",
    "type": "AzureMLExecutePipeline",
    "linkedServiceName": {
        "referenceName": "AzureMLService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mlPipelineId": "machine learning pipeline ID",
        "experimentName": "experimentName",
        "mlPipelineParameters": {
            "mlParameterName": "mlParameterValue"
        }
    }
}

```

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | DESCRIPCIÓN | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
Nombre | Nombre de la actividad en la canalización | Cadena | Sí
Tipo | El tipo de actividad es "AzureMLExecutePipeline" | Cadena | Sí
linkedServiceName | Servicio vinculado a Azure Machine Learning | Referencia al servicio vinculado | Sí
mlPipelineId | Identificador de la canalización de Azure Machine Learning publicada | Cadena (o expresión con un valor resultType de cadena) | Sí
experimentName | Nombre de experimento del historial de ejecuciones de la canalización de Machine Learning | Cadena (o expresión con un valor resultType de cadena) | Sin
mlPipelineParameters | Pares clave-valor que se pasan al punto de conexión de la canalización de Azure Machine Learning publicada. Las claves deben coincidir con los nombres de los parámetros de la canalización definidos en la canalización de Machine Learning publicada. | Objeto con pares clave-valor (o expresión con objeto resultType) | Sin
mlParentRunId | El identificador principal de la ejecución de la canalización de Azure Machine Learning | Cadena (o expresión con un valor resultType de cadena) | Sin
continueOnStepFailure | Puede decidir si se continúa la ejecución del resto de pasos de la ejecución de la canalización de Machine Learning si se produce un error en un paso. | boolean | Sin

## <a name="next-steps"></a>Pasos siguientes
Vea los siguientes artículos, en los que se explica cómo transformar datos de otras maneras:

* [Actividad de ejecución de Data Flow](control-flow-execute-data-flow-activity.md)
* [Actividad de U-SQL](transform-data-using-data-lake-analytics.md)
* [Actividad de Hive](transform-data-using-hadoop-hive.md)
* [Actividad de Pig](transform-data-using-hadoop-pig.md)
* [Actividad de MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Actividad de streaming de Hadoop](transform-data-using-hadoop-streaming.md)
* [Actividad de Spark](transform-data-using-spark.md)
* [Actividad personalizada de .NET](transform-data-using-dotnet-custom-activity.md)
* [Actividad de procedimiento almacenado](transform-data-using-stored-procedure.md)
