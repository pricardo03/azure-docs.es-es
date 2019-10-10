---
title: Transformación Búsqueda de Azure Data Factory Mapping Data Flow
description: Transformación Búsqueda de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: ef72b7aed12afd1cee47b11bc7584d1e53bf2af5
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029346"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Transformación Búsqueda de Azure Data Factory Mapping Data Flow



Use Búsqueda para agregar datos de referencia desde otro origen al flujo de datos. La transformación Búsqueda requiere un origen definido que apunte a la tabla de referencia y busque coincidencias por los campos de clave.

![Transformación Búsqueda](media/data-flow/lookup1.png "Lookup")

Seleccione los campos de clave por los que desea buscar coincidencias entre los campos de la secuencia entrante y los campos del origen de referencia. Tiene que haber creado un origen nuevo en el lienzo de diseño de Data Flow para usarlo como lado derecho de la búsqueda.

Cuando se encuentran coincidencias, las filas y columnas resultantes del origen de referencia se agregarán al flujo de datos. Puede elegir qué campos de interés desea incluir en el receptor al final de su flujo de datos.

## <a name="match--no-match"></a>Coincidente o no coincidente

Después de la transformación Búsqueda, puede usar las transformaciones posteriores para inspeccionar los resultados de cada fila coincidente mediante la función de expresión `isMatch()` para tomar más decisiones sobre la lógica en función de si la búsqueda dio como resultado una coincidencia o no.

## <a name="optimizations"></a>Optimizaciones

En Data Factory, Data Flow se ejecuta en entornos de Spark de escalabilidad horizontal. Si el conjunto de datos puede caber en el espacio de memoria de un nodo de trabajo, podemos optimizar el rendimiento de Búsqueda.

![Unión de difusión](media/data-flow/broadcast.png " de difusión")

### <a name="broadcast-join"></a>Unión de difusión

Seleccione la unión de difusión del lado izquierdo o derecho para solicitar a ADF que inserte todo el conjunto de datos de cualquiera de los dos lados de la relación de Búsqueda en la memoria.

### <a name="data-partitioning"></a>Creación de particiones de datos

También puede especificar la creación de particiones de los datos si selecciona "Set Partitioning" (establecer particiones) en la pestaña Optimizar de la transformación Búsqueda para crear conjuntos de datos que pueden adaptarse mejor a la memoria por trabajo.

## <a name="next-steps"></a>Pasos siguientes

Las transformaciones [Unión](data-flow-join.md) y [Existe](data-flow-exists.md) realizan tareas similares en los flujos de datos de asignación de ADF. A continuación, eche un vistazo a esas transformaciones.
