---
title: Resumen de datos
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo Resumir datos en Azure Machine Learning para generar un informe básico de estadísticas descriptivas para las columnas de un conjunto de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/09/2019
ms.openlocfilehash: c8051126fc4a895c6e72e90942fac65d777afd8e
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76546493"
---
# <a name="summarize-data"></a>Resumen de datos

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Use el módulo Summarize Data (Resumen de datos) para crear un conjunto de medidas estadísticas estándar que describan cada columna de la tabla de entrada.

Las estadísticas resumidas son útiles cuando se desea conocer las características del conjunto de información completo. Por ejemplo, podría necesitar saber:

- ¿Cuántos valores faltan en cada columna?
- ¿Cuántos valores únicos hay en una columna de características?
- ¿Cuál es la media y la desviación típica de cada columna?

El módulo calcula las puntuaciones importantes de cada columna y devuelve una fila de estadísticas resumidas para cada variable (columna de datos) que se proporciona como entrada.

## <a name="how-to-configure-summarize-data"></a>Configuración de Summarize Data (Resumen de datos)  

1. Agregue el módulo **Summarize Data** (Resumen de datos) a la canalización. Puede encontrar este módulo en la categoría **Funciones estadísticas** del diseñador.

1. Seleccione el conjunto de datos para el que desea generar un informe.

    Si desea notificar solo algunas columnas, use el módulo [Select Columns in Dataset](select-columns-in-dataset.md) (Seleccionar columnas del conjunto de datos) para proyectar un subconjunto de columnas con el que trabajar.

1. No hay ningún otro parámetro obligatorio. De forma predeterminada, el módulo analiza todas las columnas que se proporcionan como entrada y, en función del tipo de valores de las columnas, genera un conjunto de estadísticas pertinente, tal como se describe en la sección [Resultados](#results).

1. Ejecución de la canalización

## <a name="results"></a>Results

El informe del módulo puede incluir las siguientes estadísticas. 

|Nombre de la columna|Descripción|
|------|------|  
|**Característica**|Nombre de la columna|
|**Recuento**|Recuento de filas|
|**Número de valores únicos**|Recuento de valores únicos en la columna|
|**Missing Value Count** (Número de valores ausentes)|Recuento de valores únicos en la columna|
|**Mín.**|Valor más bajo de la columna|  
|**Máx.**|Valor más alto de la columna|
|**Promedio**|Promedio de todos los valores de la columna|
|**Mean Deviation** (Desviación media)|Desviación media de los valores de la columna|
|**1st Quartile** (1er cuartil)|Valor en el primer cuartil|
|**Mediana**|Valor de la mediana de la columna|
|**3rd Quartile** (3er cuartil)|Valor en el tercer cuartil|
|**Modo**|Modo de los valores de la columna|
|**Range**|Entero que representa el número de valores entre los valores máximo y mínimo|
|**Sample Variance**  (Varianza de la muestra)|Varianza de la columna; consulte la nota|
|**Sample Standard Deviation** (Desviación típica de la muestra)|Desviación típica de la columna; consulte la nota|
|**Sample Skewness** (Sesgo de la muestra)|Sesgo de la columna; consulte la nota|
|**Sample Kurtosis** (Curtosis de la muestra)|Curtosis de la columna; consulte la nota|
|**P0.5**|Percentil 0,5 %|
|**P1**|Percentil 1 %|
|**P5**|Percentil 5 %|
|**P95**|Percentil 95 %|
|**P99.5**|Percentil 99,5 % |

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning.  
