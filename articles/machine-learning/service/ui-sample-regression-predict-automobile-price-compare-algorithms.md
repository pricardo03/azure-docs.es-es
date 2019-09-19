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
ms.openlocfilehash: 2c45ccf55b1adde7150dee17fd562b24ee4777ba
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997097"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>Ejemplo 2 - Regresión: predicción del precio y comparación de algoritmos

Aprenda a compilar un experimento de aprendizaje automático sin necesidad de escribir una sola línea de código mediante la interfaz visual. En este ejemplo se entrenan y se comparan varios modelos de regresión para predecir el precio de un automóvil en función de sus características técnicas. Le mostraremos la justificación de las opciones elegidas en este experimento de manera que pueda superar los problemas de aprendizaje automático que se le presenten.

Si está empezando con el aprendizaje automático, puede echar un vistazo a la [versión básica](ui-sample-regression-predict-automobile-price-basic.md) del experimento de regresión.

A continuación se muestra el gráfico completo del experimento:

[![Gráfico del experimento](media/ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seleccione el botón **Abrir** correspondiente al experimento Ejemplo 2:

    ![Abrir el experimento](media/ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="experiment-summary"></a>Resumen del experimento

Para compilar el experimento, seguimos estos pasos:

1. Obtenga los datos.
1. Preprocese los datos.
1. Entrene el modelo.
1. Pruebe, evalúe y compare los modelos.

## <a name="get-the-data"></a>Obtener los datos

En este experimento usamos el conjunto de datos **Automobile price data (Raw)** del repositorio de UCI de Machine Learning. Este conjunto de datos contiene 26 columnas con información acerca de automóviles, incluida la marca, el modelo, el precio, las características (como la cilindrada), el consumo de combustible y una puntuación de riesgo para el seguro. El objetivo de este experimento es predecir el precio de un automóvil.

## <a name="pre-process-the-data"></a>Preprocesar los datos

Las tareas de preparación principales incluyen la limpieza, la integración, la transformación,la reducción y la discretización o cuantificación de los datos. En la interfaz visual puede encontrar módulos para realizar estas operaciones y otras tareas de preprocesamiento de los datos en el grupo **Data Transformation** (Transformación de datos) del panel izquierdo.

En este experimento usamos el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) para excluir las pérdidas normalizadas donde falten muchos valores. A continuación, utilizamos **Clean Missing Data** (Eliminar datos que faltan) para quitar las filas donde falten valores. Esto ayuda a crear un conjunto de datos de entrenamiento limpio.

![Procesamiento previo de los datos](media/ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Entrenamiento del modelo

Los problemas del aprendizaje automático son varios. Las tareas de aprendizaje automático habituales incluyen la clasificación, la agrupación en clústeres, la regresión y los sistemas de recomendación, que pueden requerir algoritmos diferentes. La elección del algoritmo a menudo depende de los requisitos del caso de uso. Después de elegir un algoritmo deberá ajustar sus parámetros para entrenar un modelo más preciso. A continuación, deberá evaluar todos los modelos en función de unas métricas, como la precisión, la inteligibilidad y la eficacia.

Dado que el objetivo de este experimento es predecir precios de automóviles y la columna de etiqueta (precio) contiene números reales, un modelo de regresión es una buena elección. Teniendo en cuenta que el número de características es relativamente pequeño (menor que 100) y que estas características no están dispersas, el límite de decisión es probable que sea no lineal.

Para comparar el rendimiento de algoritmos diferentes, para compilar modelos usamos dos algoritmos no lineales, **Boosted Decision Tree Regression** (Regresión de árbol de decisión incrementado) y **Decision Forest Regression** (Regresión de bosque de decisión). Ambos algoritmos tienen parámetros que se pueden cambiar, pero usamos los valores predeterminados para este experimento.

Usamos el módulo **Split Data** (Dividir datos) para dividir aleatoriamente los datos de entrada para que el conjunto de datos de entrenamiento contenga el 70 % de los datos originales y el de prueba, el 30 %.

## <a name="test-evaluate-and-compare-the-models"></a>Probar, evaluar y comparar los modelos

Como se describe en la sección anterior, usamos dos conjuntos de datos diferentes elegidos aleatoriamente y después probamos el modelo. Dividimos el conjunto de datos y usamos conjuntos de datos diferentes para entrenar y probar el modelo para que la evaluación sea más objetiva.

Después de haber entrenado el modelo, usamos los módulos **Score Model** (Puntuar modelo) y **Evaluate Model** (Evaluar modelo) para generar los resultados de predicción y evaluar los modelos. **Score Model** (Puntuar modelo) genera predicciones del conjunto de datos de prueba mediante el modelo entrenado. A continuación, pasamos las puntuaciones a **Evaluate Model** (Evaluar modelo) para generar métricas de evaluación.

En este experimento, usamos dos instancias de **Evaluate Model** (Evaluar modelo) para comparar dos pares de modelos.

En primer lugar, comparamos dos algoritmos en el conjunto de datos de entrenamiento.
En segundo lugar, comparamos dos algoritmos en el conjunto de datos de prueba.
Estos son los resultados:

![Comparación de los resultados](media/ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Estos resultados muestran que el modelo compilado con **Boosted Decision Tree Regression** (Regresión de árbol de decisión incrementado) tiene la raíz del error cuadrático medio menor que el modelo compilado con **Decision Forest Regression** (Regresión de bosque de decisión).

Ambos algoritmos tienen un error inferior en el conjunto de datos de entrenamiento que en el conjunto de datos de prueba invisible.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para la interfaz visual:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](ui-sample-regression-predict-automobile-price-basic.md)
- [Ejemplo 3 - Clasificación: predicción del riesgo crediticio](ui-sample-classification-predict-credit-risk-basic.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](ui-sample-classification-predict-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](ui-sample-classification-predict-flight-delay.md)
