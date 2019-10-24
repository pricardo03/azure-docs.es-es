---
title: Transformación Búsqueda del flujo de datos de asignación de Azure Data Factory
description: Transformación Búsqueda del flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/03/2019
ms.openlocfilehash: 25d8588f8e2c968dc2516938263aaa7d6ddcff13
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387869"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Transformación Búsqueda del flujo de datos de asignación de Azure Data Factory

Use Búsqueda para agregar datos de referencia desde otro origen al flujo de datos. La transformación Búsqueda requiere un origen definido que apunte a la tabla de referencia y busque coincidencias por los campos de clave.

![Transformación Búsqueda](media/data-flow/lookup1.png "Búsqueda")

Seleccione los campos de clave por los que desea buscar coincidencias entre los campos de la secuencia entrante y los campos del origen de referencia. Tiene que haber creado un origen nuevo en el lienzo de diseño de Data Flow para usarlo como lado derecho de la búsqueda.

Cuando se encuentran coincidencias, las filas y columnas resultantes del origen de referencia se agregarán al flujo de datos. Puede elegir qué campos de interés desea incluir en el receptor al final de su flujo de datos. O bien, puede usar una transformación Seleccionar después de Búsqueda para realizar una eliminación en la lista de campos y conservar solo los campos de las dos secuencias que desee.

La transformación Búsqueda realiza el equivalente de una combinación externa izquierda. Por lo tanto, verá que todas las filas del origen izquierdo se combinan con las coincidencias del lado derecho. Si tiene varios valores coincidentes en la búsqueda, o si desea personalizar la expresión de búsqueda, es preferible cambiar a una transformación Combinación y usar una combinación cruzada. Esto evitará cualquier posible error de producto cartesiano en la ejecución.

## <a name="match--no-match"></a>Coincidente o no coincidente

Después de la transformación Búsqueda, puede usar las transformaciones posteriores para inspeccionar los resultados de cada fila coincidente mediante la función de expresión `isMatch()` para tomar más decisiones sobre la lógica en función de si la búsqueda dio como resultado una coincidencia o no.

## <a name="optimizations"></a>Optimizaciones

En Data Factory, Data Flow se ejecuta en entornos de Spark de escalabilidad horizontal. Si el conjunto de datos puede caber en el espacio de memoria de un nodo de trabajo, podemos optimizar el rendimiento de Búsqueda.

![Combinación de difusión](media/data-flow/broadcast.png "Combinación de difusión")

### <a name="broadcast-join"></a>Unión de difusión

Seleccione la unión de difusión del lado izquierdo o derecho para solicitar a ADF que inserte todo el conjunto de datos de cualquiera de los dos lados de la relación de Búsqueda en la memoria. En los conjuntos de valores más pequeños, esto puede mejorar considerablemente el rendimiento de la búsqueda.

### <a name="data-partitioning"></a>Creación de particiones de datos

También puede especificar la creación de particiones de los datos si selecciona "Set Partitioning" (establecer particiones) en la pestaña Optimizar de la transformación Búsqueda para crear conjuntos de datos que pueden adaptarse mejor a la memoria por trabajo.

## <a name="next-steps"></a>Pasos siguientes

Las transformaciones [Combinación](data-flow-join.md) y [Existe](data-flow-exists.md) realizan tareas similares en los flujos de datos de asignación de ADF. A continuación, eche un vistazo a esas transformaciones.
