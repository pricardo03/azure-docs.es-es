---
title: Transformación Columna derivada en Asignación de Data Flow de Azure Data Factory | Microsoft Docs
description: Aprenda a transformar datos a escala en Azure Data Factory con la transformación Columna derivada en Asignación de Data Flow.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 60451fa6152590ed0fde51be436c867f39906acf
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72514821"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Transformación Columna derivada en Asignación de Data Flow

Use la transformación Columna derivada para generar nuevas columnas en el flujo de datos o para modificar los campos existentes.

## <a name="derived-column-settings"></a>Configuración de columna derivada

Para reemplazar una columna existente, selecciónela mediante la lista desplegable de la columna. De lo contrario, utilice el campo de selección de columna como un cuadro de texto y escriba el nombre de la nueva columna. Para crear la expresión de la columna derivada, haga clic en el cuadro "Escribir expresión" para abrir el [Generador de expresiones de Data Flow](concepts-data-flow-expression-builder.md).

![Configuración de columna derivada](media/data-flow/dc1.png "Configuración de columna derivada")

Para agregar columnas derivadas adicionales, mantenga el mouse sobre una columna derivada existente y haga clic en el icono "más". Elija **Agregar columna** o **Add column pattern** (Agregar patrón de columna). Los patrones de columna pueden resultar útiles si los nombres de columna son variables de los orígenes. Para más información, consulte [Patrones de columna](concepts-data-flow-column-pattern.md).

![Nueva selección de columna derivada](media/data-flow/columnpattern.png "Nueva selección de columna derivada")

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream>
    derive(
           <columnName1> = <expression1>,
           <columnName2> = <expression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <deriveTransformationName>
```

### <a name="example"></a>Ejemplo

El ejemplo siguiente es una columna derivada denominada `CleanData` que toma un flujo entrante `MoviesYear` y crea dos columnas derivadas. La primera columna derivada reemplaza la columna `Rating` por el valor de clasificación como un tipo entero. La segunda columna derivada es un patrón que coincide con cada columna cuyo nombre empieza con "movies". Para cada columna coincidente, crea una columna `movie` que es igual al valor de la columna coincidente con el prefijo "movie_". 

En la experiencia de usuario de Data Factory, esta transformación es similar a la siguiente imagen:

![Ejemplo de derivación](media/data-flow/derive-script1.png "Ejemplo de derivación")

En el siguiente fragmento de código se muestra el script del flujo de datos para esta transformación:

```
MoviesYear derive(
                Rating = toInteger(Rating),
                each(
                    match(startsWith(name,'movies')),
                    'movie' = 'movie_' + toString($$)
                )
            ) ~> CleanData
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el [lenguaje de expresiones de Mapping Data Flow](data-flow-expression-functions.md).
