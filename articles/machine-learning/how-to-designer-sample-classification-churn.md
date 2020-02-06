---
title: 'Diseñador: Ejemplo de predicción de la renovación'
titleSuffix: Azure Machine Learning
description: Siga este ejemplo de clasificación para predecir la renovación con el diseñador de Azure Machine Learning y árboles de decisión ampliados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: keli19
ms.reviewer: sgilley
ms.date: 12/25/2019
ms.openlocfilehash: 701bf186080ef627ef4621e959b281fd58aa4132
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963380"
---
# <a name="use-boosted-decision-tree-to-predict-churn-with-azure-machine-learning-designer"></a>Uso de un árbol de decisión ampliado para predecir la renovación con el diseñador de Azure Machine Learning

**Ejemplo 5 del diseñador (versión preliminar)**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Obtenga información sobre cómo compilar una canalización compleja de aprendizaje automático sin tener que escribir una sola línea de código con el diseñador (versión preliminar).

Esta canalización entrena 2 clasificadores de **árbol de decisión de dos clases** para predecir las tareas comunes de los sistemas de administración de relaciones con clientes (CRM): deserción de clientes. Los valores de los datos y las etiquetas se dividen en múltiples orígenes de datos y se codifican para anonimizar la información del cliente, sin embargo, todavía podemos usar al diseñador para combinar conjuntos de datos y entrenar un modelo con los valores ocultos.

Puesto que está intentando responder a la pregunta "¿Cuál?", esto se denomina un problema de clasificación, pero puede aplicar la misma lógica mostrada en este ejemplo para abordar cualquier tipo de problema de aprendizaje automático, ya sea una regresión, una clasificación, una agrupación en clústeres, etc.

Este es el gráfico completo de la canalización:

![Gráfico de la canalización](./media/how-to-designer-sample-classification-churn/pipeline-graph.png)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Haga clic en el ejemplo 5 para abrirlo. 

## <a name="data"></a>data

Los datos de esta canalización son de KDD Cup 2009. Tiene 50 000 filas y 230 columnas de característica. La tarea consiste en predecir la deserción, la apetencia y las ventas adicionales para clientes que usan estas características. Para más información sobre los datos y la tarea, consulte el [sitio web de KDD](https://www.kdd.org/kdd-cup/view/kdd-cup-2009).

## <a name="pipeline-summary"></a>Resumen de la canalización

Esta canalización de ejemplo en el diseñador muestra la predicción del clasificador binario de deserción, apetencia y mejora de ventas, una tarea común para la administración de relación con el cliente (CRM).

En primer lugar, realice un procesamiento de datos simple.

- Al conjunto de valores sin formato le faltan muchos valores. Use el módulo **Limpiar datos que faltan** para reemplazar los valores que faltan por 0.

    ![Limpieza del conjunto de datos](media/how-to-designer-sample-classification-churn/sample5-dataset-1225.png)

- Las características y la deserción correspondiente se encuentran en diferentes conjuntos de datos. Use el módulo **Agregar columnas** para anexar las columnas de etiqueta a las columnas de característica. La primera columna, **Col1**, es la columna de etiqueta. En el resultado de la visualización, podemos ver que el conjunto de resultados está desequilibrado. Hay más ejemplos negativos (-1) que ejemplos positivos (+ 1). Usaremos el módulo **SMOTE** para aumentar los casos subrepresentados más adelante.

    ![Adición del conjunto de datos de columna](./media/how-to-designer-sample-classification-churn/sample5-addcol-1225.png)



- Use el módulo **Dividir datos** para dividir el conjunto de datos en conjuntos de entrenamiento y prueba.

- A continuación, use el clasificador binario de árbol de decisión impulsado con los parámetros predeterminados para crear los modelos de predicción. Cree un modelo por tarea, es decir, un modelo para predecir ventas adicionales, otro para apetencia y otro para deserción de clientes.

- En la parte derecha de la canalización, usamos el módulo **SMOTE** para aumentar el porcentaje de ejemplos positivos. El porcentaje de SMOTE se establece en 100 para doblar los ejemplos positivos. Obtenga más información sobre cómo funciona el módulo SMOTE con la [referencia del módulo SMOTE0](algorithm-module-reference/smote.md).

## <a name="results"></a>Results

Visualice el resultado del módulo **Evaluar modelo** para ver el rendimiento del modelo en el conjunto de prueba. 

![Evaluar los resultados](./media/how-to-designer-sample-classification-churn/sample5-evaluate-1225.png)

 Puede mover el control deslizante **Umbral** y ver cómo cambian las métricas para la tarea de clasificación binaria. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para el diseñador:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-designer-sample-regression-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Ejemplo 3: clasificación con selección de características: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-designer-sample-classification-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: Conjunto de datos de SP 500 de Wikipedia](how-to-designer-sample-text-classification.md)
