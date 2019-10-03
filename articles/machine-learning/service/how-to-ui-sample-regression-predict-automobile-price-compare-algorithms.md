---
title: 'Ejemplo de interfaz visual n.° 3: regresión de precios y comparación de algoritmos'
titleSuffix: Azure Machine Learning
description: En este artículo se muestra cómo compilar un experimento de aprendizaje automático complejo sin necesidad de escribir una sola línea de código mediante la interfaz visual. Aprenda a entrenar y comparar varios modelos de regresión para predecir el precio de un automóvil en función de sus características técnicas
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: c40d76b87ca7437e25c567176b0309f08f3ca9f2
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71131135"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>Ejemplo 2 - Regresión: predicción del precio y comparación de algoritmos

Aprenda a compilar un experimento de aprendizaje automático sin necesidad de escribir una sola línea de código mediante la interfaz visual. En este ejemplo se entrenan y se comparan varios modelos de regresión para predecir el precio de un automóvil en función de sus características técnicas. Le mostraremos la justificación de las opciones elegidas en este experimento de manera que pueda superar los problemas de aprendizaje automático que se le presenten.

Si acaba de empezar a trabajar con el aprendizaje automático, eche un vistazo a la [versión básica](how-to-ui-sample-regression-predict-automobile-price-basic.md) de este experimento.

A continuación se muestra el gráfico completo del experimento:

[![Gráfico del experimento](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seleccione el botón **Abrir** correspondiente al experimento Ejemplo 2:

    ![Abrir el experimento](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="experiment-summary"></a>Resumen del experimento

Siga los pasos que se indican a continuación para crear el experimento de aprendizaje automático:

1. Obtenga los datos.
1. Preprocese los datos.
1. Entrene el modelo.
1. Pruebe, evalúe y compare los modelos.

## <a name="get-the-data"></a>Obtener los datos

En este experimento se usa el conjunto de datos **Automobile price data (Raw)** del repositorio de UCI de Machine Learning. Este conjunto de datos contiene 26 columnas con información acerca de automóviles, incluida la marca, el modelo, el precio, las características (como la cilindrada), el consumo de combustible y una puntuación de riesgo para el seguro.

## <a name="pre-process-the-data"></a>Preprocesar los datos

Las tareas de preparación principales incluyen la limpieza, la integración, la transformación,la reducción y la discretización o cuantificación de los datos. En la interfaz visual puede encontrar módulos para realizar estas operaciones y otras tareas de preprocesamiento de los datos en el grupo **Data Transformation** (Transformación de datos) del panel izquierdo.

Use el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) para excluir las pérdidas normalizadas donde falten muchos valores. A continuación, utilizamos **Clean Missing Data** (Eliminar datos que faltan) para quitar las filas donde falten valores. Esto ayuda a crear un conjunto de datos de entrenamiento limpio.

![Procesamiento previo de los datos](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Entrenamiento del modelo

Los problemas del aprendizaje automático son varios. Las tareas de aprendizaje automático habituales incluyen la clasificación, la agrupación en clústeres, la regresión y los sistemas de recomendación, que pueden requerir algoritmos diferentes. La elección del algoritmo a menudo depende de los requisitos del caso de uso. Después de elegir un algoritmo deberá ajustar sus parámetros para entrenar un modelo más preciso. A continuación, deberá evaluar todos los modelos en función de unas métricas, como la precisión, la inteligibilidad y la eficacia.

Dado que el objetivo de este experimento es predecir precios de automóviles y la columna de etiqueta (precio) contiene números reales, un modelo de regresión es una buena elección. Teniendo en cuenta que el número de características es relativamente pequeño (menor que 100) y que estas características no están dispersas, el límite de decisión es probable que sea no lineal.

Dado que el objetivo de este experimento es predecir precios de automóviles y la columna de etiqueta (precio) contiene números reales, un modelo de regresión es una buena elección. Teniendo en cuenta que el número de características es relativamente pequeño (menor que 100) y que estas características no están dispersas, el límite de decisión es probable que sea no lineal.

Para comparar el rendimiento de algoritmos diferentes, para compilar modelos usamos dos algoritmos no lineales, **Boosted Decision Tree Regression** (Regresión de árbol de decisión incrementado) y **Decision Forest Regression** (Regresión de bosque de decisión). Ambos algoritmos tienen parámetros que se pueden cambiar, pero se usan los valores predeterminados en este experimento.

Use el módulo **Split Data** (Dividir datos) para dividir aleatoriamente los datos de entrada de forma que el conjunto de datos de entrenamiento contenga el 70 % de los datos originales y el de prueba el 30 %.

## <a name="test-evaluate-and-compare-the-models"></a>Probar, evaluar y comparar los modelos

Como se describe en la sección anterior, se usan dos conjuntos de datos diferentes elegidos aleatoriamente para entrenar y probar el modelo. Divida el conjunto de datos y use conjuntos de datos diferentes para entrenar y probar el modelo para que la evaluación sea más objetiva.

Después de entrenar el modelo, se usan los módulos **Score Model** (Puntuar modelo) y **Evaluate Model** (Evaluar modelo) para generar los resultados de predicción y evaluar los modelos. **Score Model** (Puntuar modelo) genera predicciones del conjunto de datos de prueba mediante el modelo entrenado. Luego, se pasan las puntuaciones a **Evaluate Model** (Evaluar modelo) para generar métricas de evaluación.

En este experimento, se usan dos instancias de **Evaluate Model** (Evaluar modelo) para comparar dos pares de modelos.

En primer lugar, comparamos dos algoritmos en el conjunto de datos de entrenamiento.
En segundo lugar, comparamos dos algoritmos en el conjunto de datos de prueba.

Estos son los resultados:

![Comparación de los resultados](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Estos resultados muestran que el modelo compilado con **Boosted Decision Tree Regression** (Regresión de árbol de decisión incrementado) tiene la raíz del error cuadrático medio menor que el modelo compilado con **Decision Forest Regression** (Regresión de bosque de decisión).

Ambos algoritmos tienen un error inferior en el conjunto de datos de entrenamiento que en el conjunto de datos de prueba invisible.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para la interfaz visual:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Ejemplo 3 - Clasificación: predicción del riesgo crediticio](how-to-ui-sample-classification-predict-credit-risk-basic.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-ui-sample-classification-predict-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-ui-sample-classification-predict-flight-delay.md)
