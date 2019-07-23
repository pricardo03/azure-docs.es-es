---
title: Transformación Combinación en el flujo de datos de Azure Data Factory
description: Transformación Combinación en el flujo de datos de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/07/2019
ms.openlocfilehash: 18f713198ef9aa45cb72a6718c0f7b086c019258
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61348582"
---
# <a name="mapping-data-flow-join-transformation"></a>Transformación Combinación de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Use Combinación para combinar datos de dos tablas en su flujo de datos. Haga clic en la transformación que será la relación de la izquierda y agregue una transformación Combinación en el cuadro de herramientas. Dentro de la transformación Combinación, seleccionará otro flujo de datos para que sea la relación adecuada.

![Transformación Combinación](media/data-flow/join.png "Combinación")

## <a name="join-types"></a>Tipos de combinación

La selección de un tipo de combinación es necesaria para la transformación Combinación.

### <a name="inner-join"></a>Combinación interna

La combinación interna pasará solo a través de las filas que cumplen las condiciones de columna de ambas tablas.

### <a name="left-outer"></a>Externa izquierda

Se pasan todas las filas de la secuencia izquierda que no cumplan la condición de combinación, y las columnas de salida de la otra tabla se establecen en NULL, además de todas las filas devueltas por la combinación interna.

### <a name="right-outer"></a>Externa derecha

Se pasan todas las filas de la secuencia derecha que no cumplan la condición de combinación, y las columnas de salida que correspondan a la otra tabla se establecen en NULL, además de todas las filas devueltas por la combinación interna.

### <a name="full-outer"></a>Externa completa

Externa completa genera todas las columnas y filas de ambos lados con valores NULL para las columnas que no están presentes en la otra tabla.

### <a name="cross-join"></a>Combinación cruzada

Especifica el producto cruzado de las dos secuencias con una expresión. Se puede usar para crear condiciones de combinación personalizadas.

## <a name="specify-join-conditions"></a>Especificar condiciones de combinación

La condición de combinación izquierda procede del flujo de datos conectado a la izquierda de su combinación. La condición de combinación derecha es la segunda secuencia de datos conectada a la combinación en la parte inferior, que será un conector directo a otra secuencia o una referencia a otra secuencia.

Es necesario que escriba al menos 1 (1..n) condiciones de combinación. Pueden ser campos a los que se hace referencia directamente, seleccionados desde el menú desplegable, o expresiones.

## <a name="join-performance-optimizations"></a>Optimizaciones de rendimiento de combinación

A diferencia de la unión de combinación en herramientas como SSIS, la combinación en ADF Data Flow no es una operación de unión de combinación obligatoria. Por lo tanto, no es necesario que las claves de combinación se ordenen primero. La operación de combinación se producirá en Spark con Databricks basándose en la operación de combinación óptima de Spark: Combinación de difusión y del lado del asignación:

![Optimización de la transformación Combinación](media/data-flow/joinoptimize.png "Optimización de Combinación")

Si el conjunto de datos puede caber en la memoria de nodo de trabajo de Databricks, podemos optimizar el rendimiento de Combinación. También puede especificar la creación de particiones de los datos en la operación de combinación para crear conjuntos de datos que pueden adaptarse mejor a la memoria por trabajo.

## <a name="self-join"></a>Autocombinación

Puede lograr condiciones de autocombinación en ADF Data Flow mediante la transformación de selección para asignar un alias a una secuencia existente. En primer lugar, cree una "nueva rama" desde una secuencia y, a continuación, agregue una instrucción de selección para asignar un alias a la secuencia original completa.

![Autocombinación](media/data-flow/selfjoin.png "Autocombinación")

En el diagrama anterior, la transformación Selección se encuentra en la parte superior. Todo lo que está haciendo es asignar a la secuencia original el alias "OrigSourceBatting". En la transformación Combinación resaltada a continuación, puede ver que usamos esta secuencia de alias de selección como combinación de la derecha, lo que nos permite hacer referencia a la misma clave tanto en el lado derecho como el izquierdo de la combinación interna.

## <a name="composite-and-custom-keys"></a>Claves compuestas y personalizadas

Puede crear claves compuestas y personalizadas sobre la marcha en la transformación Combinación. Agregue filas para las columnas de combinación adicionales con el signo más (+) situado junto a cada fila de la relación. O bien calcule un nuevo valor de clave en el Generador de expresiones para obtener un valor de combinación sobre la marcha.

## <a name="next-steps"></a>Pasos siguientes

Después de combinar datos, puede [crear nuevas columnas](data-flow-derived-column.md) y [recibir sus datos en un almacén de datos de destino](data-flow-sink.md).
