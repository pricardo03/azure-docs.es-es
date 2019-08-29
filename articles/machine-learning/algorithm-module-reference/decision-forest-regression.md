---
title: 'Regresión de bosque de decisión: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Decision Forest Regression (Regresión de bosque de decisión) en Azure Machine Learning Service para crear un modelo de regresión basado en un conjunto de árboles de decisión.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: b8bb3285aecb6aff399606e6263f014027a86581
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128881"
---
# <a name="decision-forest-regression-module"></a>Módulo Decision Forest Regression (Regresión de bosque de decisión)

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para crear un modelo de regresión basado en un conjunto de árboles de decisión.

Una vez que haya configurado el modelo, debe entrenarlo mediante un conjunto de datos etiquetado y el módulo [Entrenar modelo](./train-model.md).  A continuación, el modelo entrenado podrá usarse para realizar predicciones. 

## <a name="how-it-works"></a>Cómo funciona

Los árboles de decisión son modelos no paramétricos que llevan a cabo una secuencia de pruebas simples para cada instancia, atravesando una estructura de datos de árbol binario hasta alcanzar un nodo hoja (decisión).

Los árboles de decisión tienen las siguientes ventajas:

- Son eficientes tanto en el cálculo como en la utilización de la memoria durante el entrenamiento y la predicción.

- Pueden representar límites de decisión no lineales.

- Realizan una clasificación y selección de características integradas y son resistentes en presencia de características ruidosas.

Este modelo de regresión consta de un conjunto de árboles de decisión. Cada árbol de un bosque de decisión de regresión da como resultado una predicción en forma de distribución gaussiana. Se realiza una agregación sobre el conjunto de árboles para buscar la distribución gaussiana más cercana a la distribución combinada de todos los árboles del modelo.

Para obtener más información sobre el marco teórico de este algoritmo y su implementación, consulte este artículo: [Bosques de decisión: Marco unificado para la clasificación, la regresión, la estimación de densidades, el aprendizaje múltiple y el aprendizaje supervisado parcialmente](https://www.microsoft.com/en-us/research/publication/decision-forests-a-unified-framework-for-classification-regression-density-estimation-manifold-learning-and-semi-supervised-learning/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D158806#)

## <a name="how-to-configure-decision-forest-regression-model"></a>Cómo configurar el modelo de regresión de bosque de decisión

1. Agregue el módulo **Decision Forest Regression** (Regresión de bosque de decisión) al experimento. Puede encontrar el módulo en la interfaz, en **Machine Learning**, **Initialize Model** (Inicializar modelo) y **Regresión**.

2. Abra las propiedades del módulo y, para **Resampling method** (Método de nuevo muestreo), elija el método utilizado para crear los árboles individuales.  Puede elegir entre **Bagging** (Agregación) o **Replicate** (Replicación).

    - **Bagging** (agregación): la agregación también se denomina *agregación de arranque*. Cada árbol de un bosque de decisión de regresión da como resultado una predicción en forma de distribución gaussiana. La agregación consiste en encontrar una distribución gaussiana cuyos dos primeros momentos coincidan con los momentos de la mezcla de distribuciones gaussianas determinada combinando todas las distribuciones gaussianas devueltas por los árboles individuales.

         Para obtener más información, consulte la entrada de Wikipedia sobre la [agregación de arranque](https://wikipedia.org/wiki/Bootstrap_aggregating).

    - **Replicate** (replicación): en la replicación, cada árbol se entrena exactamente con los mismos datos de entrada. La determinación de qué predicado de división se utiliza para cada nodo de árbol sigue siendo aleatoria y los árboles serán diversos.

         Para obtener más información sobre el proceso de entrenamiento con la opción **Replicate** (Replicación), consulte [Bosques de decisión para Computer Vision y análisis de imágenes médicas. Criminisi y J. Shotton. Springer 2013. ](https://research.microsoft.com/projects/decisionforests/).

3. Especifique cómo quiere que se entrene el modelo, estableciendo la opción **Create trainer mode** (Crear modo entrenador).

    - **Single Parameter** (Parámetro único)

      Si sabe cómo quiere configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos. Es posible que haya obtenido estos valores mediante experimentación o que los haya recibido como guía.



4. Para **Number of decision trees** (Número de árboles de decisión), indique el número total de árboles de decisión que se creará en el conjunto. Si crea más árboles de decisión, puede obtener una cobertura potencialmente mejor, pero aumentará el tiempo de entrenamiento.

    > [!TIP]
    > Este valor también controla el número de árboles que se muestran al visualizar el modelo entrenado. Si desea ver o imprimir un único árbol, puede establecer el valor en 1; sin embargo, esto significa que solo se producirá un único árbol (el árbol con el conjunto inicial de parámetros) y que no se realizarán más iteraciones.

5. En **Maximum depth of the decision trees** (Profundidad máxima de los árboles de decisión), escriba un número para limitar la profundidad máxima de cualquier árbol de decisión. Al aumentar la profundidad del árbol podría aumentar la precisión, a riesgo de que se produzca un sobreajuste y aumente el tiempo de entrenamiento.

6. En **Number of random splits per node** (Número de divisiones aleatorias por nodo), escriba el número de divisiones que se usarán al crear cada nodo del árbol. Una *división* significa que las características de cada nivel del árbol (nodo) se dividen al azar.

7. En **Minimum number of samples per leaf node** (Número mínimo de muestras por nodo hoja), indique el número mínimo de casos que son necesarios para crear cualquier nodo terminal (hoja) en un árbol.

     Al aumentar este valor, aumenta el umbral para crear reglas nuevas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener cinco casos como mínimo que cumplan las mismas condiciones.


9. Conecte un conjunto de datos etiquetado, seleccione una única columna de etiquetas que no contenga más de dos resultados y conéctela con [Modelo de entrenamiento](./train-model.md).

    - Si establece la opción **Create trainer mode** (Crear modo de entrenador) en **Single Parameter** (Parámetro único), entrene el modelo usando el módulo [Entrenar modelo](./train-model.md).

   

10. Ejecute el experimento.

### <a name="results"></a>Results

Una vez completado el entrenamiento:

+ Para ver el árbol que se ha creado en cada iteración, haga clic con el botón derecho en el resultado del módulo de entrenamiento y seleccione **Visualizar**.

+ Para ver las reglas de cada nodo, haga clic en cada árbol y explore en profundidad las divisiones.

+ Para guardar una instantánea del modelo entrenado, haga clic con el botón derecho en el resultado del módulo de entrenamiento y seleccione **Save As Trained Model** (Guardar como modelo entrenado). Esta copia del modelo no se actualiza en ejecuciones sucesivas del experimento. 

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 