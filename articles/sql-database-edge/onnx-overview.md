---
title: Aprendizaje automático e inteligencia artificial con ONNX en la versión preliminar de Azure SQL Database Edge | Microsoft Docs
description: El aprendizaje automático en la versión preliminar de Azure SQL Database Edge admite modelos del formato Open Neural Network Exchange (ONNX). ONNX es un formato abierto que puede usar para intercambiar modelos entre varios marcos y herramientas de aprendizaje automático.
keywords: Implementación de SQL Database Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: ronychatterjee
ms.author: achatter
ms.reviewer: davidph
ms.date: 11/04/2019
ms.openlocfilehash: c4c87f7f6f8735c9a50c61b0e083c77b915e0d98
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510649"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-database-edge-preview"></a>Aprendizaje automático e inteligencia artificial con ONNX en la versión preliminar de SQL Database Edge

El aprendizaje automático en la versión preliminar de Azure SQL Database Edge admite modelos del formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/). ONNX es un formato abierto que puede usar para intercambiar modelos entre varios [marcos y herramientas de aprendizaje automático](https://onnx.ai/supported-tools).

## <a name="supported-tool-kits"></a>Kits de herramientas compatibles

ONNXMLTools permite convertir modelos de diferentes kits de herramientas de aprendizaje automático a un modelo de ONNX. Actualmente, para los tipos de datos numéricos y las entradas de columna única, se admiten los siguientes kits de herramientas:

* [scikit-learn](https://github.com/onnx/sklearn-onnx)
* [Tensorflow](https://github.com/onnx/tensorflow-onnx)
* [Keras](https://github.com/onnx/keras-onnx)
* [CoreML](https://github.com/onnx/onnxmltools)
* [Spark ML (experimental)](https://github.com/onnx/onnxmltools/tree/master/onnxmltools/convert/sparkml)
* [LightGBM](https://github.com/onnx/onnxmltools)
* [libsvm](https://github.com/onnx/onnxmltools)
* [XGBoost](https://github.com/onnx/onnxmltools)

## <a name="get-onnx-models"></a>Obtención de modelos de ONNX

Hay varias maneras de obtener un modelo en el formato ONNX:

- [ONNX Model Zoo](https://github.com/onnx/models): Contiene varios modelos de ONNX previamente entrenados para diferentes tipos de tareas. Puede descargar y usar versiones compatibles con Windows Machine Learning.

- [Exportación nativa desde marcos de entrenamiento de aprendizaje automático](https://onnx.ai/supported-tools): Varios marcos de aprendizaje admiten la funcionalidad de exportación nativa a ONNX, que permite guardar el modelo entrenado en una versión específica del formato ONNX. Por ejemplo, Chainer, Caffee2 y PyTorch. Además, los servicios como [Azure Machine Learning Service](https://azure.microsoft.com/services/machine-learning-service/) y [Azure Custom Vision](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) también proporcionan una exportación nativa a ONNX.

  - Para obtener información sobre cómo entrenar y exportar un modelo de ONNX en la nube mediante Custom Vision, consulte [Tutorial: Uso de un modelo ONNX de Custom Vision con Windows ML (versión preliminar)](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/custom-vision-onnx-windows-ml).

- [Convertir modelos existentes mediante WinMLTools](https://docs.microsoft.com/windows/ai/windows-ml/convert-model-winmltools): Este paquete de Python permite convertir los modelos de varios formatos de marco de entrenamiento a ONNX. Puede especificar a qué versión de ONNX desea convertir el modelo, en función de las compilaciones de Windows a las que se destina la aplicación. Si no está familiarizado con Python, puede usar el [panel basado en interfaz de usuario de Windows Machine Learning](https://github.com/Microsoft/Windows-Machine-Learning/tree/master/Tools/WinMLDashboard) para convertir los modelos.

> [!IMPORTANT]
> No todas las versiones de ONNX son compatibles con Azure SQL Database Edge. Actualmente solo se admite la predicción de tipos de datos numéricos a través del modelo de ONNX.

Una vez que tenga un modelo de ONNX, puede implementarlo en Azure SQL Database Edge. Después, puede usar [la puntuación nativa con la función de predicción de T-SQL](/sql/advanced-analytics/sql-native-scoring/).

## <a name="limitations"></a>Limitaciones

Actualmente, esta compatibilidad está limitada a los modelos con **tipos de datos numéricos**:

- [int y bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql5)
- [real y float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Otros tipos numéricos se pueden convertir en tipos compatibles mediante CAST y CONVERT [CAST y CONVERT](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql).

Las entradas del modelo deben estructurarse de manera que cada entrada al modelo corresponda a una sola columna en una tabla. Por ejemplo, si usa una trama de datos de Pandas para entrenar un modelo, cada entrada debe ser una columna independiente del modelo.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de SQL Database Edge mediante Azure Portal](deploy-portal.md)
- [Implementación de un modelo de ONNX en la versión preliminar de Azure SQL Database Edge](deploy-onnx.md)