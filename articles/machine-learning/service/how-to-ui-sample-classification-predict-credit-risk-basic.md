---
title: 'Ejemplo de interfaz visual n.º 3: clasificación para predecir el riesgo de crédito'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo compilar un clasificador de aprendizaje automático sin necesidad de escribir una sola línea de código mediante la interfaz visual.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 09/23/2019
ms.openlocfilehash: 4649303b8ee643130b8e254f01bfffbe8ad9eb2b
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693501"
---
# <a name="sample-3---classification-predict-credit-risk"></a>Ejemplo 3 - Clasificación: Predicción del riesgo de crédito

Obtenga información sobre cómo compilar un clasificador de aprendizaje automático sin necesidad de escribir una sola línea de código mediante la interfaz visual. En esta canalización de ejemplo se entrena un **árbol de decisión ampliado de dos clases** para que prediga el riesgo de crédito (alto o bajo) según información de solicitudes de crédito, como, por ejemplo, el historial de créditos, la edad y el número de tarjetas de crédito.

Porque la pregunta es responder "¿Cuál?" esto se conoce como un problema de clasificación. Sin embargo, puede aplicar el mismo proceso fundamental para abordar cualquier tipo de problema de aprendizaje automático, ya sea una regresión, una clasificación, una agrupación en clústeres, etc.

Este es el gráfico de la canalización final de este ejemplo:

![Gráfico de la canalización](media/how-to-ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seleccione el botón **Abrir** correspondiente a la canalización del ejemplo 3:

    ![Apertura de la canalización](media/how-to-ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>El ejemplo relacionado

[Ejemplo 4 - Clasificación: Predicción del riesgo de crédito (sensible a los costos)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md) proporciona una canalización avanzada que resuelve el mismo problema que este ejemplo. Muestra cómo realizar una clasificación *sensible a los costos* mediante el módulo **Ejecutar script de Python** y cómo comparar el rendimiento de dos algoritmos de clasificación binarios. Consúltelo para obtener más información sobre cómo compilar canalizaciones de clasificación.


## <a name="data"></a>Datos

En el ejemplo se usa el conjunto de datos German Credit Card del repositorio de UC Irvine. El conjunto de datos contiene 1000 muestras con 20 características y 1 etiqueta. Cada ejemplo representa una persona. Las características incluyen características numéricas y categóricas. Consulte el [sitio web de UCI](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) para conocer el significado de las características categóricas. La última columna es la etiqueta, que denota el riesgo crediticio y solo tiene dos valores posibles: alto = 2 y bajo = 1.

## <a name="pipeline-summary"></a>Resumen de la canalización

Para crear la canalización, siga estos pasos:

1. Arrastre el módulo del conjunto de datos Tarjeta de crédito alemana de los datos de UCI al lienzo de la canalización.
1. Agregue un módulo **Editar metadatos** para poder agregar nombres descriptivos para cada columna.
1. Agregue un módulo **Dividir datos** para crear los conjuntos de entrenamiento y prueba. Establezca la fracción de filas del primer conjunto de datos de salida en 0,7. Esta configuración especifica que un 70 % de los datos saldrán por el puerto de la izquierda del módulo y el resto por el puerto de la derecha. Usaremos el conjunto de datos de la izquierda para el entrenamiento y el de la derecha, para las pruebas.
1. Agregue un módulo **Árbol de decisión ampliado de dos clases** para inicializar un clasificador de árbol de decisión ampliado.
1. Agregue un módulo **Entrenar modelo**. Conecte el clasificador del paso anterior con el puerto de entrada de la izquierda de **Entrenar modelo**. Agregue el conjunto de entrenamiento (puerto de salida izquierdo de **Dividir datos**) al puerto de entrada derecho de **Entrenar modelo**. El módulo **Entrenar modelo** entrenará el clasificador.
1. Agregue un módulo **Puntuar modelo** y conecte con él el módulo **Entrenar modelo**. A continuación, agregue el conjunto de pruebas (el puerto de la derecha de **Dividir datos**) a **Puntuar modelo**. El módulo **Puntuar modelo** hará las predicciones. Puede seleccionar su puerto de salida para ver las predicciones y las probabilidades de clase positiva.
1. Agregue un módulo **Evaluar modelo** y conecte el conjunto de datos puntuado a su puerto de entrada izquierdo. Para ver los resultados de la evaluación, seleccione el puerto de salida del módulo **Evaluar modelo** y seleccione **Visualizar**.

## <a name="results"></a>Results

![Evaluar los resultados](media/how-to-ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

En los resultados de la evaluación, puede ver que el AUC del modelo es 0,776. En el umbral de 0,5, la precisión es 0,621, la recuperación es 0,456 y la puntuación F1 es 0,526.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para la interfaz visual:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: Comparar algoritmos para la predicción de precios de automóviles](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-ui-sample-classification-predict-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-ui-sample-classification-predict-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: reseñas de libros](how-to-ui-sample-text-classification.md)