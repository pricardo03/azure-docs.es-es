---
title: Selección y optimización automáticas del algoritmo de Machine Learning
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo Azure Machine Learning Service puede elegir de forma automática un algoritmo y generar un modelo a partir de él para ahorrar tiempo, mediante los parámetros y criterios que proporcione con el fin de seleccionar el mejor algoritmo para el modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 620dbd22613df37fdc3c20e34906684446b2251f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60821195"
---
# <a name="what-is-automated-machine-learning"></a>Descripción del aprendizaje automático

El aprendizaje automático automatizado es el proceso de tomar datos de entrenamiento con una característica de destino definido y realizar una iteración por las combinaciones de algoritmos y selecciones de características para seleccionar de forma automática el mejor modelo para los datos según las puntuaciones de entrenamiento. El proceso tradicional de desarrollo de modelos de aprendizaje automático consume muchos recursos y requiere conocimiento del dominio y una inversión de tiempo significativa para ejecutar y comparar los resultados de docenas de modelos. El aprendizaje automático automatizado simplifica este proceso mediante la generación de modelos optimizados a partir de las restricciones y los objetivos definidos para el experimento, como el tiempo que se va a ejecutar el experimento o qué modelos se bloquean.

## <a name="how-it-works"></a>Cómo funciona

1. El usuario configura el tipo de problema de aprendizaje automático que está intentando resolver. Se admiten categorías de aprendizaje supervisado:
   + clasificación
   + Regresión
   + Previsión

   Mientras que el aprendizaje automático automatizado está disponible con carácter general, **la característica de previsión aún está en versión preliminar pública.**

   Consulte la [lista de modelos](how-to-configure-auto-train.md#select-your-experiment-type) que Azure Machine Learning puede probar al realizar el aprendizaje.

1. Especifique el origen y el formato de los datos de aprendizaje. Los datos deben estar etiquetados y se pueden almacenar en el entorno de desarrollo o en Azure Blob Storage. Si los datos se almacenan en el entorno de desarrollo, deben estar en el mismo directorio que los scripts de entrenamiento. Este directorio se copia en el destino de proceso que seleccione para el aprendizaje.

    En el script de aprendizaje, se pueden leer los datos en las matrices de Numpy o un dataframe de Pandas.

    Puede configurar las opciones de división para seleccionar datos de aprendizaje y de validación, o bien puede especificar diferentes conjuntos de datos de aprendizaje y validación.

1. Configure el [destino de proceso](how-to-set-up-training-targets.md) que se usa para entrenar el modelo.

1. Establezca la configuración del aprendizaje automático. Esto controla los parámetros que se usan a medida que Azure Machine Learning itera por los diferentes modelos, configuraciones de hiperparámetros, y qué métrica se examina al determinar el mejor modelo

1. Envíe una ejecución de aprendizaje.

Durante el aprendizaje, el servicio de Azure Machine Learning crea una serie de canalizaciones que prueban distintos parámetros y algoritmos. Se detendrá cuando se alcance el límite de iteración proporcionado, o cuando alcance el valor de destino para la métrica que especifique.

[![Aprendizaje automático automatizado](./media/how-to-automated-ml/automated-machine-learning.png)](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Puede inspeccionar la información de ejecución registrada, que contiene las métricas que se recopilan durante la ejecución. La ejecución de entrenamiento también genera un objeto serializado de Python (archivo `.pkl`) que contiene el modelo y el preprocesamiento de los datos.


## <a name="next-steps"></a>Pasos siguientes

Vea ejemplos y aprenda cómo generar modelos mediante Automated Machine Learning:

+ [Tutorial: Automatically train a classification model with Azure Automated Machine Learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento automático de un modelo de clasificación con Azure Machine Learning automatizado)

+ [Ejemplos de Bloc de notas](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)

+ [Entrenamiento de modelos con aprendizaje automático automatizado en la nube](how-to-auto-train-remote.md)

+ [Configuración del experimento de aprendizaje automático automatizado](how-to-configure-auto-train.md)
