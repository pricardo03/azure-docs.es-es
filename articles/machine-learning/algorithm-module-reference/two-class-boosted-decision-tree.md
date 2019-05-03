---
title: 'Árbol de decisión ampliado de dos clases: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de árbol de decisión impulsado de dos clases en el servicio de Azure Machine Learning para crear un modelo que se basa en el algoritmo de árboles de decisión ampliado de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 09ea530cac5bdbd62208f134177e5ceaccb545e2
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027951"
---
# <a name="two-class-boosted-decision-tree-module"></a>Módulo del árbol de decisión impulsado de dos clases

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de aprendizaje automático que se basa en el algoritmo de árboles de decisión ampliado. 

Un árbol de decisión impulsado es un conjunto en el que el árbol de la segunda corrige los errores del primer árbol, el método de aprendizaje el árbol de la tercero corrige los errores de los árboles de primeros y segundo y así sucesivamente.  Las predicciones se basan en el conjunto completo de árboles juntos que hace que la predicción.
  
Por lo general, cuando se configura correctamente, árboles de decisión incrementados son los métodos más sencillos con el que se va a obtener el máximo rendimiento en una amplia variedad de tareas de aprendizaje automático. Sin embargo, también son uno de los aprendices más intensivo de memoria y la implementación actual retiene todo en la memoria. Por lo tanto, un modelo de árbol de decisión impulsado podría no ser capaz de procesar los grandes conjuntos de datos que pueden administrar algunos aprendices lineales.

## <a name="how-to-configure"></a>Configuración

Este módulo crea un modelo de clasificación no entrenado. Dado que la clasificación es un método de aprendizaje supervisado, para entrenar el modelo, necesita un *etiquetar el conjunto de datos* que incluye una columna de etiqueta con un valor para todas las filas.

Puede entrenar este tipo de modelo mediante [entrenar modelo](././train-model.md). 

1.  En Azure Machine Learning, agregue el **Boosted Decision Tree** módulo al experimento.
  
2.  Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.
  
    + **Único parámetro**: Si sabe cómo desea configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.
  
  
3.  Para **número máximo de hojas por árbol**, indicar el número máximo de nodos terminales (hojas) que se pueden crear en cualquier árbol.
  
     Al aumentar este valor, podría aumentar el tamaño del árbol y obtener una mayor precisión, con el riesgo de sobreajuste y de mayor tiempo de entrenamiento.
  
4.  Para **número mínimo de muestras por nodo hoja**, indicar el número de casos necesarios para crear un nodo terminal (hoja) en un árbol.  
  
     Al aumentar este valor, aumente el umbral para la creación de nuevas reglas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener al menos cinco casos que cumplen las condiciones.
  
5.  Para **velocidad de aprendizaje**, escriba un número entre 0 y 1 que define el tamaño de paso durante el aprendizaje.  
  
     La velocidad de aprendizaje determina la rapidez o lentitud el aprendiz converge en la solución óptima. Si el tamaño de paso es demasiado grande, podría pasar por alto la solución óptima. Si el tamaño de paso es demasiado pequeño, entrenamiento tarda más tiempo para converger la mejor solución.
  
6.  Para **número de árboles construidos**, indique el número total de árboles de decisión para crear en el conjunto. Si crea más árboles de decisión, puede obtener una mejor cobertura, pero aumentará el tiempo de entrenamiento.
  
     Este valor también controla el número de árboles que se muestra cuando se visualice el modelo entrenado. Si desea ver o imprimir un único árbol, establezca el valor en 1. Sin embargo, al hacerlo, se genera un único árbol (es decir, el árbol con el conjunto inicial de parámetros) y no se realizan las iteraciones adicionales.
  
7.  Para **valor de inicialización aleatorio número**, opcionalmente, escriba un entero no negativo que se usará como el valor de inicialización aleatorio. Especificar un valor de inicialización garantiza la reproducibilidad durante las ejecuciones que tienen los mismos datos y los parámetros.  
  
     El valor de inicialización aleatorio se establece de forma predeterminada en 0, lo que significa que el valor de inicialización inicial se obtiene del reloj del sistema.  Ejecuciones sucesivas, con un valor de inicialización aleatorio pueden tener resultados diferentes.
  

9. Entrenar el modelo.
  
    + Si establece **crear modo de entrenador** a **único parámetro**, conectarse a un conjunto de datos etiquetado y la [Train Model](./train-model.md) módulo.  
  
   
## <a name="results"></a>Results

Una vez completado el entrenamiento del modelo, haga clic en la salida de [Train Model](./train-model.md) para ver los resultados:

+ Para ver el árbol de la que se creó en cada iteración, seleccione **visualizar**. 
+ Para explorar en profundidad las divisiones y ver las reglas para cada nodo, haga clic en cada árbol.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 