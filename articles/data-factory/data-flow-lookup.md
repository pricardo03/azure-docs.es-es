---
title: Transformación Búsqueda del flujo de datos de asignación de Azure Data Factory
description: Transformación Búsqueda del flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/03/2019
ms.openlocfilehash: 01a1dba18e21a38695146560bbf85cf1a042ba02
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72596562"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Transformación Búsqueda del flujo de datos de asignación de Azure Data Factory

Use Búsqueda para agregar datos de referencia desde otro origen al flujo de datos. La transformación Búsqueda requiere un origen definido que apunte a la tabla de referencia y busque coincidencias por los campos de clave.

![Transformación Búsqueda](media/data-flow/lookup1.png "Búsqueda")

Seleccione los campos de clave por los que desea buscar coincidencias entre los campos de la secuencia entrante y los campos del origen de referencia. Tiene que haber creado un origen nuevo en el lienzo de diseño de Data Flow para usarlo como lado derecho de la búsqueda.

Cuando se encuentran coincidencias, las filas y columnas resultantes del origen de referencia se agregarán al flujo de datos. Puede elegir qué campos de interés desea incluir en el receptor al final de su flujo de datos. O bien, puede usar una transformación Seleccionar después de Búsqueda para realizar una eliminación en la lista de campos y conservar solo los campos de las dos secuencias que desee.

La transformación Búsqueda realiza el equivalente de una combinación externa izquierda. Por lo tanto, verá que todas las filas del origen izquierdo se combinan con las coincidencias del lado derecho. Si tiene varios valores coincidentes en la búsqueda, o si desea personalizar la expresión de búsqueda, es preferible cambiar a una transformación Combinación y usar una combinación cruzada. Esto evitará cualquier posible error de producto cartesiano en la ejecución.

## <a name="match--no-match"></a>Coincidente o no coincidente

Después de la transformación Búsqueda, puede usar las transformaciones posteriores para inspeccionar los resultados de cada fila coincidente mediante la función de expresión `isMatch()` para tomar más decisiones sobre la lógica en función de si la búsqueda dio como resultado una coincidencia o no.

![Patrón de búsqueda](media/data-flow/lookup111.png "Patrón de búsqueda")

Después de usar la transformación Búsqueda, puede agregar una división de transformación del tipo División condicional en la función ```isMatch()```. En el ejemplo anterior, las filas coincidentes pasan por el flujo anterior y las filas no coincidentes fluyen a través del flujo de ```NoMatch```.

## <a name="first-or-last-value"></a>Primer o último valor

Si la Búsqueda devuelve varias coincidencias, puede que quiera reducir el número de filas coincidentes mediante la selección de la primera o la última coincidencia. Puede hacerlo mediante una transformación Agregar después de la de Búsqueda.

En este caso, se usa una transformación Agregar llamada ```PickFirst``` para elegir el primer valor de entre las coincidencias de búsqueda.

![Agregar después de Búsqueda](media/data-flow/lookup333.png "Agregar después de Búsqueda")

![Primer valor de búsqueda](media/data-flow/lookup444.png "Primer valor de Búsqueda")

## <a name="optimizations"></a>Optimizaciones

En Data Factory, Data Flow se ejecuta en entornos de Spark de escalabilidad horizontal. Si el conjunto de datos puede caber en el espacio de memoria de un nodo de trabajo, podemos optimizar el rendimiento de Búsqueda.

![Combinación de difusión](media/data-flow/broadcast.png "Combinación de difusión")

### <a name="broadcast-join"></a>Unión de difusión

Seleccione la unión de difusión del lado izquierdo o derecho para solicitar a ADF que inserte todo el conjunto de datos de cualquiera de los dos lados de la relación de Búsqueda en la memoria. En los conjuntos de valores más pequeños, esto puede mejorar considerablemente el rendimiento de la búsqueda.

### <a name="data-partitioning"></a>Creación de particiones de datos

También puede especificar la creación de particiones de los datos si selecciona "Set Partitioning" (establecer particiones) en la pestaña Optimizar de la transformación Búsqueda para crear conjuntos de datos que pueden adaptarse mejor a la memoria por trabajo.

## <a name="next-steps"></a>Pasos siguientes

* Las transformaciones [Combinación](data-flow-join.md) y [Existe](data-flow-exists.md) realizan tareas similares en los flujos de datos de asignación de ADF. A continuación, eche un vistazo a esas transformaciones.
* Use una [División condicional](data-flow-conditional-split.md) con ```isMatch()``` para dividir las filas en valores coincidentes y no coincidentes.
