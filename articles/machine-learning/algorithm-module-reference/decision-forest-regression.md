---
title: 'Regresión de bosque de decisión: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de regresión de bosque de decisión en el servicio Azure Machine Learning para crear un modelo de regresión basado en un conjunto de árboles de decisión.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: d372adf75d46fdedb7a6f2b17e47822475d1f155
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442366"
---
# <a name="decision-forest-regression-module"></a>Módulo de regresión de bosque de decisión

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de regresión basado en un conjunto de árboles de decisión.

Después de haber configurado el modelo, debe entrenar el modelo mediante un conjunto de datos etiquetado y la [entrenar modelo](./train-model.md) módulo.  El modelo entrenado, a continuación, se puede usar para realizar predicciones. 

## <a name="how-it-works"></a>Funcionamiento

Árboles de decisión son modelos no paramétricos que llevan a cabo una secuencia de pruebas simples para cada instancia, atravesando una estructura de datos de árbol binario hasta alcanza un nodo hoja (decisión).

Árboles de decisión tienen las siguientes ventajas:

- Son eficientes en los cálculos y uso de memoria durante el entrenamiento y predicción.

- Pueden representar límites de decisión no lineales.

- Se realizan la clasificación y selección de características integradas y son resistentes en presencia de características ruidosas.

Este modelo de regresión consta de un conjunto de árboles de decisión. Cada árbol en un bosque de decisión de regresión da como resultado una distribución gaussiana como una predicción. Se realiza una agregación sobre el conjunto de árboles para buscar una distribución gaussiana más cercana a la distribución combinada para todos los árboles del modelo.

Para obtener más información sobre el marco de trabajo teórico para este algoritmo y su implementación, consulte este artículo: [Los bosques de decisión: Un marco unificado de clasificación, regresión, la estimación de densidad, colector aprendizaje y el aprendizaje supervisado parcialmente](https://www.microsoft.com/en-us/research/publication/decision-forests-a-unified-framework-for-classification-regression-density-estimation-manifold-learning-and-semi-supervised-learning/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D158806#)

## <a name="how-to-configure-decision-forest-regression-model"></a>Cómo configurar el modelo de regresión de bosque de decisión

1. Agregar el **regresión de bosque de decisión** módulo en el experimento. Puede encontrar el módulo en la interfaz en **Machine Learning**, **Initialize Model**, y **regresión**.

2. Abra las propiedades del módulo y para **volver a muestrear método**, elija el método utilizado para crear los árboles individuales.  Puede elegir entre **Bagging** o **replicar**.

    - **Bagging**: También se denomina ensacado *agregación bootstrap*. Cada árbol en un bosque de decisión de regresión da como resultado una distribución gaussiana por medio de predicción. La agregación es encontrar un gaussiano cuyos dos primeros minutos coinciden con los momentos de la mezcla de Gaussians dada mediante la combinación de todos los Gaussians devueltos por árboles individuales.

         Para obtener más información, consulte la entrada de Wikipedia [agregación Bootstrap](https://wikipedia.org/wiki/Bootstrap_aggregating).

    - **Replicar**: En la replicación, cada árbol se entrena con exactamente los mismos datos de entrada. La determinación de qué división predicado se utiliza para cada nodo de árbol permanece aleatoria y los árboles será diversos.

         Para obtener más información sobre el proceso de entrenamiento con el **replicar** opción, vea [bosques de decisión para Computer Vision y análisis de imágenes médicas. Criminisi y J. Shotton. Springer 2013. ](https://research.microsoft.com/projects/decisionforests/).

3. Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.

    - **Parámetro único**

      Si sabe cómo desea configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos. Es posible que haya aprendido estos valores mediante experimentación o recibido como guía.



4. Para **número de árboles de decisión**, indique el número total de árboles de decisión para crear en el conjunto. Si crea más árboles de decisión, puede obtener una mejor cobertura, pero aumentará el tiempo de entrenamiento.

    > [!TIP]
    > Este valor también controla el número de árboles que se muestra cuando se visualice el modelo entrenado. Si desea ver o imprimir un único árbol, puede establecer el valor en 1; Sin embargo, esto significa que un único árbol serán generados (el árbol con el conjunto inicial de parámetros) y no se realizará ninguna las iteraciones adicionales.

5. Para **profundidad máxima de los árboles de decisión**, escriba un número para limitar la profundidad máxima de cualquier árbol de decisión. Al aumentar la profundidad del árbol podría aumentar la precisión, con un tiempo de entrenamiento de sobreajuste y aumente el riesgo.

6. Para **número de divisiones aleatorias por nodo**, escriba el número de divisiones que se usará al crear cada nodo del árbol. Un *dividir* significa que las características de cada nivel del árbol (nodo) se divide al azar.

7. Para **número mínimo de muestras por nodo hoja**, indicar el número mínimo de casos que son necesarios para crear cualquier nodo terminal (hoja) en un árbol.

     Al aumentar este valor, aumente el umbral para la creación de nuevas reglas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener al menos cinco casos que cumplen las condiciones.


9. Conecte un conjunto de datos con la etiqueta, seleccione una columna de una sola etiqueta con no más de dos resultados y conectarse a [Train Model](./train-model.md).

    - Si establece **crear modo de entrenador** opción **único parámetro**, entrenar el modelo usando el [entrenar modelo](./train-model.md) módulo.

   

10. Ejecute el experimento.

### <a name="results"></a>Resultados

Una vez completada la formación:

+ Para ver el árbol de la que se creó en cada iteración, haga clic en la salida del módulo de entrenamiento y seleccione **visualizar**.

+ Para ver las reglas para cada nodo, haga clic en cada árbol y explorar en profundidad las divisiones.

+ Para guardar una instantánea del modelo entrenado, haga clic en la salida del módulo de entrenamiento y seleccione **Guardar como modelo entrenado**. Esta copia del modelo no se actualiza en ejecuciones sucesivas del experimento. 

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 