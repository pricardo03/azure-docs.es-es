---
title: Transformación de receptor en el flujo de datos de asignación
description: Aprenda a configurar una transformación de receptor en el flujo de datos de asignación.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: 3b631c068d1a444691345e054219208c4c8b0b8c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020053"
---
# <a name="sink-transformation-in-mapping-data-flow"></a>Transformación de receptor en el flujo de datos de asignación

Una vez que haya transformado los datos, puede recibir los datos en un conjunto de datos de destino. Cada flujo de datos requiere al menos una transformación de receptor, pero puede agregar tantos receptores como sea necesario para completar el flujo de transformación. Para escribir en receptores adicionales, cree nuevas secuencias a través de nuevas ramas y divisiones condicionales.

Cada transformación de receptor se asocia exactamente con un conjunto de datos de Data Factory. El conjunto de datos define la forma y la ubicación de los datos en que quiere escribir.

## <a name="supported-sink-connectors-in-mapping-data-flow"></a>Conectores de receptor compatibles en el flujo de datos de asignación

Actualmente, se pueden usar los siguientes conjuntos de datos en una transformación de receptor:
    
* [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL Database](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

La configuración específica de estos conectores se encuentra en la pestaña **Configuración**. La información sobre esta configuración se encuentra en la documentación del conector. 

Azure Data Factory tiene acceso a más de [90 conectores nativos](connector-overview.md). Para escribir datos en esos otros orígenes del flujo de datos, use la herramienta de actividad de copia para cargar esos datos desde una de las áreas de almacenamiento provisional compatibles tras finalizar el flujo de datos.

## <a name="sink-settings"></a>Configuración del receptor

Una vez que haya agregado un receptor, configúrelo a través de la pestaña **Receptor**. Aquí puede elegir o crear el conjunto de datos en el que escibe el receptor. 

![Configuración del receptor](media/data-flow/sink-settings.png "Configuración del receptor")

**Desfase de esquema:** El [desfase de esquema](concepts-data-flow-schema-drift.md) es la capacidad de Data Factory de administrar de forma nativa los esquemas flexibles en los flujos de datos sin necesidad de definir explícitamente los cambios en las columnas. Habilite **Permitir el desfase de esquema** para escribir columnas adicionales sobre lo que se define en el esquema de datos del receptor.

**Validate schema:** (Validar esquema) Si Validar esquema está seleccionado, el flujo de datos dará error si no se encuentra ninguna columna del esquema definido del conjunto de datos.

## <a name="field-mapping"></a>Asignación de campos

De forma similar a la transformación Selección, en la pestaña **Asignación** del receptor, puede decidir qué columnas de entrada se escribirán. De forma predeterminada, se asignan todas las columnas de entrada, incluidas las columnas desfasadas. Esto se conoce como **registro automático**.

Cuando desactive la asignación automática, tendrá la opción de agregar asignaciones basadas en columnas o basadas en reglas. Las asignaciones basada en reglas le permiten escribir expresiones con coincidencia de patrones, mientras que la asignación fija asignará nombres de columna lógicos y físicos. Para más información sobre la asignación basada en reglas, consulte [Patrones de columna en flujos de datos de asignación](concepts-data-flow-column-pattern.md#rule-based-mapping-in-select-and-sink).

## <a name="custom-sink-ordering"></a>Ordenación de receptores personalizados

De forma predeterminada, los datos se escriben en varios receptores en un orden no determinista. El motor de ejecución escribirá datos en paralelo a medida que se complete la lógica de transformación y el orden de los receptores puede variar en cada ejecución. Para especificar una ordenación de receptores exacta, habilite la opción **Custom sink ordering** (Ordenación de receptores personalizada) en la pestaña General del flujo de datos. Una vez habilitada, los receptores se escribirán secuencialmente en orden ascendente.

![Ordenación de receptores personalizada](media/data-flow/custom-sink-ordering.png "Ordenación de receptores personalizados")

## <a name="data-preview-in-sink"></a>Vista previa de los datos en el receptor

Al obtener una vista previa de los datos en un clúster de depuración, no se escribirá ningún dato en el receptor. Se devolverá una instantánea de la apariencia de los datos, pero no se escribirá nada en el destino. Para probar la escritura de datos en el receptor, ejecute una depuración de canalización desde el lienzo de canalización.

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha creado el flujo de datos, agregue una [actividad de Data Flow a la canalización](concepts-data-flow-overview.md).
