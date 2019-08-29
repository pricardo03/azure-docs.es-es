---
title: 'Regresión de bosque de decisión: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Perceptrón promedio de dos clases en Azure Machine Learning Service para crear un modelo de Machine Learning basado en el algoritmo de perceptrón promedio.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: f560923b0a5457ac5fd03c7f76fc4315c6ca08e8
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128435"
---
# <a name="two-class-averaged-perceptron-module"></a>Módulo Perceptrón promedio de dos clases

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para crear un modelo de Machine Learning basado en el algoritmo de perceptrón promedio.  
  
Este algoritmo de clasificación es un método de aprendizaje supervisado y requiere un *conjunto de datos etiquetado*, que incluya una columna de etiqueta. Puede entrenar el modelo proporcionando el modelo y el conjunto de datos etiquetado como entrada para [Entrenar modelo](./train-model.md). Después, el modelo entrenado puede utilizarse para predecir valores para los nuevos ejemplos de entrada.  

### <a name="about-averaged-perceptron-models"></a>Acerca de los modelos de perceptrón promedio de dos clases

El *método de perceptrón promedio* es una versión anterior y sencilla de una red neuronal. En este enfoque, las entradas se clasifican en varias posibles salidas según una función lineal y, a continuación, se combinan con un conjunto de pesos que se derivan del vector de característica, de ahí, el nombre "perceptrón".

Los modelos de perceptrón más sencillos son adecuados para aprender patrones separables linealmente, mientras que las redes neuronales (especialmente las redes neuronales profundas) pueden modelar límites de clase más complejos. Sin embargo, los perceptrones son más rápidos y, puesto que procesan los casos en serie, pueden usarse en el entrenamiento continuo.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>Procedimiento para configurar el perceptrón promedio de dos clases

1.  Agregue el módulo **Perceptrón promedio de dos clases** al experimento.  

2.  Especifique cómo quiere que se entrene el modelo, estableciendo la opción **Create trainer mode** (Crear modo entrenador).  
  
    -   **Single Parameter** (Parámetro único): Si sabe cómo quiere configurar el modelo, proporcione un conjunto específico de valores como argumentos.
  
3.  En **Velocidad de aprendizaje**, especifique un valor para la *velocidad de aprendizaje*. Los valores de velocidad de aprendizaje controlan el tamaño del paso que se utiliza en el descenso de gradiente estocástico cada vez que se prueba y se corrige el modelo.
  
     Al disminuir la velocidad, se prueba el modelo más a menudo, con el riesgo de que pueda quedarse atascado en un nivel local. Al aumentar el paso, puede convergir con mayor rapidez, con el riesgo de superar los mínimos verdaderos.
  
4.  En **Maximum number of iterations** (Número máximo de iteraciones), escriba el número de veces que desea que el algoritmo examine los datos de entrenamiento.  
  
     Si se detiene pronto proporciona, a menudo, una mejor generalización. Aumentar el número de iteraciones mejora el ajuste, con riesgo de sobreajuste.
  
5.  En **Random number seed** (Inicialización de número aleatorio), escriba opcionalmente un valor de entero que se usará como inicialización. Se recomienda usar una inicialización si desea garantizar la reproducibilidad del experimento a través de las ejecuciones.  
  
1.  Conecte un conjunto de datos de entrenamiento y uno de los módulos de entrenamiento:
  
    -   Si establece **Create trainer mode** (Crear modo entrenador) en **Single Parameter** (Parámetro único), use el módulo [Entrenar modelo](train-model.md).

## <a name="results"></a>Results

Una vez completado el entrenamiento:

+ Para ver un resumen de los parámetros del modelo, junto con los pesos de característica aprendidos en el entrenamiento, haga clic con el botón derecho en la salida de [Train Model](./train-model.md) (Entrenar modelo).


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 