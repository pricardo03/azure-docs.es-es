---
title: Desfase de esquema de flujos de datos de asignación de Azure Data Factory
description: Creación de flujos de datos resistentes en Azure Data Factory con desfase de esquema
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: b5777300f5033569caf3868218e747df3ff83a76
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640232"
---
# <a name="mapping-data-flow-schema-drift"></a>Desfase del esquema de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

El concepto de desfase de esquema es el caso en que los orígenes suelen cambiar los metadatos. Los campos, columnas, tipos, etc., pueden agregarse, quitarse o cambiarse sobre la marcha. Si no se controla el desfase de esquema, el flujo de datos se vuelve vulnerable a los cambios del origen de datos de nivel superior. Cuando se cambian los campos y las columnas de entrada, los patrones ETL típicos generan un error porque tienden a estar vinculados a los nombres de esos orígenes.

Con el fin de protegerse contra el desfase de esquema, es importante disponer de las funciones en la herramienta Data Flow que le permite, como ingeniero de datos, hacer lo siguiente:

* Definir orígenes que tienen nombres de campo, tipos de datos, valores y tamaños mutables
* Definir parámetros de transformación que pueden funcionar con modelos de datos en lugar de con campos y valores codificados de forma rígida
* Definir expresiones que comprenden los patrones para que coincidan con los campos de entrada, en lugar de utilizar campos con nombre

## <a name="how-to-implement-schema-drift-in-adf-mapping-data-flows"></a>Cómo implementar el desfase de esquema en Mapping Data Flows de ADF
ADF admite de forma nativa esquemas flexibles que cambian de una ejecución a otra, lo que le permite compilar lógica de transformación de datos genéricos sin necesidad de volver a compilar los flujos de datos.

* Elija "Allow Schema Drift" (Permitir desfase de esquema) en la transformación de origen.

<img src="media/data-flow/schemadrift001.png" width="400">

* Cuando haya seleccionado esta opción, todos los campos de entrada se leerán desde el origen de cada ejecución de flujo de datos y se pasará a través del flujo completo al receptor.

* Todas las columnas que se acaban de detectar (columnas desfasadas) llegarán como un tipo de datos String de forma predeterminada. En la transformación de origen, seleccione "Infer drifted column types" (Inferir los tipos de las columnas desfasadas) si quiere que ADF infiera automáticamente los tipos de datos del origen.

* Asegúrese de usar "Asignación automática" para asignar todos los nuevos campos en la transformación de receptor con el fin de que todos los nuevos campos se seleccionen y coloquen en el destino. Establezca también "Allow Schema Drift" (Permitir el desfase de esquema) en el receptor.

<img src="media/data-flow/automap.png" width="400">

* Todo funcionará correctamente cuando se introducen nuevos campos en ese escenario con una sencilla asignación origen -> receptor (copia).

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

Puede ver esta funcionalidad de desfase del esquema en acción con el ejemplo "Taxi Demo" de Azure Data Factory Data Flow. Active la sesión de depuración con el botón de alternancia Depurar en la parte superior de la superficie de diseño de Data Flow para que pueda ver los resultados de forma interactiva:

<img src="media/data-flow/taxidrift2.png" width="800">

## <a name="access-new-columns-downstream"></a>Acceso a nuevas columnas de nivel inferior
Al generar nuevas columnas con patrones de columnas, puede acceder a esas columnas nuevas más tarde en las transformaciones del flujo de datos con estos métodos:

* Use "byPosition" para identificar las nuevas columnas por número de posición.
* Use "byName" para identificar las nuevas columnas por nombre.
* En los patrones de columna, use "Name", "Stream", "Position", "Type" o cualquier combinación de estos para buscar coincidencias con nuevas columnas.

## <a name="rule-based-mapping"></a>Asignación basada en reglas
Las transformaciones Seleccionar y de receptor admiten la coincidencia de patrones mediante la asignación basada en reglas. Esto le permitirá compilar reglas que pueden asignar columnas desplazadas a los alias de columna y recibir esas columnas en el destino.

## <a name="next-steps"></a>Pasos siguientes
En el [lenguaje de expresiones de Data Flow](data-flow-expression-functions.md) encontrará características adicionales para patrones de columnas y desfase de esquema incluidas las funciones "byName" y "byPosition".
