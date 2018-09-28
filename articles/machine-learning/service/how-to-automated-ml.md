---
title: 'Descripción del aprendizaje automático: Azure Machine Learning'
description: En este artículo, aprenderá sobre el aprendizaje automático. El servicio Azure Machine Learning puede elegir automáticamente un algoritmo y generar un modelo a partir de él. El aprendizaje automático ayuda a ahorrar tiempo al usar los parámetros y los criterios que se proporcionan para seleccionar el mejor algoritmo para el modelo.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: krishnan
author: krishnaanumalasetty
ms.date: 9/24/2018
ms.openlocfilehash: 2a9c05b68d05102fab80b2aa8fb1c1dad8a367ea
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960046"
---
# <a name="what-is-automated-machine-learning"></a>Descripción del aprendizaje automático

En este artículo, aprenderá sobre el aprendizaje automático. El servicio Azure Machine Learning puede elegir automáticamente un algoritmo y generar un modelo a partir de él. El aprendizaje automático ayuda a ahorrar tiempo al usar los parámetros y los criterios que se proporcionan para seleccionar el mejor algoritmo para el modelo.

## <a name="how-it-works"></a>Cómo funciona

1. El usuario configura el tipo de problema de aprendizaje automático que está intentando resolver. Se admiten dos categorías de aprendizaje supervisado:
   + clasificación
   + Regresión

   Consulte la [lista de modelos](how-to-configure-auto-train.md#select-your-experiment-type) que Azure Machine Learning puede probar al realizar el aprendizaje.

1. Especifique el origen y el formato de los datos de aprendizaje. Los datos deben estar etiquetados y se pueden almacenar en el entorno de desarrollo o en Azure Blob Storage. Si los datos se almacenan en el entorno de desarrollo, deben estar en el mismo directorio que los scripts de entrenamiento. Este directorio se copia en el destino de proceso que seleccione para el aprendizaje.

    En el script de aprendizaje, se pueden leer los datos en las matrices de Numpy o un dataframe de Pandas.

    Puede configurar las opciones de división para seleccionar datos de aprendizaje y de validación, o bien puede especificar diferentes conjuntos de datos de aprendizaje y validación.

1. Configure el [destino de proceso](how-to-set-up-training-targets.md) que se usa para entrenar el modelo.

1. Establezca la configuración del aprendizaje automático. Esto controla los parámetros utilizados a medida que Azure Machine Learning itera por los diferentes modelos, configuraciones de hiperparámetros, y qué métrica ver al determinar el mejor modelo. 

1. Envíe una ejecución de aprendizaje.

Durante el aprendizaje, el servicio de Azure Machine Learning crea una serie de canalizaciones que prueban distintos parámetros y algoritmos. Se detendrá cuando se alcance el límite de iteración proporcionado, o cuando alcance el valor de destino para la métrica que especifique.

[ ![Automated Machine learning](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Puede inspeccionar la información de ejecución registrada, que contiene las métricas que se recopilan durante la ejecución. La ejecución de aprendizaje también genera un objeto serializado de Python (archivo .pkl) que contiene el modelo y el preprocesamiento de los datos.

## <a name="next-steps"></a>Pasos siguientes

Vea ejemplos y aprenda cómo generar modelos mediante Automated Machine Learning:

+ [Tutorial: Automatically train a classification model with Azure Automated Machine Learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento automático de un modelo de clasificación con Azure Automated Machine Learning)

+ [How to configure settings for automatic training](how-to-configure-auto-train.md) (Configuración de opciones del aprendizaje automático)

+ [How to use automatic training on a remote resource](how-to-auto-train-remote.md) (Uso del aprendizaje automático en un recurso remoto) 
