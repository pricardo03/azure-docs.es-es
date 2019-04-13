---
title: Desfase de esquema de flujos de datos de asignación de Azure Data Factory
description: Creación de flujos de datos resistentes en Azure Data Factory con desfase de esquema
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: aadab68185347dc0a12e0802f675efe13ecea545
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59547151"
---
# <a name="mapping-data-flow-schema-drift"></a>Desfase del esquema de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

El concepto de desfase de esquema es el caso en que los orígenes suelen cambiar los metadatos. Los campos, columnas, tipos, etc., pueden agregarse, quitarse o cambiarse sobre la marcha. Si no se controla el desfase de esquema, el flujo de datos se vuelve vulnerable a los cambios del origen de datos de nivel superior. Cuando se cambian los campos y las columnas de entrada, los patrones ETL típicos generan un error porque tienden a estar vinculados a los nombres de esos orígenes.

Con el fin de protegerse contra el desfase de esquema, es importante disponer de las funciones en la herramienta Data Flow que le permite, como ingeniero de datos, hacer lo siguiente:

* Definir orígenes que tienen nombres de campo, tipos de datos, valores y tamaños mutables
* Definir parámetros de transformación que pueden funcionar con modelos de datos en lugar de con campos y valores codificados de forma rígida
* Definir expresiones que comprenden los patrones para que coincidan con los campos de entrada, en lugar de utilizar campos con nombre

## <a name="how-to-implement-schema-drift"></a>Cómo implementar la desviación en el esquema

* Elija "Allow Schema Drift" (Permitir desfase de esquema) en la transformación de origen.

<img src="media/data-flow/schemadrift001.png" width="400">

* Cuando haya seleccionado esta opción, todos los campos de entrada se leerán desde el origen de cada ejecución de flujo de datos y se pasará a través del flujo completo al receptor.

* Asegúrese de usar "Asignación automática" para asignar todos los nuevos campos en la transformación de receptor con el fin de que todos los nuevos campos se seleccionen y coloquen en el destino:

<img src="media/data-flow/automap.png" width="400">

* Todo funcionará correctamente cuando se introducen nuevos campos en ese escenario con una sencilla asignación origen -> receptor (también conocida como copia).

* Para agregar las transformaciones de flujo de trabajo que controla el desfase de esquema, puede usar la coincidencia de patrones para hacer coincidir las columnas por nombre, tipo y valor.

* Haga clic en "Add Column Pattern" (Agregar patrón de columna) en la columna derivada o la transformación agregada si desea crear una transformación que tenga en cuenta el desfase de esquema.

<img src="media/data-flow/columnpattern.png" width="400">

> [!NOTE]
> Deberá tomar una decisión de arquitectura en el flujo de datos para aceptar el desfase de esquema en todo el flujo. Al hacerlo, puede protegerse de los cambios de esquema en los orígenes. Sin embargo, se perderá el enlace temprano de las columnas y los tipos a lo largo del flujo de datos. Azure Data Factory trata los flujos de desfase de esquema como flujos de enlace tardío, por lo que, al compilar las transformaciones, los nombres de columna no estarán disponibles en las vistas de esquema en todo el flujo.

<img src="media/data-flow/taxidrift1.png" width="400">

En el flujo de datos de ejemplo Taxi Demo, hay un desfase de esquema de ejemplo en el flujo de datos inferior con el origen TripFare. En la transformación agregada, tenga en cuenta que estamos usando el diseño de patrón de columnas para los campos de agregación. En lugar de asignar nombres a columnas específicas, o de buscar columnas por posición, daremos por hecho que los datos se pueden cambiar y no pueden aparecer en el mismo orden entre ejecuciones.

En este ejemplo de control de desfase de esquema de Data Flow de Azure Data Factory, hemos creado y agregado los exámenes de las columnas de tipo "double", sabiendo que el dominio de datos contiene los precios por cada carrera. A continuación, se puede realizar un cálculo matemático agregado en todos los campos "double" en el origen, independientemente de donde se coloque la columna y de la nomenclatura de la columna.

La sintaxis de Data Flow de Azure Data Factory usa $$ para representar cada columna coincidente del patrón de coincidencia. También puede hacer coincidir los nombres de las columnas utilizando funciones complejas de búsqueda de cadenas y expresiones regulares. En este caso, vamos a crear un nuevo nombre de campo agregados en función de cada coincidencia de un tipo de columna "double" y agregar el texto ```_total``` a cada uno de esos coincidieron con nombres: 

```concat($$, '_total')```

A continuación, redondearemos y sumaremos los valores para cada una de esas columnas coincidentes:

```round(sum ($$))```

Puede probarlo con el ejemplo de flujo de datos de Azure Data Factory "Taxi Demo". Active la sesión de depuración con el botón de alternancia Depurar en la parte superior de la superficie de diseño de Data Flow para que pueda ver los resultados de forma interactiva:

<img src="media/data-flow/taxidrift2.png" width="800">

## <a name="access-new-columns-downstream"></a>Acceso a nuevas columnas de nivel inferior

Al generar nuevas columnas con los patrones de columnas, puede acceder a esas columnas nuevo más tarde en sus transformaciones de flujo de datos mediante la función de la expresión "byName".

## <a name="next-steps"></a>Pasos siguientes

En el [lenguaje de expresiones de flujo de datos](data-flow-expression-functions.md) encontrará características adicionales para patrones de columnas y la desviación en el esquema incluido "byName" y "byPosition".
