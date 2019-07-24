---
title: 'Regresión: Predicción del precio'
titleSuffix: Azure Machine Learning service
description: Aprenda a compilar un modelo de Machine Learning para predecir el precio de un automóvil sin necesidad de escribir una sola línea de código.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: ddc7ae10581075127f72dd020c59cf28bbfc9ae2
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606135"
---
# <a name="sample-1---regression-predict-price"></a>Ejemplo 1 - Regresión: Predicción del precio

Aprenda a compilar un modelo de regresión de Machine Learning sin necesidad de escribir una sola línea de código mediante la interfaz visual.

Este experimento entrena un **regresor de bosque de decisión** para predecir el precio de un automóvil en función de sus características técnicas, como la marca, el modelo, los caballos de potencia y el tamaño. Como estamos intentando responder a la pregunta "¿cuánto?", esto se denomina un problema de regresión. Sin embargo, puede aplicar los mismos pasos fundamentales para abordar cualquier tipo de problema de aprendizaje automático, ya sea una regresión, una clasificación, una agrupación en clústeres, etc.

Los pasos fundamentales de un modelo de Machine Learning son:

1. Obtener los datos
1. Preprocesar los datos
1. Entrenamiento del modelo
1. Evaluación del modelo

Este es el gráfico final y completo del experimento en el que vamos a trabajar. Le presentaremos el razonamiento de todos los módulos para que pueda tomar sus propias decisiones.

![Gráfico del experimento](media/ui-sample-regression-predict-automobile-price-basic/overall-graph.png)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seleccione el botón **Abrir** correspondiente al experimento Ejemplo 1:

    ![Abrir el experimento](media/ui-sample-regression-predict-automobile-price-basic/open-sample1.png)

## <a name="get-the-data"></a>Obtener los datos

En este experimento usamos el conjunto de datos **Automobile price data (Raw)** del repositorio de UCI de Machine Learning. Este conjunto de datos contiene 26 columnas con información acerca de automóviles, incluida la marca, el modelo, el precio, las características (como la cilindrada), el consumo de combustible y una puntuación de riesgo para el seguro. El objetivo de este experimento es predecir el precio del automóvil.

## <a name="pre-process-the-data"></a>Preprocesar los datos

Las tareas de preparación principales incluyen la limpieza, la integración, la transformación,la reducción y la discretización o cuantificación de los datos. En la interfaz visual puede encontrar módulos para realizar estas operaciones y otras tareas de preprocesamiento de los datos en el grupo **Data Transformation** (Transformación de datos) del panel izquierdo.

Usaremos el módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos) para excluir las pérdidas normalizadas donde falten muchos valores. A continuación, utilizamos **Clean Missing Data** (Eliminar datos que faltan) para quitar las filas donde falten valores. Esto ayuda a crear un conjunto de datos de entrenamiento limpio.

![Procesamiento previo de los datos](./media/ui-sample-regression-predict-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>Entrenamiento del modelo

Los problemas del aprendizaje automático son varios. Las tareas de aprendizaje automático habituales incluyen la clasificación, la agrupación en clústeres, la regresión y los sistemas de recomendación, que pueden requerir algoritmos diferentes. La elección del algoritmo a menudo depende de los requisitos del caso de uso. Después de elegir un algoritmo deberá ajustar sus parámetros para entrenar un modelo más preciso. A continuación, deberá evaluar todos los modelos en función de unas métricas, como la precisión, la inteligibilidad y la eficacia.

Dado que el objetivo de este experimento es predecir precios de automóviles y la columna de etiqueta (precio) contiene números reales, un modelo de regresión es una buena elección. Teniendo en cuenta que el número de características es relativamente pequeño (menor que 100) y que estas características no están dispersas, el límite de decisión es probable que sea no lineal. Por lo tanto, en este experimento usaremos la **regresión de bosque de decisión**.

Usamos el módulo **Split Data** (Dividir datos) para dividir aleatoriamente los datos de entrada para que el conjunto de datos de entrenamiento contenga el 70 % de los datos originales y el de prueba, el 30 %.

## <a name="test-evaluate-and-compare"></a>Prueba, evaluación y comparación

 Dividimos el conjunto de datos y usamos conjuntos de datos diferentes para entrenar y probar el modelo para que la evaluación sea más objetiva.

Después de haber entrenado el modelo, usamos los módulos **Score Model** (Puntuar modelo) y **Evaluate Model** (Evaluar modelo) para generar los resultados de predicción y evaluar los modelos.

**Score Model** (Puntuar modelo) genera predicciones del conjunto de datos de prueba mediante el modelo entrenado. Para comprobar el resultado, seleccione el puerto de salida de **Score Model** (Puntuar modelo) y **Visualize** (Visualizar).

![Resultado de la puntuación](./media/ui-sample-regression-predict-automobile-price-basic/score-result.png)

A continuación, pasamos las puntuaciones al módulo **Evaluate Model** (Evaluar modelo) para generar métricas de evaluación. Para comprobar el resultado, seleccione el puerto de salida de **Evaluate Model** (Evaluar modelo) y **Visualize** (Visualizar).

![Evaluación del resultado](./media/ui-sample-regression-predict-automobile-price-basic/evaluate-result.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para la interfaz visual:

- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Ejemplo 3 - Clasificación: predicción del riesgo crediticio](ui-sample-classification-predict-credit-risk-basic.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](ui-sample-classification-predict-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](ui-sample-classification-predict-flight-delay.md)