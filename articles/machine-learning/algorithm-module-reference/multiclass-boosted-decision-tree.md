---
title: 'Árbol de decisión ampliado multiclase: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo Multiclass Boosted Decision Tree (Árbol de decisión ampliado multiclase) en Azure Machine Learning para crear un clasificador mediante datos etiquetados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 11/19/2019
ms.openlocfilehash: 73a832d98d0b17f59a98e2d57cf801818c7ed1da
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76311944"
---
# <a name="multiclass-boosted-decision-tree"></a>Árbol de decisión ampliado multiclase

En este artículo se describe un módulo del diseñador de Azure Machine Learning.

Utilice este módulo para crear un modelo de Machine Learning basado en el algoritmo de árboles de decisión ampliados.

Un árbol de decisión ampliado es un método de aprendizaje de conjuntos en que el segundo árbol corrige los errores del primer árbol, el tercer árbol corrige los errores del primer y del segundo árbol y así sucesivamente. Las predicciones se basan en el conjunto de árboles juntos.

## <a name="how-to-configure"></a>Cómo se configura 

En este módulo se crea un modelo de clasificación no entrenado. Dado que la clasificación es un método de aprendizaje supervisado, necesita un *conjunto de datos etiquetado* que incluya una columna de etiqueta con un valor para todas las filas.

Puede entrenar este tipo de modelo mediante [Entrenar modelo](././train-model.md). 

1.  Agregue el módulo **Árbol de decisión ampliado multiclase** a la canalización.

1.  Especifique cómo quiere que se entrene el modelo. Para ello, establezca la opción **Create trainer mode** (Crear modo entrenador).

    + **Single Parameter** (Parámetro único): Si sabe cómo quiere configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.


    *  **Maximum number of leaves per tree** (Número máximo de hojas por árbol) limita el número máximo de nodos terminales (hojas) que se pueden crear en un árbol.
    
        Al aumentar este valor, podría aumentar el tamaño del árbol y obtener una mayor precisión, a riesgo de experimentar un sobreajuste y un mayor tiempo de entrenamiento.
  
    * **Minimum number of samples per leaf node** (Número mínimo de muestras por nodo hoja) indica el número de casos necesarios para crear cualquier nodo terminal (hoja) en un árbol.  

         Al aumentar este valor, aumenta el umbral para crear reglas nuevas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener cinco casos como mínimo que cumplan las mismas condiciones.

    * La **velocidad de aprendizaje** define el tamaño del paso durante el aprendizaje. Especifique un número comprendido entre 0 y 1.

         La velocidad de aprendizaje determina la rapidez o la lentitud con la que el aprendiz converge en una solución óptima. Si el tamaño del paso es demasiado grande, puede pasar por alto la solución óptima. Si el tamaño del paso es demasiado pequeño, el entrenamiento tarda más tiempo en converger en la mejor solución.

    * **Number of trees constructed** (Número de árboles construidos) indica el número total de árboles de decisión que se van a crear en el conjunto. Si crea más árboles de decisión, puede obtener una cobertura potencialmente mejor, pero aumentará el tiempo de entrenamiento.

    *  **Random number seed** (Inicialización con número aleatorio) establece opcionalmente un número entero no negativo para que se use como valor de inicialización aleatorio. Al especificar un valor, se garantiza la reproducibilidad durante las ejecuciones que tienen los mismos datos y parámetros.  

         El valor de inicialización aleatorio se establece de forma predeterminada en 42. Las ejecuciones sucesivas que usen un valor de inicialización aleatorio diferente podrían tener resultados diferentes.

> [!Note]
> Si establece **Create trainer mode** (Crear modo entrenador) en **Single Parameter** (Parámetro único), conecte un conjunto de datos etiquetado y el módulo [Entrenar modelo](./train-model.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 
