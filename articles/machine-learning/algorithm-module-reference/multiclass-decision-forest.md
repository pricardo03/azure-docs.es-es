---
title: 'Bosque de decisión multiclase: referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de bosque de decisión multiclase en Azure Machine Learning Service para crear un modelo de Machine Learning basado en el algoritmo de *bosques de decisión*.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: b56f08dfd1a14ffedffb612bb8974086ee08ede7
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128615"
---
# <a name="multiclass-decision-forest-module"></a>Módulo sobre el bosque de decisión multiclase

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para crear un modelo de Machine Learning basado en el algoritmo de *bosques de decisión*. Un bosque de decisión es un modelo de conjunto que genera rápidamente una serie de árboles de decisión, al mismo tiempo que aprende de los datos etiquetados.

## <a name="more-about-decision-forests"></a>Más información sobre los bosques de decisión

Este algoritmo de bosque de decisión es un método de aprendizaje de conjunto para la clasificación. El algoritmo crea varios árboles de decisión y después *se vota* la clase de resultado más popular. Votar es una forma de agregación en la que cada árbol de un bosque de decisión de clasificación produce un histograma de etiquetas de frecuencia no normalizada. El proceso de agregación suma estos histogramas y normaliza el resultado para obtener las "probabilidades" de cada etiqueta. Los árboles con un nivel alto de confianza en la predicción tienen un peso mayor en la decisión final del conjunto.

Los árboles de decisión en general son modelos no paramétricos, lo que significa que admiten datos con distribuciones variadas. En cada árbol se ejecuta una secuencia de pruebas simples para cada clase, lo que aumenta los niveles de la estructura de árbol hasta que se alcanza un nodo hoja (decisión).

Los árboles de decisión tienen muchas ventajas:

+ Pueden representar límites de decisión no lineales.
+ Son eficientes tanto en el cálculo como en la utilización de la memoria durante el entrenamiento y la predicción.
+ Realizan la selección y clasificación de características integradas.
+ Son resistentes en presencia de características ruidosas.

El clasificador del bosque de decisión en Azure Machine Learning consta de un conjunto de árboles de decisión. Por lo general, los modelos de conjunto proporcionan mejor cobertura y precisión que los árboles de decisión únicos. Para más información, vea [Bosques de decisión](https://go.microsoft.com/fwlink/?LinkId=403677).

## <a name="how-to-configure-multiclass-decision-forest"></a>Cómo configurar un bosque de decisión multiclase



1. Agregue el módulo **Bosque de decisión multiclase** a su experimento en la interfaz. Puede encontrar este módulo en **Machine Learning**, **Inicializar modelo** y **Clasificación**.

2. Haga doble clic en el módulo para abrir el panel **Propiedades**.

3. Para obtener información sobre el **método de nuevo muestreo**, elija el método utilizado para crear los árboles individuales.  Puede elegir entre agregación o replicación.

    + **Agregación**: La agregación también se denomina *agregación de arranque*. En este método, cada árbol crece en una muestra nueva, creada al muestrear de forma aleatoria el conjunto de datos original con el conjunto de reemplazo hasta que haya un conjunto de datos con el tamaño del original. Los resultados de los modelos se combinan mediante *votación*, que es una forma de agregación. Para obtener más información, consulte la entrada de Wikipedia sobre la agregación de arranque.

    + **Replicación**: En la replicación, cada árbol se entrena exactamente con los mismos datos de entrada. La determinación de qué predicado de división se utiliza para cada nodo de árbol sigue siendo aleatoria, lo que crea árboles diversos.

   

4. Especifique cómo quiere que se entrene el modelo, estableciendo la opción **Create trainer mode** (Crear modo entrenador).

    + **Single Parameter** (Parámetro único): seleccione esta opción si sabe cómo quiere configurar el modelo y proporcione un conjunto de valores como argumentos.


5. **Número de árboles de decisión**: escriba el número máximo de árboles de decisión que se pueden crear en el conjunto. Si crea más árboles de decisión, puede obtener potencialmente mejor cobertura, pero puede aumentar el tiempo de entrenamiento.

    Este valor también controla el número de árboles que se muestran en los resultados al visualizar el modelo entrenado. Para ver o imprimir un único árbol, puede establecer el valor en 1; sin embargo, esto significa que solo se puede producir un único árbol (el árbol con el conjunto inicial de parámetros) y que no se realizan más iteraciones.

6. **Profundidad máxima de los árboles de decisión**: escriba un número para limitar la profundidad máxima de cualquier árbol de decisión. Al aumentar la profundidad del árbol podría aumentar la precisión, a riesgo de que se produzca un sobreajuste y aumente el tiempo de entrenamiento.

7. **Número de divisiones aleatorias por nodo**: escriba el número de divisiones que se usarán al crear cada nodo del árbol. Una *división* significa que las características de cada nivel del árbol (nodo) se dividen al azar.

8. **Número mínimo de muestras por nodo hoja**: indique el número mínimo de casos necesarios para crear un nodo terminal (hoja) en un árbol. Al aumentar este valor, aumenta el umbral para crear reglas nuevas.

    Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener cinco casos como mínimo que cumplan las mismas condiciones.



10. Conecte un conjunto de datos etiquetados y uno de los módulos de entrenamiento:

    + Si establece **Create trainer mode** (Crear modo entrenador) en **Single Parameter** (Parámetro único), use el módulo [Entrenar modelo](./train-model.md).

11. Ejecute el experimento.

## <a name="results"></a>Results

Una vez completado el entrenamiento:

+ Para ver el árbol que se ha creado en cada iteración, haga clic con el botón derecho en el resultado del módulo [Modelo de entrenamiento](./train-model.md) y seleccione **Visualizar**.
+ Haga clic en cada árbol para explorar en profundidad las divisiones y ver las reglas de cada nodo.


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 