---
title: 'Diseñador: Ejemplo de predicción del riesgo crediticio'
titleSuffix: Azure Machine Learning
description: Compile un clasificador y use scripts de Python personalizados para predecir el riesgo crediticio con el diseñador de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.date: 12/25/2019
ms.openlocfilehash: ed8ee9b1c711ee0056377154379b8df56e0785df
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964607"
---
# <a name="build-a-classifier--use-python-scripts-to-predict-credit-risk-using-azure-machine-learning-designer"></a>Compilación de un clasificador y uso scripts de Python para predecir el riesgo crediticio con el diseñador de Azure Machine Learning

**Ejemplo 4 del diseñador (versión preliminar)**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Este artículo muestra cómo compilar una canalización compleja de aprendizaje automático con el diseñador (versión preliminar). Aprenderá a implementar lógica personalizada mediante scripts de Python y a comparar varios modelos para elegir la mejor opción.

En este ejemplo se entrena un clasificador para que predecir el riesgo crediticio según información de aplicaciones de crédito, como, por ejemplo, el historial de créditos, la edad y el número de tarjetas de crédito. Sin embargo, puede aplicar los conceptos de este artículo para abordar su propios problemas de aprendizaje automático.

Este es el gráfico completo de la canalización:

[![Gráfico de la canalización](./media/how-to-designer-sample-classification-credit-risk-cost-sensitive/graph.png)](./media/how-to-designer-sample-classification-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Haga clic en el ejemplo 4 para abrirlo.

## <a name="data"></a>data

En este ejemplo se usa el conjunto de datos German Credit Card del repositorio de UC Irvine. Contiene 1000 muestras con 20 características y 1 etiqueta. Cada ejemplo representa una persona. Las 20 características incluyen características numéricas y categóricas. Para más información sobre el conjunto de datos, consulte el [sitio web de UCI](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29). La última columna es la etiqueta, que denota el riesgo crediticio y solo tiene dos valores posibles: alto = 2 y bajo = 1.

## <a name="pipeline-summary"></a>Resumen de la canalización

En esta canalización, se comparan dos enfoques diferentes de generación de modelos para solucionar este problema:

- Entrenar con el conjunto de datos original.
- Entrenar con un conjunto de datos replicado.

Con ambos enfoques se evalúan los modelos utilizando el conjunto de datos de prueba con replicación para garantizar que los resultados están alineados con la función de costo. Pruebe dos clasificadores con ambos enfoques: **Two-Class Support Vector Machine** (Máquina de vectores que admite dos clases) y **Two-Class Boosted Decision Tree** (Árbol de decisión promovido por dos clases).

El coste derivado de clasificar incorrectamente como alto un ejemplo de riesgo bajo es 1, mientras que, en el caso de clasificar incorrectamente como bajo un ejemplo de riesgo alto, el coste es 5. Usamos un módulo **Execute Python Script** (Ejecutar script de Python) para tener en cuenta el costo de la clasificación incorrecta.

Este es el gráfico de la canalización:

[![Gráfico de la canalización](./media/how-to-designer-sample-classification-credit-risk-cost-sensitive/graph.png)](./media/how-to-designer-sample-classification-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="data-processing"></a>Procesamiento de datos

Comenzamos usando el módulo **Metadata Editor** (Editor de metadatos) para agregar nombres de columna a fin de reemplazar los predeterminados por otros más descriptivos que se obtienen de la descripción del conjunto de datos del sitio de UCI. Proporcione los nuevos nombres de columna como valores separados por comas en el campo de nombre **New column** (Nueva columna) de **Metadata Editor** (Editor de metadatos).

A continuación, genere los conjuntos de entrenamiento y prueba usados para desarrollar el modelo de predicción de riesgos. Divida el conjunto de datos original en conjuntos de entrenamiento y prueba del el mismo tamaño mediante el módulo **Split Data** (Dividir datos). Para crear conjuntos de igual tamaño, establezca la opción **Fracción de filas en el primer conjunto de datos de salida** en 0.7.

### <a name="generate-the-new-dataset"></a>Generación del nuevo conjunto de datos

Como el costo de infravalorar el riesgo es elevado, establezca el costo de un error de clasificación como este:

- Para casos de alto riesgo clasificados incorrectamente como de bajo riesgo: 5
- Para casos de bajo riesgo clasificados incorrectamente como de alto riesgo: 1

Para reflejar esta función de costo, genere un nuevo conjunto de datos. En el nuevo conjunto de datos, cada ejemplo de alto riesgo se replica cinco veces, mientras que el número de ejemplos de bajo riesgo no varía. Divida los datos en conjuntos de datos de entrenamiento y prueba antes de la replicación para evitar que la misma fila esté en ambos conjuntos.

Para replicar los datos de alto riesgo, coloque este código de Python en un módulo **Execute Python Script** (Ejecutar script de Python):

```Python
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    df_label_1 = dataframe1[dataframe1.iloc[:, 20] == 1]
    df_label_2 = dataframe1[dataframe1.iloc[:, 20] == 2]

    result = df_label_1.append([df_label_2] * 5, ignore_index=True)
    return result,
```

El módulo **Execute Python Script** (Ejecutar script de Python) replica los conjuntos de datos de entrenamiento y de prueba.

### <a name="feature-engineering"></a>Ingeniería de características

El algoritmo **Two-Class Support Vector Machine** (Máquina de vectores que admite dos clases) requiere datos normalizados. Así que use el módulo **Normalize Data** (Normalizar datos) para normalizar los intervalos de todas las características numéricas con una transformación `tanh`. Una transformación `tanh` convierte todas las características numéricas en valores dentro de un intervalo de 0 y 1 conservando la distribución global de los valores.

El módulo **Two-Class Support Vector Machine** (Máquina de vectores de soporte de dos clases) controla las características de cadena al convertirlas en características categóricas y después en binarias con un valor de 0 o 1. Por lo tanto, no es necesario normalizar estas características.

## <a name="models"></a>Modelos

Dado que ha aplicado dos clasificadores (**Two-Class Support Vector Machine** [SVM] [Máquina de vectores que admite dos clases] y **Two-Class Boosted Decision Tree** [Árbol de decisión promovido por dos clases]) y también usamos dos conjuntos de datos, se generan cuatro modelos en total:

- SVM entrenado con datos originales.
- SVM entrenado con datos replicados.
- Árbol de decisión promovido entrenado con datos originales.
- Árbol de decisión promovido entrenado con datos replicados.

En este ejemplo se usa el flujo de trabajo de ciencia de datos estándar para crear, entrenar y probar los modelos:

1. Inicialice los algoritmos de aprendizaje con **Two-Class Support Vector Machine** (Máquina de vectores que admite dos clases) y **Two-Class Boosted Decision Tree** (Árbol de decisión promovido por dos clases).
1. Use **Train Model** (Entrenar modelo) para aplicar el algoritmo a los datos y crear el modelo real.
1. Use **Score Model** (Puntuar modelo) para generar puntuaciones con los ejemplos de prueba.

En el siguiente diagrama se muestra una parte de esta canalización, en que se usan los conjuntos de datos de entrenamiento originales y replicados para entrenar dos modelos SVM diferentes. **Train Model** (Entrenar modelo) está conectado al conjunto de entrenamiento y **Score Model** (Puntuar modelo), al conjunto de prueba.

![Gráfico de la canalización](./media/how-to-designer-sample-classification-credit-risk-cost-sensitive/score-part.png)

En la fase de evaluación de la canalización se calcula la precisión de cada uno de los cuatro modelos. En esta canalización, use **Evaluate Model** (Evaluar modelo) para comparar los ejemplos que tienen la misma clasificación incorrecta de costo.

El módulo **Evaluate Model** (Evaluar modelo) puede calcular las métricas de rendimiento de dos modelos de puntuación como máximo. Por lo tanto, puede usar una instancia de **Evaluate Model** (Evaluar modelo) para evaluar los dos modelos SVM y otra para los dos modelos de árbol de decisión promovido.

Observe que en **Score Model** (Puntuar modelo) se usa como entrada el conjunto de datos replicados de prueba. En otras palabras, las puntuaciones de precisión finales incluyen el costo por obtener mal las etiquetas.

## <a name="combine-multiple-results"></a>Combinación de varios resultados

El módulo **Evaluate Model** (Evaluar modelo) genera una tabla con una sola fila que contiene varias métricas. Para crear un único conjunto de resultados de precisión, primero usamos **Add Rows** (Agregar filas) para combinar los resultados en una sola tabla. A continuación, usamos el siguiente script de Python en el módulo **Execute Python Script** (Ejecutar script de Python) para agregar el nombre del modelo y el enfoque de entrenamiento para cada fila de la tabla de resultados:

```Python
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    new_cols = pd.DataFrame(
            columns=["Algorithm","Training"],
            data=[
                ["SVM", "weighted"],
                ["SVM", "unweighted"],
                ["Boosted Decision Tree","weighted"],
                ["Boosted Decision Tree","unweighted"]
            ])

    result = pd.concat([new_cols, dataframe1], axis=1)
    return result,
```

## <a name="results"></a>Results

Para ver los resultados de la canalización, haga clic con el botón derecho en la salida de Visualize (Visualizar) del último módulo **Select Columns in Dataset** (Seleccionar columnas del conjunto de datos).

![Visualización de la salida](media/how-to-designer-sample-classification-credit-risk-cost-sensitive/sample4-lastselect-1225.png)

La primera columna muestra el algoritmo de aprendizaje automático usado para generar el modelo.

La segunda columna indica el tipo de conjunto de entrenamiento.

La tercera contiene el valor de precisión sensible a los costes.

En estos resultados, puede ver que se proporciona la mejor precisión mediante el modelo que se creó con **Two-Class Support Vector Machine** (Máquina de vectores que admite dos clases) y se entrenó con el conjunto de datos de entrenamiento replicados.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para el diseñador:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-designer-sample-regression-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Ejemplo 3: clasificación con selección de características: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-designer-sample-classification-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-designer-sample-classification-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: Conjunto de datos de SP 500 de Wikipedia](how-to-designer-sample-text-classification.md)
