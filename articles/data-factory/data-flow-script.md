---
title: Script de flujo de datos de asignación de Azure Data Factory
description: Información general sobre el lenguaje de código subyacente de un script de flujo de datos de Data Factory
author: kromerm
ms.author: nimoolen
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/10/2019
ms.openlocfilehash: 4ff5a05fd40ef086c1f2332443ca03d5e872e9a8
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74010153"
---
# <a name="data-flow-script-dfs"></a>Script de flujo de datos (DFS)

El script de flujo de datos (DFS) son los metadatos subyacentes, similares a un lenguaje de codificación, que se usan para ejecutar las transformaciones que se incluyen en un flujo de datos de asignación. Cada transformación se representa mediante una serie de propiedades que proporcionan la información necesaria para ejecutar el trabajo correctamente. El script se puede ver y editar en ADF haciendo clic en el botón "Script" de la cinta de opciones superior de la interfaz de usuario del explorador.

![Botón Script](media/data-flow/scriptbutton.png "Botón Script")

Por ejemplo, `allowSchemaDrift: true,` en una transformación de origen indica al servicio que incluya todas las columnas del conjunto de datos de origen en el flujo de datos, incluso si no están incluidas en la proyección del esquema.

## <a name="use-cases"></a>Casos de uso
El DFS lo genera automáticamente la interfaz de usuario. Puede hacer clic en el botón Script para ver y personalizar el script. También puede generar scripts fuera de la interfaz de usuario de ADF y después pasarlos al cmdlet de PowerShell. Al depurar flujos de datos complejos, es posible que le resulte más fácil examinar el código subyacente del script en lugar de examinar la representación del gráfico de la interfaz de usuario de los flujos.

Estos son algunos casos de uso de ejemplo:
- Generación mediante programación de muchos flujos de datos que sean bastante similares, es decir, "sellar" flujos de trabajo.
- Expresiones complejas que son difíciles de administrar en la interfaz de usuario o que producen problemas de validación.
- Depuración y mejor reconocimiento de distintos errores devueltos durante la ejecución.

Al compilar un script de flujo de datos para usarlo con PowerShell o una API, debe contraer el texto con formato a una sola línea. Puede mantener las tabulaciones y las líneas nuevas como caracteres de escape. Pero se debe dar formato al texto para que quepa en una propiedad JSON. Hay un botón en la interfaz de usuario del editor de scripts en la parte inferior que dará formato al script como una sola línea.

![Botón Copiar](media/data-flow/copybutton.png "botón Copiar")

## <a name="how-to-add-transforms"></a>Incorporación de transformaciones
La incorporación de transformaciones requiere tres pasos básicos: agregar los datos de la transformación principal, volver a enrutar el flujo de entrada y, después, volver a enrutar el flujo de salida. Esto puede verse más fácilmente en un ejemplo.
Supongamos que empezamos con un origen sencillo para recibir un flujo de datos como el siguiente:

```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Si decidimos agregar una transformación derivada, primero es necesario crear el texto de transformación básico, que tiene una expresión simple para agregar una nueva columna en mayúsculas denominada `upperCaseTitle`:
```
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
```

A continuación, tomamos el DFS existente y agregamos la transformación:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Y ahora volvemos a enrutar el flujo entrante al identificar qué transformación queremos que sea la nueva transformación que venga después (en este caso, `source1`) y copiar el nombre del flujo en la nueva transformación:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Por último, identificamos la transformación que deseamos que venga después de esta nueva transformación y reemplazamos su flujo de entrada (en este caso, `sink1`) por el nombre del flujo de salida de la nueva transformación:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
deriveTransformationName sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="dfs-fundamentals"></a>Aspectos básicos de DFS
El DFS se compone de una serie de transformaciones conectadas, que incluyen orígenes, receptores y varios usuarios que pueden agregar nuevas columnas, filtrar datos, combinar datos y mucho más. Normalmente, el script comienza con uno o más orígenes seguidos de muchas transformaciones y termina con uno o más receptores.

Todos los orígenes tienen la misma construcción básica:
```
source(
  source properties
) ~> source_name
```

Por ejemplo, un origen simple con tres columnas (movieId, title y genres) sería el siguiente:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
```

Todas las transformaciones que no sean orígenes tienen la misma construcción básica:
```
name_of_incoming_stream transformation_type(
  properties
) ~> new_stream_name
```

Por ejemplo, una transformación simple derivada que toma una columna (title) y la sobrescribe con una versión en mayúsculas sería la siguiente:
```
source1 derive(
  title = upper(title)
) ~> derive1
```

Y un receptor sin esquema sería sencillamente:
```
derive1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="next-steps"></a>Pasos siguientes

Explore los flujos de datos comenzando por el [artículo de información general sobre los flujos de datos](concepts-data-flow-overview.md)
