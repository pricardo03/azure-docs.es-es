---
title: 'Regresión: Predecir el precio y compare de algoritmos'
titleSuffix: Azure Machine Learning service
description: Este experimento de ejemplo de la interfaz visual muestra cómo comparar el rendimiento de dos modelos de regresión para predecir el precio de un automóvil. El proceso incluye aprendizaje, probar y evaluar el modelo en el conjunto de datos Automobile price data (Raw).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: 2a4a9e74fa7f56b67f0f4a64f6619db1c5c69a2c
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442137"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>Ejemplo 2: regresión: Predecir el precio y compare de algoritmos

Este experimento de ejemplo de la interfaz visual muestra cómo comparar el rendimiento de dos modelos de regresión para predecir el precio de un automóvil. El proceso incluye entrenar, probar y evaluar el modelo utilizando el **datos de precios de automóviles (Raw)** conjunto de datos.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seleccione el **abierto** botón para el experimento de ejemplo 2:

    ![Abra el experimento](media/ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="related-sample"></a>Ejemplo relacionado

[Ejemplo 1: regresión: Predicción de precios de automóviles (Basic)](ui-sample-regression-predict-automobile-price-basic.md) proporciona un experimento más sencillo que resuelve el mismo problema que este experimento, pero que usa un único modelo de regresión. Hacer referencia a él si desea obtener un ejemplo básico de regresión.

## <a name="experiment-summary"></a>Resumen del experimento

Estos pasos se usa para generar el experimento:

1. Obtener los datos.
1. Procesar previamente los datos.
1. Entrenar el modelo.
1. Probar, evaluar y comparar los modelos.

Este es el gráfico completo del experimento:

[![Gráfico del experimento](media/ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png#lightbox)


## <a name="get-the-data"></a>Obtener los datos

En este experimento, usamos el **datos de precios de automóviles (Raw)** conjunto de datos, que está en el repositorio de aprendizaje automático de UCI. Este conjunto de datos contiene 26 columnas que contienen información acerca de automóviles, incluida la marca, modelo, precio, características de vehículo (como el número de cilindros), MPG y una puntuación de riesgo de seguros. El objetivo de este experimento es predecir el precio de un automóvil.

## <a name="pre-process-the-data"></a>Procesar previamente los datos

Las tareas de preparación de datos principal incluyen la limpieza de datos, integración, transformación, reducción y la discretización o cuantificación. En la interfaz visual, puede encontrar módulos para realizar estas operaciones y otros datos de procesamiento previo de las tareas en el **transformación de datos** grupo en el panel izquierdo.

En este experimento, usamos el **Select Columns in Dataset** módulo para excluir normalized-losses que tienen muchos valores que faltan. A continuación, utilizamos **Clean Missing Data** para quitar las filas que tienen valores que faltan. Esto ayuda a crear un conjunto de datos de entrenamiento limpio.

![Procesamiento previo de los datos](media/ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Entrenamiento del modelo

Problemas de aprendizaje automático varían. Tareas de aprendizaje automático comunes incluyen la clasificación, agrupación en clústeres, regresión y los sistemas de recomendación, cada uno de los cuales puede requerir un algoritmo diferente. La elección del algoritmo a menudo depende de los requisitos del caso de uso. Después de elegir un algoritmo, deberá ajustar sus parámetros para entrenar un modelo más preciso. A continuación, deberá evaluar todos los modelos basados en métricas, como la precisión, inteligibilidad y eficacia.

Dado que el objetivo de este experimento es predecir precios de automóviles y porque la columna de etiqueta (precio) contiene números reales, un modelo de regresión es una buena elección. Teniendo en cuenta que el número de características es relativamente pequeño (inferior a 100) y estas características no están dispersas, el límite de decisión es probable que sea no lineal.

Para comparar el rendimiento de algoritmos diferentes, usamos dos algoritmos no lineales, **regresión de árbol de decisión impulsado** y **regresión de bosque de decisión**, para generar modelos. Ambos algoritmos tienen parámetros que se pueden cambiar, pero se usa los valores predeterminados para este experimento.

Usamos el **dividir datos** módulo dividir aleatoriamente los datos de entrada para que el conjunto de datos de entrenamiento contiene un 70% de los datos originales y el conjunto de datos de prueba contiene un 30% de los datos originales.

## <a name="test-evaluate-and-compare-the-models"></a>Probar, evaluar y comparar los modelos

Se usan dos conjuntos diferentes de datos elegidos al azar para entrenar y probar el modelo, como se describe en la sección anterior. Se divide el conjunto de datos y utilizan diferentes conjuntos de datos para entrenar y probar el modelo para realizar la evaluación del modelo más objetivas.

Después de que el modelo se entrena, usamos el **Score Model** y **Evaluate Model** módulos para generar los resultados de predicción y evaluar los modelos. **Puntuar modelo** genera predicciones del conjunto de datos de prueba mediante el modelo entrenado. A continuación, pasamos las puntuaciones para **Evaluate Model** para generar métricas de evaluación.

En este experimento, usamos dos instancias de **Evaluate Model** para comparar dos pares de modelos.

En primer lugar, comparamos dos algoritmos en el conjunto de datos de entrenamiento.
En segundo lugar, comparamos dos algoritmos en el conjunto de datos de prueba.
Estos son los resultados:

![Compare los resultados](media/ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Estos resultados muestran que el modelo creado con **regresión de árbol de decisión impulsado** tiene una raíz inferior error cuadrático medio que el modelo basado en **regresión de bosque de decisión**.

Ambos algoritmos tienen un error inferior en el conjunto de datos de entrenamiento en el conjunto de datos de prueba no visto.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore los ejemplos disponibles para la interfaz visual:

- [Ejemplo 1: regresión: Predecir el precio de un automóvil](ui-sample-regression-predict-automobile-price-basic.md)
- [Ejemplo 3: clasificación: Predecir el riesgo de crédito](ui-sample-classification-predict-credit-risk-basic.md)
- [Ejemplo 4: clasificación: Predecir el riesgo de crédito (costo confidencial)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Ejemplo 5: clasificación: Predecir el abandono](ui-sample-classification-predict-churn.md)
