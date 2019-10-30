---
title: 'Máquina de vectores de soporte de dos clases: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de **Máquina de vectores de soporte de dos clases** en Azure Machine Learning Service para crear un modelo basado en el algoritmo de máquina de vectores de soporte.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: c58e66070f137f9c5d713a45682afac5f30bbd1e
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692605"
---
# <a name="two-class-support-vector-machine-module"></a>Máquina de vectores de soporte de dos clases

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Use este módulo para crear un modelo basado en el algoritmo de máquina de vectores de soporte. 

Las máquinas de vectores de soporte (SVM) son una clase contrastada de métodos de aprendizaje supervisado. Esta implementación determinada es adecuada para la predicción de dos resultados posibles basados en las variables continuas o de categoría.

Después de definir los parámetros del modelo, entrene el modelo mediante los módulos de entrenamiento y proporcione un *conjunto de datos etiquetado* que incluya una columna de etiqueta o resultado.

## <a name="about-support-vector-machines"></a>Acerca de las máquinas de vectores de soporte

Las máquinas de vectores de soporte están entre los primeros algoritmos de aprendizaje automático, y los modelos de SVM se han usado en muchas aplicaciones, desde la recuperación de información a la clasificación de texto e imágenes. Las SVM pueden usarse para tareas de clasificación y regresión.

Este modelo de SVM es un modelo de aprendizaje supervisado que requiere datos etiquetados. En el proceso de entrenamiento, el algoritmo analiza los datos de entrada y reconoce patrones en un espacio de características multidimensionales denominado *hiperplano*.  Todos los ejemplos de entrada se representan como puntos en este espacio y se asignan a categorías de salida, de tal manera que las categorías se dividen de forma tan amplia y clara como sea posible.

Para la predicción, el algoritmo de SVM asigna ejemplos nuevos a una categoría u otra, asignándolos en ese mismo espacio. 

## <a name="how-to-configure"></a>Configuración 

Para este tipo de modelo, se recomienda normalizar el conjunto de datos antes de usarlo para entrenar al clasificador.
  
1.  Agregue el módulo **Máquina de vectores de soporte de dos clases** a la canalización.  
  
2.  Especifique cómo quiere que se entrene el modelo, estableciendo la opción **Create trainer mode** (Crear modo entrenador).  
  
    -   **Parámetro único**: Si sabe cómo quiere configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.  

3.  En **Número de iteraciones**, escriba un número que indique el número de iteraciones usadas al compilar el modelo.  
  
     Este parámetro puede usarse para controlar el equilibrio entre velocidad de entrenamiento y precisión.  
  
4.  En **Lambda**, escriba un valor que se usará como peso para la regularización L1.  
  
     Este coeficiente de regularización puede usarse para optimizar el modelo. Los valores mayores penalizan modelos más complejos.  
  
5.  Seleccione la opción **Normalize features** (Normalizar características) si quiere normalizar las características antes del entrenamiento.
  
     Si aplica la normalización antes del entrenamiento, los puntos de datos se centran según la media y se escalan para que tengan una unidad de desviación estándar.
  
6.  Seleccione la opción **Project to the unit sphere** (Proyectar en la esfera de la unidad) para normalizar los coeficientes.
  
     La proyección de valores en el espacio de la unidad significa que, antes del entrenamiento, los puntos de datos se centran en 0 y se escalan para que tengan una unidad de desviación estándar.
  
7.  En **Random number seed** (Inicialización de números aleatorios), escriba un valor entero para usarlo como valor de inicialización si quiere garantizar la reproducibilidad entre ejecuciones.  En caso contrario, se usa un valor del reloj del sistema como valor de inicialización, que puede dar lugar a resultados ligeramente diferentes entre ejecuciones.
  
9. Conecte un conjunto de datos etiquetados y uno de los [módulos de entrenamiento](module-reference.md):
  
    -   Si establece **Create trainer mode** (Crear modo entrenador) en **Single Parameter** (Parámetro único), use el módulo [Entrenar modelo](train-model.md).
  

10. Ejecución de la canalización

## <a name="results"></a>Results

Una vez completado el entrenamiento:

+ Para ver un resumen de los parámetros del modelo, junto con los pesos de característica aprendidos en el entrenamiento, haga clic con el botón derecho en la salida de [Train Model](./train-model.md) (Entrenar modelo) y seleccione **Visualizar**.

+ Para utilizar los modelos entrenados para realizar predicciones, conecte el modelo entrenado al módulo [Score Model](score-model.md) (Modelo de puntuación).


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 