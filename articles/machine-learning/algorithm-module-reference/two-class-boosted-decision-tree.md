---
title: 'Árbol de decisión ampliado de dos clases: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de árbol de decisión ampliado de dos clases en el servicio Azure Machine Learning para crear un modelo de Machine Learning basado en el algoritmo de árboles de decisión ampliados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 5795dc994872ac5da37a1226524dddd35aa35126
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692633"
---
# <a name="two-class-boosted-decision-tree-module"></a>Módulo de árbol de decisión ampliado de dos clases

En este artículo se describe un módulo de la interfaz visual (versión preliminar) del servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de Machine Learning basado en el algoritmo de árboles de decisión ampliados. 

Un árbol de decisión ampliado es un método de aprendizaje de conjuntos en que el segundo árbol corrige los errores del primer árbol, el tercer árbol corrige los errores del primer y del segundo árbol y así sucesivamente.  Las predicciones se basan en el conjunto completo de árboles juntos que establecen la predicción.
  
Por lo general, cuando se configuran correctamente, los árboles de decisión ampliados son los métodos más sencillos para obtener el máximo rendimiento en una amplia variedad de tareas de aprendizaje automático. Sin embargo, también son uno de los elementos aprendices que consumen más recursos de memoria y la implementación actual retiene todo en la memoria. Por tanto, un modelo de árbol de decisión ampliado podría no ser capaz de procesar los grandes conjuntos de datos que algunos elementos aprendices lineales pueden administrar.

## <a name="how-to-configure"></a>Configuración

En este módulo se crea un modelo de clasificación no entrenado. Dado que la clasificación es un método de aprendizaje supervisado, para entrenar el modelo, necesita un *conjunto de datos etiquetado* que incluye una columna de etiqueta con un valor para todas las filas.

Puede entrenar este tipo de modelo mediante [Entrenar modelo](././train-model.md). 

1.  En Azure Machine Learning, agregue el módulo **Boosted Decision Tree** (Árbol de decisión ampliado) a la canalización.
  
2.  Especifique cómo quiere que se entrene el modelo, estableciendo la opción **Create trainer mode** (Crear modo entrenador).
  
    + **Parámetro único**: Si sabe cómo quiere configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.
  
  
3.  En **Número máximo de hojas por árbol**, indique el número máximo de nodos terminales (hojas) que se pueden crear en un árbol.
  
     Al aumentar este valor, podría aumentar el tamaño del árbol y obtener una mayor precisión, a riesgo de experimentar un sobreajuste y un mayor tiempo de entrenamiento.
  
4.  En **Número mínimo de muestras por nodo hoja**, indique el número de casos necesario para crear cualquier nodo terminal (hoja) en un árbol.  
  
     Al aumentar este valor, aumenta el umbral para crear reglas nuevas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener cinco casos como mínimo que cumplan las mismas condiciones.
  
5.  En **Velocidad de aprendizaje**, escriba un número entre 0 y 1 que defina el tamaño de paso durante el aprendizaje.  
  
     La velocidad de aprendizaje determina la rapidez o lentitud con la que el aprendiz converge en la solución óptima. Si el tamaño del paso es demasiado grande, puede pasar por alto la solución óptima. Si el tamaño del paso es demasiado pequeño, el entrenamiento tarda más tiempo en converger en la mejor solución.
  
6.  En **Número de árboles construidos**, indique el número total de árboles de decisión que se van a crear en el conjunto. Si crea más árboles de decisión, puede obtener potencialmente mejor cobertura, pero aumenta el tiempo de entrenamiento.
  
     Este valor también controla el número de árboles que se muestran al visualizar el modelo entrenado. Si quiere ver o imprimir un único árbol, establezca el valor en 1. Sin embargo, al hacerlo, solo se produce un único árbol (el árbol con el conjunto inicial de parámetros) y no se realizan iteraciones adicionales.
  
7.  En **Inicialización con número aleatorio**, escriba opcionalmente un número entero no negativo para que se use como valor de inicialización aleatorio. Al especificar un valor, se garantiza la reproducibilidad durante las ejecuciones que tienen los mismos datos y parámetros.  
  
     De forma predeterminada, el valor de inicialización aleatorio se establece en 0, lo que significa que el valor de inicialización inicial se obtiene a partir del reloj del sistema.  Las ejecuciones sucesivas, con un valor de inicialización aleatorio, pueden tener resultados diferentes.
  

9. Entrene el modelo.
  
    + Si establece **Create trainer mode** (Crear modo entrenador) en **Single Parameter** (Parámetro único), conecte un conjunto de datos etiquetado y el módulo [Entrenar modelo](./train-model.md).  
  
   
## <a name="results"></a>Results

Una vez completado el entrenamiento del modelo, haga clic en la salida de [Entrenar modelo](./train-model.md) para ver los resultados:

+ Para ver el árbol que se ha creado en cada iteración, seleccione **Visualizar**. 
+ Haga clic en cada árbol para explorar en profundidad las divisiones y ver las reglas de cada nodo.


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 