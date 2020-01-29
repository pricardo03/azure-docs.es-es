---
title: 'Diseñador: ejemplo de predicción de precios de automóviles (avanzado)'
titleSuffix: Azure Machine Learning
description: Cree y compare varios modelos de regresión de Machine Learning para predecir el precio de un automóvil en función de las características técnicas con el diseñador de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.date: 12/25/2019
ms.openlocfilehash: b2511feefbe419223f355509ecb435518ce8382e
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76311060"
---
# <a name="train--compare-multiple-regression-models-to-predict-car-prices-with-azure-machine-learning-designer"></a>Entrenamiento y comparación de varios modelos de regresión para predecir los precios de automóviles con el diseñador de Azure Machine Learning

**Ejemplo de diseñador 2**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Aprenda a compilar una canalización de aprendizaje automático sin tener que escribir una sola línea de código con el diseñador. En este ejemplo se entrenan y se comparan varios modelos de regresión para predecir el precio de un automóvil en función de sus características técnicas. Le mostraremos la justificación de las opciones elegidas en esta canalización de manera que pueda superar los problemas de aprendizaje automático que se le presenten.

Si acaba de empezar a trabajar con el aprendizaje automático, eche un vistazo a la [versión básica](how-to-designer-sample-regression-automobile-price-basic.md) de esta canalización.

Este es el gráfico completo de la canalización:

[![Gráfico de la canalización](./media/how-to-designer-sample-regression-automobile-price-compare-algorithms/graph.png)](./media/how-to-designer-sample-regression-automobile-price-compare-algorithms/graph.png#lightbox)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Haga clic en el ejemplo 2 para abrirlo. 

## <a name="pipeline-summary"></a>Resumen de la canalización

Siga los pasos que se indican a continuación para crear la canalización de aprendizaje automático:

1. Obtenga los datos.
1. Preprocese los datos.
1. Entrene el modelo.
1. Pruebe, evalúe y compare los modelos.

## <a name="get-the-data"></a>Obtener los datos

En este experimento se usa el conjunto de datos **Automobile price data (Raw)** del repositorio de UCI de Machine Learning. Este conjunto de datos contiene 26 columnas con información acerca de automóviles, incluida la marca, el modelo, el precio, las características (como la cilindrada), el consumo de combustible y una puntuación de riesgo para el seguro.

## <a name="pre-process-the-data"></a>Preprocesar los datos

Las tareas de preparación principales incluyen la limpieza, la integración, la transformación,la reducción y la discretización o cuantificación de los datos. En el diseñador, puede buscar módulos para realizar estas operaciones y otras tareas de procesamiento previo de datos en el grupo **Transformación de datos** del panel izquierdo.

Use el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) para excluir las pérdidas normalizadas donde falten muchos valores. A continuación, utilizamos **Clean Missing Data** (Eliminar datos que faltan) para quitar las filas donde falten valores. Esto ayuda a crear un conjunto de datos de entrenamiento limpio.

![Procesamiento previo de los datos](./media/how-to-designer-sample-regression-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Entrenamiento del modelo

Los problemas del aprendizaje automático son varios. Las tareas de aprendizaje automático habituales incluyen la clasificación, la agrupación en clústeres, la regresión y los sistemas de recomendación, que pueden requerir algoritmos diferentes. La elección del algoritmo a menudo depende de los requisitos del caso de uso. Después de elegir un algoritmo deberá ajustar sus parámetros para entrenar un modelo más preciso. A continuación, deberá evaluar todos los modelos en función de unas métricas, como la precisión, la inteligibilidad y la eficacia.

Dado que el objetivo de esta canalización es predecir precios de automóviles y la columna de etiqueta (precio) contiene números reales, un modelo de regresión es una buena elección. Teniendo en cuenta que el número de características es relativamente pequeño (menor que 100) y que estas características no están dispersas, el límite de decisión es probable que sea no lineal.

Para comparar el rendimiento de algoritmos diferentes, para compilar modelos usamos dos algoritmos no lineales, **Boosted Decision Tree Regression** (Regresión de árbol de decisión incrementado) y **Decision Forest Regression** (Regresión de bosque de decisión). Ambos algoritmos tienen parámetros que se pueden cambiar, pero se usan los valores predeterminados en esta canalización.

Use el módulo **Split Data** (Dividir datos) para dividir aleatoriamente los datos de entrada de forma que el conjunto de datos de entrenamiento contenga el 70 % de los datos originales y el de prueba el 30 %.

## <a name="test-evaluate-and-compare-the-models"></a>Probar, evaluar y comparar los modelos

Como se describe en la sección anterior, se usan dos conjuntos de datos diferentes elegidos aleatoriamente para entrenar y probar el modelo. Divida el conjunto de datos y use conjuntos de datos diferentes para entrenar y probar el modelo para que la evaluación sea más objetiva.

Después de entrenar el modelo, se usan los módulos **Score Model** (Puntuar modelo) y **Evaluate Model** (Evaluar modelo) para generar los resultados de predicción y evaluar los modelos. **Score Model** (Puntuar modelo) genera predicciones del conjunto de datos de prueba mediante el modelo entrenado. Luego, se pasan las puntuaciones a **Evaluate Model** (Evaluar modelo) para generar métricas de evaluación.



He aquí los resultados:

![Comparación de los resultados](./media/how-to-designer-sample-regression-automobile-price-compare-algorithms/result.png)

Estos resultados muestran que el modelo compilado con **Boosted Decision Tree Regression** (Regresión de árbol de decisión incrementado) tiene la raíz del error cuadrático medio menor que el modelo compilado con **Decision Forest Regression** (Regresión de bosque de decisión).



## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para el diseñador:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-designer-sample-regression-automobile-price-basic.md)
- [Ejemplo 3: clasificación con selección de características: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-designer-sample-classification-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-designer-sample-classification-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: Conjunto de datos de SP 500 de Wikipedia](how-to-designer-sample-text-classification.md)
