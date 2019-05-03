---
title: 'Regresión de bosque de decisión: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de Perceptrón promedio de dos clases en el servicio de Azure Machine Learning para crear un modelo basado en el algoritmo de perceptrón promedio de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f0fec525ed87f91cf102053383b2934aac4b71c0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029241"
---
# <a name="two-class-averaged-perceptron-module"></a>Módulo de Perceptrón promedio de dos clases

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo basado en el algoritmo de perceptrón promedio de aprendizaje automático.  
  
Este algoritmo de clasificación es un método de aprendizaje supervisado y requiere un *etiquetados dataset*, que incluye una columna de etiqueta. Puede entrenar el modelo, ya que proporciona el modelo y el conjunto de datos etiquetado como entrada para [entrenar modelo](./train-model.md). El modelo entrenado, a continuación, puede utilizarse para predecir valores para los nuevos ejemplos de entrada.  

### <a name="about-averaged-perceptron-models"></a>Acerca de los modelos de perceptrón promedio

El *método de perceptrón promedio de* es una versión primeriza y sencilla de una red neuronal. En este enfoque, las entradas se clasifican en varias posibles salidas según una función lineal y, a continuación, junto con un conjunto de pesos que se derivan del vector de característica, por lo tanto, el nombre "perceptrón".

Los modelos de perceptrón más sencillos son adecuados para aprender patrones linealmente separables, mientras que redes neurales (especialmente las redes neuronales profundas) pueden modelar límites de clase más complejos. Sin embargo, los perceptrones son más rápidos, y puesto que procesan casos en serie, pueden usarse con entrenamiento continuo.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>Configuración de Perceptrón promedio de dos clases

1.  Agregar el **Perceptrón promedio de dos clases** módulo al experimento.  

2.  Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.  
  
    -   **Único parámetro**: Si sabe cómo desea configurar el modelo, debe proporcionar un conjunto específico de valores como argumentos.
  
3.  Para **velocidad de aprendizaje**, especifique un valor para el *velocidad de aprendizaje*. El tamaño del paso que se utiliza en descenso de gradiente estocástico cada vez que se prueba y se corrige el modelo de control de valores de la velocidad de aprendizaje.
  
     Al realizar la tasa más pequeños, probar el modelo más a menudo, con el riesgo que puede quedar atascado en un nivel predefinido local. Al realizar el paso más grandes, puede convergir con mayor rapidez, con el riesgo de rebasar los mínimos reales.
  
4.  Para **número máximo de iteraciones**, escriba el número de veces que desea que el algoritmo para examinar los datos de entrenamiento.  
  
     Si se detiene pronto a menudo proporciona una mejor generalización. Aumentar el número de iteraciones mejora el ajuste, con el riesgo de sobreajuste.
  
5.  Para **valor de inicialización aleatorio número**, opcionalmente, escriba un valor entero que se usará como el valor de inicialización. Uso de un valor de inicialización se recomienda si desea garantizar la reproducibilidad del experimento en se ejecuta.  
  
1.  Conecte un conjunto de datos de entrenamiento y uno de los módulos de aprendizaje:
  
    -   Si establece **crear modo de entrenador** a **único parámetro**, utilice el [Train Model](train-model.md) módulo.

## <a name="results"></a>Results

Una vez completada la formación:

+ Para ver un resumen de los parámetros del modelo, junto con los pesos de característica aprendidos de entrenamiento, haga clic en la salida de [Train Model](./train-model.md).


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 