---
title: 'Clasificación: predicción de la deserción de clientes, la apetencia y las ventas adicionales '
titleSuffix: Azure Machine Learning service
description: Este experimento de ejemplo de interfaz visual muestra la predicción del clasificador binario de la deserción de clientes, una tarea común en administración de relaciones con clientes (CRM).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 7d10d996febd0e31c9085bf5cb82324cce101c80
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606153"
---
# <a name="sample-5---classification-predict-churn-appetency-and-up-selling"></a>Ejemplo 5 - Clasificación: predicción de la deserción de clientes, la apetencia y las ventas adicionales 

Aprenda a compilar un experimento de aprendizaje automático sin necesidad de escribir una sola línea de código mediante la interfaz visual.

Este experimento entrena tres clasificadores de **árbol de decisión impulsado de dos clases** para predecir las tareas comunes de los sistemas de administración de relaciones con clientes (CRM): la deserción de clientes, la apetencia y las ventas adicionales. Los valores de datos y las etiquetas se dividen entre varios orígenes de datos y se codifican para anonimizar la información del cliente. No obstante, todavía podemos usar la interfaz visual para combinar conjuntos de datos y entrenar un modelo con valores codificados.

Puesto que estamos intentando responder a la pregunta "¿Cuál?", esto se conoce como un problema de clasificación. Sin embargo, puede aplicar los mismos pasos para abordar cualquier tipo de problema de aprendizaje automático, ya sea una regresión, una clasificación, una agrupación en clústeres, etc.

A continuación se muestra el gráfico completo del experimento:

![Gráfico del experimento](./media/ui-sample-classification-predict-churn/experiment-graph.png)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seleccione el botón **Abrir** correspondiente al experimento Ejemplo 5.

    ![Abrir el experimento](media/ui-sample-classification-predict-churn/open-sample5.png)

## <a name="data"></a>Datos

Los datos que se utilizan en este experimento son de KDD Cup 2009. El conjunto de datos tiene 50 000 filas y 230 columnas de característica. La tarea consiste en predecir la deserción, la apetencia y las ventas adicionales para clientes que usan estas características. Para más información sobre los datos y la tarea, consulte el [sitio web de KDD](https://www.kdd.org/kdd-cup/view/kdd-cup-2009).

## <a name="experiment-summary"></a>Resumen del experimento

Este experimento de ejemplo de interfaz visual muestra la predicción del clasificador binario de la deserción de clientes, la apetencia y las ventas adicionales, una tarea común en administración de relaciones con clientes (CRM).

En primer lugar, se realiza un procesamiento de datos simple.

- El conjunto de datos sin procesar contiene una gran cantidad de valores que faltan. Usamos el módulo **Limpiar datos que faltan** para reemplazar los valores que faltan por 0.

    ![Limpieza del conjunto de datos](./media/ui-sample-classification-predict-churn/cleaned-dataset.png)

- Las características y las etiquetas correspondientes de deserción, apetencia y ventas adicionales se encuentran en conjuntos de datos distintos. Usamos el módulo **Agregar columnas** para anexar las columnas de etiqueta a las columnas de característica. La primera columna, **Col1**, es la columna de etiqueta. El resto de las columnas, **Var1**, **Var2**, etc., son las columnas de característica.

    ![Adición del conjunto de datos de columna](./media/ui-sample-classification-predict-churn/added-column1.png)

- Usamos el módulo **Dividir datos** para dividir el conjunto de datos en conjuntos de entrenamiento y prueba.

    A continuación, usamos el clasificador binario de árbol de decisión impulsado con los parámetros predeterminados para crear los modelos de predicción. Creamos un modelo por tarea, es decir, un modelo para predecir ventas adicionales, otro para apetencia y otro para deserción de clientes.

## <a name="results"></a>Results

Visualice el resultado del módulo **Evaluar modelo** para ver el rendimiento del modelo en el conjunto de prueba. Para la tarea de ventas adicionales, la curva ROC muestra que el modelo funciona mejor que un modelo aleatorio. El área bajo la curva (AUC) es 0,857. En el umbral de 0,5, la precisión es 0,7, la recuperación es 0,463 y la puntuación F1 es 0,545.

![Evaluar los resultados](./media/ui-sample-classification-predict-churn/evaluate-result.png)

 Puede mover el control deslizante **Umbral** y ver cómo cambian las métricas para la tarea de clasificación binaria.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para la interfaz visual:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](ui-sample-regression-predict-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Ejemplo 3 - Clasificación: predicción del riesgo crediticio](ui-sample-classification-predict-credit-risk-basic.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](ui-sample-classification-predict-flight-delay.md)