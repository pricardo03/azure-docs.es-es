---
title: Transformación Búsqueda de Azure Data Factory Mapping Data Flow
description: Transformación Búsqueda de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 197f5ba9d6921f4a9921b7074b9e05162d3e37b8
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64868120"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Transformación Búsqueda de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Use Búsqueda para agregar datos de referencia desde otro origen al flujo de datos. La transformación Búsqueda requiere un origen definido que apunte a la tabla de referencia y busque coincidencias por los campos de clave.

![Transformación Búsqueda](media/data-flow/lookup1.png "Lookup")

Seleccione los campos de clave por los que desea buscar coincidencias entre los campos de la secuencia entrante y los campos del origen de referencia. Tiene que haber creado un origen nuevo en el lienzo de diseño de Data Flow para usarlo como lado derecho de la búsqueda.

Cuando se encuentran coincidencias, las filas y columnas resultantes del origen de referencia se agregarán al flujo de datos. Puede elegir qué campos de interés desea incluir en el receptor al final de su flujo de datos.

## <a name="match--no-match"></a>Coincide con o sin coincidencia

Después de la transformación búsqueda, puede usar las transformaciones posteriores para inspeccionar los resultados de cada fila de la coincidencia con la función de la expresión `isMatch()` tomar más decisiones en su lógica en función de si la búsqueda dieron lugar a una coincidencia de fila o no.

## <a name="optimizations"></a>Optimizaciones

En Data Factory, flujos de datos se ejecute en entornos de Spark de escalado horizontal. Si el conjunto de datos puede caber en el espacio de memoria de nodo de trabajo, podemos optimizar el rendimiento de la búsqueda.

![Combinación de difusión](media/data-flow/broadcast.png "combinación de difusión")

### <a name="broadcast-join"></a>Combinación de difusión

Seleccionar izquierda o derecha difundir combinación para solicitar el ADF para insertar todo el conjunto de datos de ambos lados de la relación de búsqueda en memoria.

### <a name="data-partitioning"></a>Creación de particiones de datos

También puede especificar la creación de particiones de los datos mediante la selección de "Particiones del conjunto de" en la pestaña de la optimización de la transformación búsqueda para crear conjuntos de datos que pueden adaptarse mejor a la memoria por trabajo.

## <a name="next-steps"></a>Pasos siguientes

[Únase a](data-flow-join.md) y [Exists](data-flow-exists.md) transformaciones realizan tareas similares en flujos de datos de asignación de ADF. Eche un vistazo a esas transformaciones siguiente.
