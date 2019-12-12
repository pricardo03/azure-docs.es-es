---
title: Transformación Agregar en Asignación de Data Flow
description: Obtenga información sobre cómo agregar datos a escala en Azure Data Factory con la transformación Agregar en Asignación de Data Flow.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/15/2019
ms.openlocfilehash: 74b96bf2cac0de7c57e496c637f2e3ef549eb61f
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930452"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Transformación Agregar en Asignación de Data Flow 

En la transformación Agregar se definen las agregaciones de columnas de flujos de datos. Mediante el Generador de expresiones, puede definir diferentes tipos de agregaciones, tales como SUM, MIN, MAX y COUNT agrupados por columnas calculadas o existentes.

## <a name="group-by"></a>Agrupar por

Seleccione una columna existente o cree una nueva columna calculada para usarla como una cláusula Agrupar por para la agregación. Para usar una columna existente, selecciónela en la lista desplegable. Para crear una nueva columna calculada, mantenga el mouse sobre la cláusula y haga clic en **Columna calculada**. Se abrirá el [generador de expresiones de flujo de datos](concepts-data-flow-expression-builder.md). Una vez creada la columna calculada, escriba el nombre de la columna de salida en el campo **Name as** (Nombre como). Si desea agregar una cláusula Agrupar por adicional, mantenga el mouse sobre una cláusula existente y haga clic en el icono "más".

![Configuración de Agrupar por de una transformación Agregar](media/data-flow/agg.png "Configuración de Agrupar por de una transformación Agregar")

Una cláusula Agrupar por es opcional en una transformación Agregar.

## <a name="aggregate-column"></a>Columna de agregación 

Vaya a la pestaña **Agregados** para generar las expresiones de agregación. Puede sobrescribir una columna existente con una agregación, o bien crear un campo nuevo con un nombre nuevo. La expresión de agregación se escribirá en el cuadro de la derecha, junto al selector de nombre de columna. Para editar la expresión, haga clic en el cuadro de texto para abrir el Generador de expresiones. Para incorporar una agregación adicional, mantenga el mouse sobre una expresión existente y haga clic en icono "más" para crear una nueva columna de agregación o un [patrón de columnas](concepts-data-flow-column-pattern.md).

Cada expresión de agregación debe contener al menos una función de agregado.

![Configuración de agregado de la transformación Agregar](media/data-flow/agg2.png "Configuración de agregado de la transformación Agregar")


> [!NOTE]
> En el modo Depurar, el Generador de expresiones no puede generar vistas previas de los datos con las funciones de agregado. Para ver las vistas previas de los datos para las transformaciones Agregar, cierre el Generador de expresiones y vea los datos a través de la pestaña "Vista previa de los datos".

## <a name="reconnect-rows-and-columns"></a>Reconexión de filas y columnas

Las transformaciones Agregar son similares a las consultas de selección de agregado de SQL. Las columnas que no estén incluidas en la cláusula Agrupar por o en las funciones de agregado no fluirán a través de la salida de la transformación Agregar. Si desea incluir otras columnas en la salida agregada, siga uno de estos métodos:

* Use una función de agregado como `last()` o `first()` para incluir esa columna adicional.
* Vuelva a unir las columnas al flujo de salida con el [patrón self join](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/).

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream>
    aggregate(
           groupBy(
                <groupByColumnName> = <groupByExpression1>,
                <groupByExpression2>
               ),
           <aggregateColumn1> = <aggregateExpression1>,
           <aggregateColumn2> = <aggregateExpression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <aggregateTransformationName>
```

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se toma un flujo entrante `MoviesYear` y las filas se agrupan por columna `year`. La transformación crea una columna de agregado `avgrating` que se evalúa como el promedio de la columna `Rating`. Esta transformación Agregar se denomina `AvgComedyRatingsByYear`.

En la experiencia de usuario de Data Factory, esta transformación es similar a la siguiente imagen:

![Ejemplo de Agrupar por](media/data-flow/agg-script1.png "Ejemplo de Agrupar por")

![Ejemplo de Agregar](media/data-flow/agg-script2.png "Ejemplo de Agregar")

En el siguiente fragmento de código se muestra el script del flujo de datos para esta transformación.

```
MoviesYear aggregate(
                groupBy(year),
                avgrating = avg(toInteger(Rating))
            ) ~> AvgComedyRatingByYear
```

## <a name="next-steps"></a>Pasos siguientes

* Definir un agregación basada en ventanas mediante la [transformación Ventana](data-flow-window.md).
