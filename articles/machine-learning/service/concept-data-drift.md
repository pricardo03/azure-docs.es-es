---
title: Supervisión del desfase de datos (versión preliminar)
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo puede Azure Machine Learning Service supervisar el desfase de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cody-dkdc
ms.author: copeters
ms.date: 06/20/2019
ms.openlocfilehash: a03e3124647869e7148f271810bb523986a851c6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442380"
---
# <a name="what-is-data-drift-monitoring-preview"></a>¿Qué es la supervisión del desfase de datos (versión preliminar)?

El desfase de datos es el cambio en la distribución de los datos. En el contexto del aprendizaje automático, los modelos de aprendizaje automático entrenados pueden experimentar un rendimiento de predicción degradado por el desfase. La supervisión del desfase entre los datos de aprendizaje y los datos usados para realizar predicciones pueden ayudar a detectar problemas de rendimiento.

Los modelos de Machine Learning son igual de buenos que los datos usados para entrenarlos. La implementación de modelos en producción sin supervisar su rendimiento puede dar lugar a efectos no detectados y perjudiciales. Con la supervisión del desfase de datos, puede detectar y adaptarse al desfase de datos. 

## <a name="when-to-monitor-for-data-drift"></a>¿Cuándo se debe supervisar el desfase de datos?

Entre las métricas que podemos supervisar se incluyen:

+ Magnitud del desfase (coeficiente de desfase)
+ Causa del desfase (contribución del desfase por característica)
+ Métricas de distancia (Wasserstein, Distancia energética, etc.)

Con esta supervisión en contexto, se pueden configurar alertas o acciones al detectarse el desfase y el científico de datos puede investigar la causa principal del problema. 

Si piensa que pueden cambiar los datos de entrada para su modelo implementado, debe considerar la posibilidad de usar la detección del desfase de datos.

## <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Cómo se supervisa el desfase de datos en Azure Machine Learning Service

Al usar **Azure Machine Learning Service**, el desfase de datos se supervisa a través de conjuntos de datos o implementaciones. Para supervisar el desfase de datos, se especifica un conjunto de datos de base de referencia, normalmente el conjunto de datos de entrenamiento para un modelo. Un segundo conjunto de datos (normalmente los datos de entrada de modelo recopilados de una implementación) se prueba en el conjunto de datos de base de referencia. Se han [generado perfiles](how-to-explore-prepare-data.md#explore-with-summary-statistics) para ambos conjuntos de datos y se han especificado en el servicio de supervisión del desfase de datos. Un modelo de Machine Learning se entrena para detectar diferencias entre los dos conjuntos de datos. El rendimiento del modelo se convierte en el coeficiente de desfase, que mide la magnitud del desfase entre los dos conjuntos de datos. Al usar la [interoperabilidad del modelo](machine-learning-interpretability-explainability.md), se calculan las características que han contribuido al coeficiente de desfase. En el perfil del conjunto de datos, se realiza un seguimiento de la información estadística sobre cada característica. 

## <a name="data-drift-metric-output"></a>Salida de métricas de desfase de datos

Existen varias formas de ver las métricas de desfase:

* Usar el widget de Jupyter.
* Usar la función `get_metrics()` en cualquier objeto `datadriftRun`.
* Ver las métricas en Azure Portal en su modelo.

Las métricas siguientes se guardan en cada iteración de ejecución de una tarea de desfase de datos:

|Métrica|DESCRIPCIÓN|
--|--|
wasserstein_distance|Distancia estadística definida para la distribución numérica unidimensional.|
energy_distance|Distancia estadística definida para la distribución numérica unidimensional.|
datadrift_coefficient|Coeficiente de correlación de Matthews, un número real entre -1 y 1. En el contexto de desfase, 0 indica que no hay desfase y 1 indica desfase máximo.|
datadrift_contribution|Importancia de las características que contribuyen al desfase.|

## <a name="next-steps"></a>Pasos siguientes

Vea ejemplos y obtenga información sobre cómo supervisar el desfase de datos:

+ [Learn how to monitor data drift on models deployed through Azure Kubernetes Service (AKS)](how-to-monitor-data-drift.md) (Obtenga información sobre cómo supervisar el desfase de datos en modelos implementados a través de Azure Kubernetes Service [AKS])
+ Pruebe [ejemplos de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/data-drift/)