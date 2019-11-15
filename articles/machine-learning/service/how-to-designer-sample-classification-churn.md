---
title: 'Diseñador: Clasificación, predicción de renovación'
titleSuffix: Azure Machine Learning
description: Este diseñador (versión preliminar) muestra un ejemplo de canalización que presenta la predicción de deserción con un clasificador binario, una tarea común para la administración de relaciones con los clientes (CRM).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 11/04/2019
ms.openlocfilehash: a09060e16b877df4412b785edbb60930cb4122bc
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73647171"
---
# <a name="sample-5---classification-predict-churn"></a>Ejemplo 5 - Clasificación: Predicción de la renovación
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

Obtenga información sobre cómo compilar una canalización compleja de aprendizaje automático sin tener que escribir una sola línea de código con el diseñador (versión preliminar).

Esta canalización entrena 2 clasificadores de **árbol de decisión de dos clases** para predecir las tareas comunes de los sistemas de administración de relaciones con clientes (CRM): deserción de clientes. Los valores de los datos y las etiquetas se dividen en múltiples orígenes de datos y se codifican para anonimizar la información del cliente, sin embargo, todavía podemos usar al diseñador para combinar conjuntos de datos y entrenar un modelo con los valores ocultos.

Puesto que está intentando responder a la pregunta "¿Cuál?", esto se denomina un problema de clasificación, pero puede aplicar la misma lógica mostrada en este ejemplo para abordar cualquier tipo de problema de aprendizaje automático, ya sea una regresión, una clasificación, una agrupación en clústeres, etc.

Este es el gráfico completo de la canalización:

![Gráfico de la canalización](./media/how-to-ui-sample-classification-predict-churn/pipeline-graph.png)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Haga clic en el ejemplo 5 para abrirlo. 

## <a name="data"></a>Datos

Los datos de esta canalización son de KDD Cup 2009. Tiene 50 000 filas y 230 columnas de característica. La tarea consiste en predecir la deserción, la apetencia y las ventas adicionales para clientes que usan estas características. Para más información sobre los datos y la tarea, consulte el [sitio web de KDD](https://www.kdd.org/kdd-cup/view/kdd-cup-2009).

## <a name="pipeline-summary"></a>Resumen de la canalización

Esta canalización de ejemplo en el diseñador muestra la predicción del clasificador binario de deserción, apetencia y mejora de ventas, una tarea común para la administración de relación con el cliente (CRM).

En primer lugar, realice un procesamiento de datos simple.

- Al conjunto de valores sin formato le faltan muchos valores. Use el módulo **Limpiar datos que faltan** para reemplazar los valores que faltan por 0.

    ![Limpieza del conjunto de datos](./media/how-to-ui-sample-classification-predict-churn/cleaned-dataset.png)

- Las características y la deserción correspondiente se encuentran en diferentes conjuntos de datos. Use el módulo **Agregar columnas** para anexar las columnas de etiqueta a las columnas de característica. La primera columna, **Col1**, es la columna de etiqueta. En el resultado de la visualización, podemos ver que el conjunto de resultados está desequilibrado. Hay más ejemplos negativos (-1) que ejemplos positivos (+ 1). Usaremos el módulo **SMOTE** para aumentar los casos subrepresentados más adelante.

    ![Adición del conjunto de datos de columna](./media/how-to-ui-sample-classification-predict-churn/added-column1.png)



- Use el módulo **Dividir datos** para dividir el conjunto de datos en conjuntos de entrenamiento y prueba.

- A continuación, use el clasificador binario de árbol de decisión impulsado con los parámetros predeterminados para crear los modelos de predicción. Cree un modelo por tarea, es decir, un modelo para predecir ventas adicionales, otro para apetencia y otro para deserción de clientes.

- En la parte derecha de la canalización, usamos el módulo **SMOTE** para aumentar el porcentaje de ejemplos positivos. El porcentaje de SMOTE se establece en 100 para doblar los ejemplos positivos. Obtenga más información sobre cómo funciona el módulo SMOTE con la [referencia del módulo SMOTE0](../././algorithm-module-reference/SMOTE.md).

## <a name="results"></a>Results

Visualice el resultado del módulo **Evaluar modelo** para ver el rendimiento del modelo en el conjunto de prueba. 

![Evaluar los resultados](./media/how-to-ui-sample-classification-predict-churn/evaluate-result.png)

 Puede mover el control deslizante **Umbral** y ver cómo cambian las métricas para la tarea de clasificación binaria. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para el diseñador:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-designer-sample-regression-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Ejemplo 3: clasificación con selección de características: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-designer-sample-classification-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: Conjunto de datos de SP 500 de Wikipedia](how-to-designer-sample-text-classification.md)
