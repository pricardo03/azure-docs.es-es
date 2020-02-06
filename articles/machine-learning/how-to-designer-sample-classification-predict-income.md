---
title: 'Diseñador: Ejemplo de clasificación para predicción de ingresos'
titleSuffix: Azure Machine Learning
description: Siga este ejemplo para crear un clasificador sin código para predecir los ingresos con el diseñador de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.date: 12/25/2019
ms.openlocfilehash: 560339fb04e3bbbe42c4370655e74e8536a7c015
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963381"
---
# <a name="build-a-classifier--use-feature-selection-to-predict-income-with-azure-machine-learning-designer"></a>Creación de un clasificador y uso de la selección de características para predecir los ingresos con el diseñador de Azure Machine Learning

**Ejemplo 3 del diseñador (versión preliminar)**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Obtenga información sobre cómo compilar un clasificador de aprendizaje automático sin tener que escribir una sola línea de código con el diseñador (versión preliminar). Esta ejemplo entrena un **árbol de decisión de dos clases** para predecir los ingresos del censo de adultos (>=50K o <=50K).

Porque la pregunta es responder "¿Cuál?" esto se conoce como un problema de clasificación. Sin embargo, puede aplicar el mismo proceso fundamental para abordar cualquier tipo de problema de aprendizaje automático: una regresión, una clasificación, una agrupación en clústeres, etc.

Este es el gráfico de la canalización final de este ejemplo:

![Gráfico de la canalización](./media/how-to-designer-sample-classification-predict-income/overall-graph.png)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Haga clic en el ejemplo 3 para abrirlo.



## <a name="data"></a>data

El conjunto de datos contiene 14 características y una columna de etiqueta. Existen varios tipos de características, lo que incluye numéricas y de categorías. En el diagrama siguiente se muestra un extracto del conjunto de datos: ![datos](media/how-to-designer-sample-classification-predict-income/sample3-dataset-1225.png)



## <a name="pipeline-summary"></a>Resumen de la canalización

Para crear la canalización, siga estos pasos:

1. Arrastre el módulo de conjunto de datos Ingresos binarios del censo de adultos al lienzo de la canalización.
1. Agregue un módulo **Dividir datos** para crear los conjuntos de entrenamiento y prueba. Establezca la fracción de filas del primer conjunto de datos de salida en 0,7. Esta configuración especifica que un 70 % de los datos saldrán por el puerto de la izquierda del módulo y el resto por el puerto de la derecha. Usaremos el conjunto de datos de la izquierda para el entrenamiento y el de la derecha, para las pruebas.
1. Agregue el módulo de **Selección de características basada en filtros** para seleccionar 5 características de PearsonCorreclation. 
1. Agregue un módulo **Árbol de decisión ampliado de dos clases** para inicializar un clasificador de árbol de decisión ampliado.
1. Agregue un módulo **Entrenar modelo**. Conecte el clasificador del paso anterior con el puerto de entrada de la izquierda de **Entrenar modelo**. Conecte el conjunto de datos filtrado del módulo Selección de características basada en filtros como conjunto de datos de entrenamiento.  El módulo **Entrenar modelo** entrenará el clasificador.
1. Agregue el módulo Selección de transformación de columnas y Aplicar transformación para aplicar la misma transformación (selección de características basada en filtros) al conjunto de datos de prueba.
![aplicar-transformación](./media/how-to-designer-sample-classification-predict-income/transformation.png)
1. Agregue el módulo de **Puntuación modelo** y conecte al mismo el módulo **Entrenar modelo**. Después, agregue el conjunto de pruebas (la salida del módulo Aplicar transformación, que también aplica la selección de características para el conjunto de pruebas) a **Puntuación modelo**. El módulo **Puntuar modelo** hará las predicciones. Puede seleccionar su puerto de salida para ver las predicciones y las probabilidades de clase positiva.


    Esta canalización tiene dos módulos de puntuación, el que está a la derecha ha excluido la columna de etiqueta antes de hacer la predicción. Esto está preparado para implementar un punto de conexión en tiempo real, ya que la entrada del servicio web solo esperará las características sin etiqueta. 

1. Agregue un módulo **Evaluar modelo** y conecte el conjunto de datos puntuado a su puerto de entrada izquierdo. Para ver los resultados de la evaluación, seleccione el puerto de salida del módulo **Evaluar modelo** y seleccione **Visualizar**.

## <a name="results"></a>Results

![Evaluar los resultados](media/how-to-designer-sample-classification-predict-income/sample3-evaluate-1225.png)

En los resultados de la evaluación, se puede ver que las curvas como ROC, la precisión de recuperación y las métricas de confusión. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para el diseñador:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-designer-sample-regression-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-designer-sample-classification-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-designer-sample-classification-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: Conjunto de datos de SP 500 de Wikipedia](how-to-designer-sample-text-classification.md)
