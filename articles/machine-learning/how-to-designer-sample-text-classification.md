---
title: 'Diseñador: ejemplo de clasificación de las reseñas de libros'
titleSuffix: Azure Machine Learning
description: Compile un clasificador de regresión logística multiclase para predecir la categoría de la empresa con el conjunto de datos SP 500 de Wikipedia mediante el diseñador de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: xiaoharper
ms.author: zhanxia
ms.reviewer: peterlu
ms.date: 11/04/2019
ms.openlocfilehash: 4d22fd39eae5d5cf207d6d44819f0ce7ab2eceb5
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963248"
---
# <a name="build-a-classifier-to-predict-company-category-using-azure-machine-learning-designer"></a>Compile un clasificador para predecir la categoría de la empresa con el diseñador de Azure Machine Learning.

**Ejemplo 7 del diseñador (versión preliminar)**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

En este ejemplo se muestra cómo usar los módulos de análisis de texto para compilar una canalización de clasificación de texto en el diseñador de Azure Machine Learning (versión preliminar).

El objetivo de la clasificación de texto es asignar parte de un texto a una o varias clases o categorías predefinidas. El fragmento de texto puede ser un documento, un artículo de noticias, una consulta de búsqueda, un correo electrónico, un tweet, vales de soporte técnico, comentarios de cliente, reseñas de productos de usuario, etc. Entre las aplicaciones de clasificación de texto se incluye la categorización de artículos de periódicos y el contenido de las noticias en temas, la organización de páginas web en categorías jerárquicas, el filtrado de correo electrónico no deseado, el análisis de opiniones, la predicción de la intención del usuario a partir de las consultas de búsqueda, enrutamiento de las incidencias de soporte técnico y análisis de los comentarios de los clientes. 

Esta canalización entrena un **clasificador de regresión logística multiclase** para predecir la categoría de la empresa con el **conjunto de datos SP 500 de wikipedia que procede de la Wikipedia**.  

Los pasos fundamentales de un modelo de aprendizaje automático son:

1. Obtener los datos

1. Preprocesar los datos de texto

1. Diseño de características

   Convierta la característica de texto en la característica numérica con el módulo de extracción de características, como el hash de características, extraiga la característica n-grama de los datos de texto.

1. Entrenamiento del modelo

1. Puntuación de conjunto de datos

1. Evaluación del modelo

Aquí se encuentra el último y completo gráfico de la canalización en la que trabajaremos. Le presentaremos el razonamiento de todos los módulos para que pueda tomar sus propias decisiones.

[![Gráfico de la canalización](./media/how-to-designer-sample-text-classification/nlp-modules-overall.png)](./media/how-to-designer-sample-text-classification/nlp-modules-overall.png#lightbox)

## <a name="data"></a>data

En este canalización, usamos el conjunto de datos **Wikipedia SP 500**. Los conjuntos de datos se han extraído de Wikipedia (https://www.wikipedia.org/) ) y se basan en artículos de cada empresa del índice S&P 500. Antes de subir al diseñador de Azure Machine Learning, el conjunto de datos se procesaba de la siguiente manera:

- Se extrajo el contenido de texto para cada empresa específica.
- Se eliminó el formato wiki.
- Se eliminaron los caracteres no alfanuméricos
- Se convirtió todo el texto a minúscula.
- Se agregaron las categorías de empresas conocidas.

No se encontraron artículos para algunas empresas, por lo que el número de registros es inferior a 500.

## <a name="pre-process-the-text-data"></a>Preprocesar los datos de texto

Usamos el módulo **Preprocess Text** (Preprocesar texto) para preprocesar los datos de texto, incluida la detección de oraciones, las oraciones con tokens, etc. Encontrará todas las opciones admitidas en el artículo [**Preprocesamiento de texto**](algorithm-module-reference/preprocess-text.md). Después de preprocesar los datos de texto, usamos el módulo **Split Data** (Dividir datos) para dividir aleatoriamente los datos de entrada para que el conjunto de datos de entrenamiento contenga el 50 % de los datos originales y el de prueba, el 50 %.

## <a name="feature-engineering"></a>Diseño de características
En este ejemplo, usaremos dos métodos que realizan el diseño de características.

### <a name="feature-hashing"></a>Hash de características
Usamos el módulo [**Feature Hashing**](algorithm-module-reference/feature-hashing.md) (Hash de características) para convertir el texto sin formato de los artículos en enteros y usar los valores enteros como características de entrada para el modelo. 

El módulo **Feature Hashing** (Hash de características) se puede usar para convertir documentos de texto de longitud variable en vectores de características numéricos de igual longitud, mediante el método de hash murmurhash v3 de 32 bits proporcionado por la biblioteca de wabbit de Vowpal. El objetivo de usar el hash de características es la reducción de la dimensionalidad; además, con el hash de características, la búsqueda de pesos de características sea más rápida en el momento de la clasificación porque utiliza la comparación de valores hash en lugar de la comparación de cadenas.

En la canalización de la muestra, se establece el número de bits de hash a 14 y el número de n-gramos a 2. Con esta configuración, la tabla hash puede contener 2^14 entradas, en las que cada característica de hash representa una o más características de n-grama y su valor representa la frecuencia de repetición de la n-grama en la instancia de texto. Para muchos problemas, es más adecuada una tabla hash de este tamaño, pero en algunos casos es posible que se necesite más espacio para evitar colisiones. Evalúe el rendimiento de la solución de aprendizaje automático con un número de bits diferente. 

### <a name="extract-n-gram-feature-from-text"></a>Extracción de características de n-gramas del texto

Un n-grama es una secuencia contigua de n términos de una secuencia de texto determinada. Un n-grama de tamaño 1 se conoce como unigrama; un n-grama de tamaño 2 es bigrama; un n-grama de tamaño 3 es un trigrama. A veces, el valor de n hace referencia a n-gramas de tamaños mayores, por ejemplo, "cuatro-gramas", "cinco-gramas", etc.

Usamos el módulo [**Extract N-Gram Feature from Text**](algorithm-module-reference/extract-n-gram-features-from-text.md) (Extracción de características de n-gramas del texto) como otra solución para el diseño de características. En este módulo se extrae primero el conjunto de n-gramas, además de los n-gramas, el número de documentos en los que aparece cada n-grama en el texto. En este ejemplo, se usa la métrica TF-IDF para calcular los valores de las características. A continuación, convierte los datos de texto no estructurados en vectores de características numéricos de igual longitud, donde cada característica representa el TF-IDF de un n-grama en una instancia de texto.

Después de convertir los datos de texto en vectores de característica numéricos, se usa el módulo **Select Column** (Seleccionar columna) para quitar los datos de texto del conjunto de datos. 

## <a name="train-the-model"></a>Entrenamiento del modelo

La elección del algoritmo a menudo depende de los requisitos del caso de uso. Dado que el objetivo de esta canalización es predecir la categoría de la empresa, un modelo de clasificador de clase múltiple es una buena elección. Teniendo en cuenta que el número de características es grande y que son escasas, usamos el modelo **Multiclass Logistic Regression** para esta canalización.

## <a name="test-evaluate-and-compare"></a>Prueba, evaluación y comparación

 Dividimos el conjunto de datos y usamos conjuntos de datos diferentes para entrenar y probar el modelo para que la evaluación sea más objetiva.

Después de haber entrenado el modelo, usamos los módulos **Score Model** (Puntuar modelo) y **Evaluate Model** (Evaluar modelo) para generar los resultados de predicción y evaluar los modelos. Sin embargo, antes de usar el módulo **Score Model** (Puntuar modelo), es necesario realizar el diseño de características, tal como se ha hecho durante el entrenamiento. 

Para el módulo **Feature Hashing** (Hash de características), es fácil realizar un diseño de las características en el flujo de puntuación como flujo de aprendizaje. Use el módulo **Feature Hashing** (Hash de características) directamente para procesar los datos de texto de entrada.

En el caso del módulo **Extract N-Gram Feature from Text** (Extracción de características de n-gramas del texto), se conectará a la **salida del vocabulario resultante** desde el flujo de datos de entrenamiento al **vocabulario de entrada** en el flujo de datos de puntuación y se establecerá el parámetro **Vocabulary mode** (Modo de vocabulario) en **ReadOnly**.
[![Gráfico de puntuación de n-grama](./media/how-to-designer-sample-text-classification/n-gram.png)](./media/how-to-designer-sample-text-classification/n-gram.png)

Después de finalizar el paso de ingeniería, **Score Model** (Puntuar modelo) podría usarse para generar predicciones para el conjunto de datos de prueba mediante el modelo entrenado. Para comprobar el resultado, seleccione el puerto de salida de **Score Model** (Puntuar modelo) y **Visualize** (Visualizar).

A continuación, pasamos las puntuaciones al módulo **Evaluate Model** (Evaluar modelo) para generar métricas de evaluación. **Evaluate Model** (Evaluar modelo) tiene dos puertos de entrada, por lo que podríamos evaluar y comparar los conjuntos de datos puntuados que se generan con distintos métodos. En este ejemplo, se compara el rendimiento del resultado generado con el método de hash de características y el método n-grama.
Para comprobar el resultado, seleccione el puerto de salida de **Evaluate Model** (Evaluar modelo) y **Visualize** (Visualizar).

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para el diseñador:
- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-designer-sample-regression-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Ejemplo 3: clasificación con selección de características: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-designer-sample-classification-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-designer-sample-classification-flight-delay.md)
