---
title: 'Clasificación: Predicción del riesgo de crédito'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo compilar un clasificador de aprendizaje automático sin necesidad de escribir una sola línea de código mediante la interfaz visual.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 6ec91ca83d7aa1bc5e6c290d35b573a60cc0ed19
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605766"
---
# <a name="sample-3---classification-predict-credit-risk"></a>Ejemplo 3 - Clasificación: Predicción del riesgo de crédito

Obtenga información sobre cómo compilar un clasificador de aprendizaje automático sin necesidad de escribir una sola línea de código mediante la interfaz visual. En este ejemplo se entrena un **árbol de decisión ampliado de dos clases** para que prediga el riesgo de crédito (alto o bajo) según información de solicitudes de crédito, como, por ejemplo, el historial de créditos, la edad y el número de tarjetas de crédito.

Puesto que estamos intentando responder a la pregunta "¿Cuál?", esto se conoce como un problema de clasificación. Sin embargo, puede aplicar el mismo proceso fundamental para abordar cualquier tipo de problema de aprendizaje automático, ya sea una regresión, una clasificación, una agrupación en clústeres, etc.

A continuación se muestra el gráfico completado para este experimento:

![Gráfico del experimento](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seleccione el botón **Abrir** correspondiente al experimento Ejemplo 3:

    ![Abrir el experimento](media/ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>El ejemplo relacionado

[Ejemplo 4 - Clasificación: Predicción del riesgo de crédito (sensible a los costos)](ui-sample-classification-predict-credit-risk-cost-sensitive.md) proporciona un experimento avanzado que resuelve el mismo problema que este experimento. Muestra cómo realizar una clasificación _sensible a los costos_ mediante el módulo **Ejecutar script de Python** y cómo comparar el rendimiento de dos algoritmos de clasificación binarios. Consúltelo para obtener más información sobre cómo compilar experimentos de clasificación.

## <a name="data"></a>Datos

Usamos el conjunto de datos Tarjeta de crédito alemana del repositorio de UC Irvine.
El conjunto de datos contiene 1000 muestras con 20 características y 1 etiqueta. Cada ejemplo representa a una persona. Las características incluyen características numéricas y categóricas. Consulte el [sitio web de UCI](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) para conocer el significado de las características categóricas. La última columna es la etiqueta, que denota el riesgo de crédito y solo tiene dos valores posibles: riesgo de crédito alto = 2 y riesgo de crédito bajo = 1.

## <a name="experiment-summary"></a>Resumen del experimento

Para crear el experimento, siga estos pasos:

1. Arrastre el módulo del conjunto de datos Tarjeta de crédito alemana de los datos de UCI al lienzo del experimento.
1. Agregue un módulo **Editar metadatos** para poder agregar nombres descriptivos para cada columna.
1. Agregue un módulo **Dividir datos** para crear los conjuntos de entrenamiento y prueba. Establezca la fracción de filas del primer conjunto de datos de salida en 0,7. Esta configuración especifica que un 70 % de los datos saldrán por el puerto de la izquierda del módulo y el resto por el puerto de la derecha. Usaremos el conjunto de datos de la izquierda para el entrenamiento y el de la derecha, para las pruebas.
1. Agregue un módulo **Árbol de decisión ampliado de dos clases** para inicializar un clasificador de árbol de decisión ampliado.
1. Agregue un módulo **Entrenar modelo**. Conecte el clasificador del paso anterior con el puerto de entrada de la izquierda de **Entrenar modelo**. Agregue el conjunto de entrenamiento (puerto de salida izquierdo de **Dividir datos**) al puerto de entrada derecho de **Entrenar modelo**. El módulo **Entrenar modelo** entrenará el clasificador.
1. Agregue un módulo **Puntuar modelo** y conecte con él el módulo **Entrenar modelo**. A continuación, agregue el conjunto de pruebas (el puerto de la derecha de **Dividir datos**) a **Puntuar modelo**. El módulo **Puntuar modelo** hará las predicciones. Puede seleccionar su puerto de salida para ver las predicciones y las probabilidades de clase positiva.
1. Agregue un módulo **Evaluar modelo** y conecte el conjunto de datos puntuado a su puerto de entrada izquierdo. Para ver los resultados de la evaluación, seleccione el puerto de salida del módulo **Evaluar modelo** y seleccione **Visualizar**.

A continuación se muestra el gráfico completo del experimento:

![Gráfico del experimento](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="results"></a>Results

![Evaluar los resultados](media/ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

En los resultados de la evaluación, puede ver que el AUC del modelo es 0,776. En el umbral de 0,5, la precisión es 0,621, la recuperación es 0,456 y la puntuación F1 es 0,526.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para la interfaz visual:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](ui-sample-regression-predict-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: Comparar algoritmos para la predicción de precios de automóviles](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](ui-sample-classification-predict-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](ui-sample-classification-predict-flight-delay.md)