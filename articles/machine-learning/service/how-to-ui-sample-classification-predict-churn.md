---
title: 'Ejemplo de interfaz visual n.º 5: clasificación para predecir la deserción de clientes, la apetencia y las ventas adicionales'
titleSuffix: Azure Machine Learning
description: Esta canalización de ejemplo de interfaz visual muestra la predicción del clasificador binario de la deserción de clientes, una tarea común en administración de relaciones con clientes (CRM).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 09/23/2019
ms.openlocfilehash: 82639779dde08bb1f71fb75dba62038dbf34d1b6
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693547"
---
# <a name="sample-5---classification-predict-churn-appetency-and-up-selling"></a>Ejemplo 5 - Clasificación: predicción de la deserción de clientes, la apetencia y las ventas adicionales 

Aprenda a compilar una canalización compleja de aprendizaje automático sin tener que escribir una sola línea de código con la interfaz visual.

Esta canalización entrena tres clasificadores de **árbol de decisión impulsado de dos clases** para predecir las tareas comunes de los sistemas de administración de relaciones con clientes (CRM): la deserción de clientes, la apetencia y las ventas adicionales. Los valores de datos y las etiquetas se dividen entre varios orígenes de datos y se codifican para anonimizar la información del cliente. No obstante, todavía podemos usar la interfaz visual para combinar conjuntos de datos y entrenar un modelo con los valores ocultos.

Puesto que está intentando responder a la pregunta "¿Cuál?", esto se denomina un problema de clasificación, pero puede aplicar la misma lógica mostrada en este ejemplo para abordar cualquier tipo de problema de aprendizaje automático, ya sea una regresión, una clasificación, una agrupación en clústeres, etc.

Este es el gráfico completo de la canalización:

![Gráfico de la canalización](./media/how-to-ui-sample-classification-predict-churn/pipeline-graph.png)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seleccione el botón **Abrir** correspondiente a la canalización del ejemplo 5.

    ![Apertura de la canalización](media/how-to-ui-sample-classification-predict-churn/open-sample5.png)

## <a name="data"></a>Datos

Los datos de esta canalización son de KDD Cup 2009. Tiene 50 000 filas y 230 columnas de característica. La tarea consiste en predecir la deserción, la apetencia y las ventas adicionales para clientes que usan estas características. Para más información sobre los datos y la tarea, consulte el [sitio web de KDD](https://www.kdd.org/kdd-cup/view/kdd-cup-2009).

## <a name="pipeline-summary"></a>Resumen de la canalización

Esta canalización de ejemplo de interfaz visual muestra la predicción del clasificador binario de la deserción de clientes, la apetencia y las ventas adicionales, una tarea común en administración de relaciones con clientes (CRM).

En primer lugar, realice un procesamiento de datos simple.

- Al conjunto de valores sin formato le faltan muchos valores. Use el módulo **Limpiar datos que faltan** para reemplazar los valores que faltan por 0.

    ![Limpieza del conjunto de datos](./media/how-to-ui-sample-classification-predict-churn/cleaned-dataset.png)

- Las características y las etiquetas correspondientes de deserción, apetencia y ventas adicionales se encuentran en conjuntos de datos distintos. Use el módulo **Agregar columnas** para anexar las columnas de etiqueta a las columnas de característica. La primera columna, **Col1**, es la columna de etiqueta. El resto de las columnas, **Var1**, **Var2**, etc., son las columnas de característica.

    ![Adición del conjunto de datos de columna](./media/how-to-ui-sample-classification-predict-churn/added-column1.png)

- Use el módulo **Dividir datos** para dividir el conjunto de datos en conjuntos de entrenamiento y prueba.

- A continuación, use el clasificador binario de árbol de decisión impulsado con los parámetros predeterminados para crear los modelos de predicción. Cree un modelo por tarea, es decir, un modelo para predecir ventas adicionales, otro para apetencia y otro para deserción de clientes.

## <a name="results"></a>Results

Visualice el resultado del módulo **Evaluar modelo** para ver el rendimiento del modelo en el conjunto de prueba. Para la tarea de ventas adicionales, la curva ROC muestra que el modelo funciona mejor que un modelo aleatorio. El área bajo la curva (AUC) es 0,857. En el umbral de 0,5, la precisión es 0,7, la recuperación es 0,463 y la puntuación F1 es 0,545.

![Evaluar los resultados](./media/how-to-ui-sample-classification-predict-churn/evaluate-result.png)

 Puede mover el control deslizante **Umbral** y ver cómo cambian las métricas para la tarea de clasificación binaria.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para la interfaz visual:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Ejemplo 3 - Clasificación: predicción del riesgo crediticio](how-to-ui-sample-classification-predict-credit-risk-basic.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-ui-sample-classification-predict-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: reseñas de libros](how-to-ui-sample-text-classification.md)
