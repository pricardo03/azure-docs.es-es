---
title: Guía de asignación de flujo de datos de rendimiento y optimización en Azure Data Factory | Microsoft Docs
description: Obtenga información sobre los factores clave que afectan al rendimiento de los flujos de datos en Azure Data Factory cuando se usa la asignación de los flujos de datos.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: 7fca586083f70e0b0f7e593d5203392260cd2136
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172346"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Rendimiento de flujos de datos de asignación y la Guía de optimización

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory asignación fluyen los datos proporcionan una interfaz de explorador sin código para diseñar, implementar y orquestar las transformaciones de datos a escala.

> [!NOTE]
> Si no está familiarizado con flujos de datos de asignación de ADF en general, consulte [información general de flujos de datos](concepts-data-flow-overview.md) antes de leer este artículo.
>

> [!NOTE]
> Al diseñar y probar los flujos de datos de la UI de ADF, asegúrese de activar el conmutador de depuración para que puedan ejecutar los flujos de datos en tiempo real sin esperar a un clúster de preparación.
>

![Depurar botón](media/data-flow/debugb1.png "depurar")

## <a name="optimizing-for-azure-sql-database"></a>Optimización de base de datos SQL Azure

![Parte del origen](media/data-flow/sourcepart2.png "parte de origen")

### <a name="you-can-match-spark-data-partitioning-to-your-source-database-partitioning-based-on-a-database-table-column-key-in-the-source-transformation"></a>Puede hacer coincidir las particiones de datos para el particionamiento de base de datos de origen según una clave de columna de tabla de base de datos en la transformación de código fuente de Spark

* Vaya a "Optimizar" y seleccione "Origen". Establecer una columna de una tabla específica o un tipo en una consulta.
* Si eligió "columna", a continuación, seleccione la columna de partición.
* Además, Establece el número máximo de conexiones en Azure SQL DB. Puede probar un valor superior para obtener las conexiones en paralelo a la base de datos. Sin embargo, algunos casos pueden provocar un rendimiento más rápido con un número limitado de conexiones.

### <a name="set-batch-size-and-query-on-source"></a>Establecer tamaño del lote y consulta de origen

![Origen](media/data-flow/source4.png "origen")

* Establecer el tamaño de lote indicará ADF para almacenar datos en conjuntos en memoria en lugar de la fila a fila. Es una configuración opcional y puede quedarse sin recursos en los nodos de proceso si no se tamaño correctamente.
* Configuración de una consulta puede le permiten filtrar derecha de las filas en el origen antes de que lleguen incluso para el flujo de datos para su procesamiento, que puede hacer que la adquisición de datos inicial más rápida.
* Si usa una consulta, puede agregar sugerencias de consulta opcional para Azure SQL DB, es decir, READ UNCOMMITTED

### <a name="set-sink-batch-size"></a>Establezca el tamaño del lote de receptor

![Receptor](media/data-flow/sink4.png "receptor")

* Con el fin de evitar el procesamiento fila por fila de sus témpanos de datos, establezca el "tamaño del lote" en la configuración del receptor de Azure SQL DB. Esto indicará a ADF para procesar base de datos que se escribe en lotes basándose en el tamaño especificado.

### <a name="set-partitioning-options-on-your-sink"></a>Establecer opciones en el receptor de particionamiento

* Incluso si no tiene los datos con particiones en las tablas de base de datos de SQL Azure de destino, vaya a la ficha optimizar y particiones del conjunto.
* Muy a menudo, simplemente indicando ADF se utiliza Round Robin, creación de particiones en los clústeres de ejecución de Spark resultados en la carga en lugar de obligarle a todas las conexiones desde una sola partición o nodo de datos mucho más rápidos.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Aumente el tamaño de su motor de proceso en tiempo de ejecución de integración de Azure

![Nueva instancia de IR](media/data-flow/ir-new.png "IR nuevo")

* Aumentar el número de núcleos, lo que aumentará el número de nodos y proporcionarle más capacidad de procesamiento para consultar y escribir en la base de datos de SQL Azure.
* Pruebe las opciones de "Proceso optimizado" y "Con optimización para memoria" para aplicar más recursos a los nodos de proceso.

### <a name="disable-indexes-on-write"></a>Deshabilitar índices durante la escritura
* Usar una actividad de procedimiento almacenado de canalización ADF antes de la actividad de flujo de datos que deshabilita los índices en las tablas de destino que se escriben en el receptor.
* Después de la actividad de flujo de datos, agregue otra actividad de procedimiento almacenado que habilitar estos índices.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Aumentar el tamaño de la base de datos de SQL Azure
* Programar un cambio de tamaño de su origen y receptor de Azure SQL DB antes de la serie de límites de la canalización para aumentar el rendimiento y minimizar la limitación de Azure una vez que alcance DTU.
* Una vez completada la ejecución de la canalización, puede cambiar el tamaño de las bases de datos a la velocidad de ejecución normal.

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos de flujo de datos:

- [Información general sobre el flujo de datos](concepts-data-flow-overview.md)
- [Actividad de flujo de datos](control-flow-execute-data-flow-activity.md)

