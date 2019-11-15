---
title: Aprendizaje automático e inteligencia artificial con ONNX en la versión preliminar de Azure SQL Database Edge | Microsoft Docs
description: El aprendizaje automático en la versión preliminar de Azure SQL Database Edge admite modelos del formato Open Neural Network Exchange (ONNX). ONNX es un formato abierto que puede usar para intercambiar modelos entre varios marcos y herramientas de aprendizaje automático.
keywords: Implementación de SQL Database Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.subservice: machine-learning
ms.topic: conceptual
author: ronychatterjee
ms.author: achatter
ms.reviewer: davidph
ms.date: 11/07/2019
ms.openlocfilehash: 976c849f9cb48e1c197f70d10e911216a6a7425c
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822845"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-database-edge-preview"></a>Aprendizaje automático e inteligencia artificial con ONNX en la versión preliminar de SQL Database Edge

El aprendizaje automático en la versión preliminar de Azure SQL Database Edge admite modelos del formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/). ONNX es un formato abierto que puede usar para intercambiar modelos entre varios [marcos y herramientas de aprendizaje automático](https://onnx.ai/supported-tools).

## <a name="overview"></a>Información general

Para inferir modelos de Machine Learning en Azure SQL Database Edge, primero es preciso obtener un modelo. Puede tratarse de un modelo previamente entrenado, o bien de un modelo personalizado entrenado con la plataforma que prefiera. Azure SQL Database Edge admite el formato ONNX, por lo que tendrá que convertir el modelo a este formato. Esto no debería afectar a la precisión del modelo y, una vez que tenga el modelo ONNX, puede implementar el modelo en Azure SQL Database perimetral y usar la [puntuación nativa con la función PREDICT de T-SQL de predicción](/sql/advanced-analytics/sql-native-scoring/).

## <a name="get-onnx-models"></a>Obtención de modelos de ONNX

Hay varias maneras de obtener un modelo en el formato ONNX:

- [ONNX Model Zoo](https://github.com/onnx/models): Contiene muchos modelos ONNX previamente entrenados para distintos tipos de tareas que se pueden descargar y están listos para su uso.

- [Exportación nativa desde marcos de entrenamiento de Machine Learning](https://onnx.ai/supported-tools): Varios marcos de entrenamiento admiten la funcionalidad de exportación nativa a ONNX, lo que permite guardar el modelo entrenado en una versión específica del formato ONNX, entre las que se incluyen [PyTorch](https://pytorch.org/docs/stable/onnx.html), Chainer y Caffe2. Además, los servicios de creación de modelos como la [característica de aprendizaje automático automatizado de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) y [Azure Custom Vision Service](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) proporcionan la exportación de ONNX.

- [Convertir los modelos existentes](https://github.com/onnx/tutorials#converting-to-onnx-format): En el caso de los marcos que no admiten la exportación nativa, hay paquetes independientes para convertir los modelos al formato ONNX. Para ver ejemplos y tutoriales, consulte [Conversión al formato ONNX](https://github.com/onnx/tutorials#converting-to-onnx-format). 

### <a name="supported-frameworks"></a>Marcos admitidos

Los convertidores de ONNX permiten convertir los modelos entrenados de diferentes marcos de aprendizaje automático al formato ONNX. Entre los convertidores más usados se encuentran: 

* [PyTorch](http://pytorch.org/docs/master/onnx.html)
* [Tensorflow](https://github.com/onnx/tensorflow-onnx)
* [Keras](https://github.com/onnx/keras-onnx)
* [Scikit-learn](https://github.com/onnx/sklearn-onnx)
* [CoreML](https://github.com/onnx/onnxmltools)

Para obtener la lista completa de los marcos de trabajo admitidos, consulte [Conversión al formato ONNX](https://github.com/onnx/tutorials#converting-to-onnx-format).

## <a name="limitations"></a>Limitaciones

Actualmente, no todas las versiones de ONNX son compatibles con Azure SQL Database Edge. La compatibilidad se limita a los modelos con **tipos de datos numéricos**:

- [int y bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql5)
- [real y float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Otros tipos numéricos se pueden convertir en tipos compatibles mediante [CAST y CONVERT](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql).

Las entradas del modelo deben estructurarse de manera que cada entrada al modelo corresponda a una sola columna en una tabla. Por ejemplo, si usa una trama de datos de Pandas para entrenar un modelo, cada entrada debe ser una columna independiente del modelo.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de SQL Database Edge mediante Azure Portal](deploy-portal.md)
- [Implementación de un modelo de ONNX en la versión preliminar de Azure SQL Database Edge](deploy-onnx.md)
