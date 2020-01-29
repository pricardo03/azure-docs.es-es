---
title: 'Diseñador: Ejemplo de predicción de precios de automóviles (básico)'
titleSuffix: Azure Machine Learning
description: Cree un modelo de regresión de aprendizaje automático para predecir el precio de un automóvil sin escribir una sola línea de código con el diseñador de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.date: 12/25/2019
ms.openlocfilehash: 18a9f9d49dce96b037f10a95a458e52d020fe6c7
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76311077"
---
# <a name="use-regression-to-predict-car-prices-with-azure-machine-learning-designer"></a>Uso de la regresión para predecir los precios de los automóviles con el diseñador de Azure Machine Learning

**Ejemplo de diseñador 1**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Obtenga información sobre cómo compilar un modelo de regresión de aprendizaje automático sin tener que escribir una sola línea de código mediante el diseñador.

Esta canalización entrena un **regresor lineal** para predecir el precio de un automóvil en función de las características técnicas, como la marca, el modelo, la potencia y el tamaño. Como estamos intentando responder a la pregunta "¿cuánto?", esto se denomina un problema de regresión. Sin embargo, puede aplicar los mismos pasos fundamentales de este ejemplo para abordar cualquier tipo de problema de aprendizaje automático, ya sea una regresión, una clasificación, una agrupación en clústeres, etc.

Los pasos fundamentales de un modelo de Machine Learning son:

1. Obtener los datos
1. Preprocesar los datos
1. Entrenamiento del modelo
1. Evaluación del modelo

Este es el gráfico final completo de la canalización. Le presentaremos el razonamiento de todos los módulos para que pueda tomar sus propias decisiones.

![Gráfico de la canalización](./media/how-to-designer-sample-regression-automobile-price-basic/overall-graph.png)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Haga clic en el ejemplo 1 para abrirlo.


## <a name="get-the-data"></a>Obtener los datos

En este experimento se usa el conjunto de datos **Automobile price data (Raw)** del repositorio de UCI de Machine Learning. Este conjunto de datos contiene 26 columnas con información acerca de automóviles, incluida la marca, el modelo, el precio, las características (como la cilindrada), el consumo de combustible y una puntuación de riesgo para el seguro. El objetivo de este ejemplo es predecir el precio del automóvil.

## <a name="pre-process-the-data"></a>Preprocesar los datos

Las tareas de preparación principales incluyen la limpieza, la integración, la transformación,la reducción y la discretización o cuantificación de los datos. En el diseñador, puede buscar módulos para realizar estas operaciones y otras tareas de procesamiento previo de datos en el grupo **Transformación de datos** del panel izquierdo.

Use el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) para excluir las pérdidas normalizadas donde falten muchos valores. A continuación, use **Clean Missing Data** (Eliminar datos que faltan) para quitar las filas donde falten valores. Esto ayuda a crear un conjunto de datos de entrenamiento limpio.

![Procesamiento previo de los datos](./media/how-to-designer-sample-regression-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>Entrenamiento del modelo

Los problemas del aprendizaje automático son varios. Las tareas de aprendizaje automático habituales incluyen la clasificación, la agrupación en clústeres, la regresión y los sistemas de recomendación, que pueden requerir algoritmos diferentes. La elección del algoritmo a menudo depende de los requisitos del caso de uso. Después de elegir un algoritmo deberá ajustar sus parámetros para entrenar un modelo más preciso. A continuación, deberá evaluar todos los modelos en función de unas métricas, como la precisión, la inteligibilidad y la eficacia.

Dado que el objetivo de este ejemplo es predecir precios de automóviles y la columna de etiqueta (precio) contiene números reales, un modelo de regresión es una buena elección. Teniendo en cuenta que el número de características es relativamente pequeño (menor que 100) y que estas características no están dispersas, el límite de decisión es probable que sea no lineal. Por lo tanto, en esta canalización usaremos **Regresión de bosque de decisión**.

Use el módulo **Split Data** (Dividir datos) para dividir aleatoriamente los datos de entrada de forma que el conjunto de datos de entrenamiento contenga el 70 % de los datos originales y el de prueba el 30 %.

## <a name="test-evaluate-and-compare"></a>Prueba, evaluación y comparación

Divida el conjunto de datos y use conjuntos de datos diferentes para entrenar y probar el modelo para que la evaluación sea más objetiva.

Después de haber entrenado el modelo, puede usar los módulos **Score Model** (Puntuar modelo) y **Evaluate Model** (Evaluar modelo) para generar los resultados de predicción y evaluar los modelos.

**Score Model** (Puntuar modelo) genera predicciones del conjunto de datos de prueba mediante el modelo entrenado. Para comprobar el resultado, seleccione el puerto de salida de **Score Model** (Puntuar modelo) y **Visualize** (Visualizar).

![Resultado de la puntuación](./media/how-to-designer-sample-regression-automobile-price-basic/sample1-score-1225.png)

Pase las puntuaciones al módulo **Evaluate Model** (Evaluar modelo) para generar métricas de evaluación. Para comprobar el resultado, seleccione el puerto de salida de **Evaluate Model** (Evaluar modelo) y **Visualize** (Visualizar).

![Evaluación del resultado](./media/how-to-designer-sample-regression-automobile-price-basic/sample1-evaluate-1225.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para el diseñador:

- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Ejemplo 3: clasificación con selección de características: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-designer-sample-classification-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-designer-sample-classification-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: Conjunto de datos de SP 500 de Wikipedia](how-to-designer-sample-text-classification.md)