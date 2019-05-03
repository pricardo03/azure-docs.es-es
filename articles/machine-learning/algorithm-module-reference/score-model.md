---
title: 'Modelo de puntuación: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo puntuar modelo en el servicio de Azure Machine Learning para generar predicciones mediante un modelo de regresión o clasificación entrenado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f8f7bfcbbf013f2cf32957772086d7e44d31e310
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029271"
---
# <a name="score-model-module"></a>Score Model module

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para generar predicciones con un modelo de clasificación o regresión entrenado.

## <a name="how-to-use"></a>Modo de uso

1. Agregar el **Score Model** módulo al experimento.

2. Adjuntar un modelo entrenado y un conjunto de datos que contiene los nuevos datos de entrada. 

    Los datos deben ser en un formato compatible con el tipo de modelo entrenado que use. El esquema del conjunto de datos de entrada también generalmente debe coincidir con el esquema de los datos utilizados para entrenar el modelo.

3. Ejecute el experimento.

## <a name="results"></a>Results

Después de haber generado un conjunto de puntuaciones mediante [Score Model](./score-model.md):

+ Para generar un conjunto de métricas utilizadas para evaluar la exactitud del modelo (rendimiento).  puede conectar el conjunto de datos puntuado a [Evaluate Model](./evaluate-model.md), 
+ Haga clic en el módulo y seleccione **visualizar** para ver un ejemplo de los resultados.
+ Guardar los resultados en un conjunto de datos.

La puntuación, o el valor de predicción, puede estar en muchos formatos diferentes, según el modelo y los datos de entrada:

- Para los modelos de clasificación [Score Model](./score-model.md) da como resultado un valor de predicción para la clase, así como la probabilidad de que el valor de predicción.
- Para los modelos de regresión, [Score Model](./score-model.md) genera simplemente el valor numérico predicho.
- Para los modelos de clasificación de imagen, la puntuación puede ser la clase del objeto en la imagen o un valor booleano que indica si se ha encontrado una característica determinada.

## <a name="publish-scores-as-a-web-service"></a>Publicar las puntuaciones de como un servicio web

Un uso común de la puntuación es devolver el resultado como parte de un servicio web predictivo. Para obtener más información, vea este tutorial sobre cómo crear un servicio web basado en un experimento en Azure Machine Learning:


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 