---
title: Guía de asignación de flujo de datos de rendimiento y optimización en Azure Data Factory | Microsoft Docs
description: Obtenga información sobre los factores clave que afectan al rendimiento de los flujos de datos en Azure Data Factory cuando se usa la asignación de los flujos de datos.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: 46be01c57be0e4f5fa74f8e8b0d91db3d78f441c
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480417"
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

## <a name="monitor-data-flow-performance"></a>Supervisar el rendimiento de flujo de datos

Al diseñar los datos de asignación fluye en el explorador, se puede realizar pruebas unitarias cada transformación individuales haciendo clic en la pestaña de vista previa de datos en el panel de configuración de la parte inferior para cada transformación. Es el siguiente paso que debe seguir probar el flujo de datos al extremo en el Diseñador de canalizaciones. Agregar una actividad de ejecución de flujo de datos y utilice el botón de depuración para probar el rendimiento del flujo de datos. En el panel inferior de la ventana de la canalización, verá un icono anteojos en "acciones":

![Monitor de flujo de datos](media/data-flow/mon002.png "2 del Monitor de flujo de datos")

Al hacer clic en ese icono mostrará el plan de ejecución y el perfil de rendimiento subsiguientes del flujo de datos. Puede usar esta información para calcular el rendimiento del flujo de datos en orígenes de datos de tamaño diferentes. Tenga en cuenta que puede suponer un minuto de tiempo de configuración de ejecución de trabajos de clúster en los cálculos de rendimiento general y si está utilizando el valor predeterminado en tiempo de ejecución de integración de Azure, es posible que deberá agregar 5 minutos de tiempo de spin up clúster también.

![Supervisión del flujo de datos](media/data-flow/mon003.png "3 del Monitor de flujo de datos")

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Optimización de base de datos SQL Azure y Azure SQL Data Warehouse

![Parte del origen](media/data-flow/sourcepart2.png "parte de origen")

### <a name="partition-your-source-data"></a>Divida los datos de origen

* Vaya a "Optimizar" y seleccione "Origen". Establecer una columna de una tabla específica o un tipo en una consulta.
* Si eligió "columna", a continuación, seleccione la columna de partición.
* Además, Establece el número máximo de conexiones en Azure SQL DB. Puede probar un valor superior para obtener las conexiones en paralelo a la base de datos. Sin embargo, algunos casos pueden provocar un rendimiento más rápido con un número limitado de conexiones.
* Las tablas de base de datos de origen no es necesario crear particiones.
* Establecimiento de una consulta en la transformación de origen que coincida con el esquema de partición de la tabla de base de datos, permitirá que el motor de base de datos de origen aprovechar la eliminación de particiones.
* Si el origen ya no está particionado, ADF seguirá utilizando la creación de particiones en el entorno de transformación de Spark basado en la clave que seleccionó en la transformación del origen de datos.

### <a name="set-batch-size-and-query-on-source"></a>Establecer tamaño del lote y consulta de origen

![Origen](media/data-flow/source4.png "origen")

* Establecer el tamaño de lote indicará ADF para almacenar datos en conjuntos en memoria en lugar de la fila a fila. Es una configuración opcional y puede quedarse sin recursos en los nodos de proceso si no se tamaño correctamente.
* Configuración de una consulta puede le permiten filtrar derecha de las filas en el origen antes de que lleguen incluso para el flujo de datos para su procesamiento, que puede hacer que la adquisición de datos inicial más rápida.
* Si usa una consulta, puede agregar sugerencias de consulta opcional para Azure SQL DB, es decir, READ UNCOMMITTED

### <a name="set-sink-batch-size"></a>Establezca el tamaño del lote de receptor

![Receptor](media/data-flow/sink4.png "receptor")

* Con el fin de evitar el procesamiento fila por fila de los flujos de datos, establezca el "tamaño del lote" en la configuración del receptor de Azure SQL DB. Esto indicará a ADF para procesar base de datos que se escribe en lotes basándose en el tamaño especificado.

### <a name="set-partitioning-options-on-your-sink"></a>Establecer opciones en el receptor de particionamiento

* Incluso si no tiene los datos con particiones en las tablas de base de datos de SQL Azure de destino, vaya a la ficha optimizar y particiones del conjunto.
* Muy a menudo, simplemente indicando ADF se utiliza Round Robin, creación de particiones en los clústeres de ejecución de Spark resultados en la carga en lugar de obligarle a todas las conexiones desde una sola partición o nodo de datos mucho más rápidos.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Aumente el tamaño de su motor de proceso en tiempo de ejecución de integración de Azure

![Nueva instancia de IR](media/data-flow/ir-new.png "IR nuevo")

* Aumentar el número de núcleos, lo que aumentará el número de nodos y proporcionarle más capacidad de procesamiento para consultar y escribir en la base de datos de SQL Azure.
* Pruebe las opciones de "Proceso optimizado" y "Con optimización para memoria" para aplicar más recursos a los nodos de proceso.

### <a name="unit-test-and-performance-test-with-debug"></a>Pruebas unitarias y pruebas de rendimiento con la depuración

* Cuando los flujos de datos pruebas unitarias, establezca el botón "Datos de flujo de depuración" en ON.
* En el Diseñador de flujo de datos, utilice la pestaña de vista previa de datos en las transformaciones para ver los resultados de la lógica de transformación.
* Prueba unitaria de los datos fluyen desde el Diseñador de canalizaciones mediante la colocación de una actividad de flujo de datos en el diseño de canalización de lienzo y usar el botón "Debug" para probar.
* Las pruebas en modo de depuración funcionará en un entorno de clúster calienten activo sin necesidad de esperar una número de seguridad de clúster just-in-time.

### <a name="disable-indexes-on-write"></a>Deshabilitar índices durante la escritura
* Usar una actividad de procedimiento almacenado de canalización ADF antes de la actividad de flujo de datos que deshabilita los índices en las tablas de destino que se escriben en el receptor.
* Después de la actividad de flujo de datos, agregue otra actividad de procedimiento almacenado que habilitar estos índices.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Aumentar el tamaño de la base de datos de SQL Azure
* Programar un cambio de tamaño de su origen y receptor de Azure SQL DB antes de la serie de límites de la canalización para aumentar el rendimiento y minimizar la limitación de Azure una vez que alcance DTU.
* Una vez completada la ejecución de la canalización, puede cambiar el tamaño de las bases de datos a la velocidad de ejecución normal.

## <a name="optimizing-for-azure-sql-data-warehouse"></a>Optimización del almacenamiento de datos SQL Azure

### <a name="use-staging-to-load-data-in-bulk-via-polybase"></a>Utilizar el almacenamiento provisional para cargar datos de forma masiva a través de Polybase

* Con el fin de evitar el procesamiento fila por fila de los flujos de datos, establezca la opción de "Ensayo" en la configuración del receptor para que ADF puede aprovechar Polybase para evitar inserciones de fila por fila en el almacenamiento de datos. Esto indicará a ADF para usar Polybase para que los datos se pueden cargar de forma masiva.
* Cuando se ejecuta la actividad de flujo de datos de una canalización con almacenamiento provisional activado, deberá seleccionar la ubicación del almacén de blobs de los datos de almacenamiento provisionales para la carga masiva.

### <a name="increase-the-size-of-your-azure-sql-dw"></a>Aumentar el tamaño de almacenamiento de datos de SQL Azure

* Programar un cambio de tamaño de su origen y receptor de Azure SQL DW antes de ejecutar la canalización para aumentar el rendimiento y minimizar la limitación de Azure una vez que alcance los límites de DWU.

* Una vez completada la ejecución de la canalización, puede cambiar el tamaño de las bases de datos a la velocidad de ejecución normal.

## <a name="optimize-for-files"></a>Optimizar archivos

* Puede controlar cuántas particiones que se va a usar ADF. En cada transformación del origen y receptor, así como cada transformación individuales, puede establecer un esquema de partición. Para archivos más pequeños, es posible seleccionar "Partición única" a veces puede trabajar mejor y más rápido que pedir Spark para la partición de los archivos pequeños.
* Si no tiene suficiente información acerca de los datos de origen, puede elegir "Round Robin" creación de particiones y establecer el número de particiones.
* Si explorar los datos y que tenga las columnas que pueden ser claves hash correcto, use la opción de particionamiento de Hash.

### <a name="file-naming-options"></a>Opciones de nombre de archivo

* La naturaleza de forma predeterminada de la escritura de datos transformados en flujos de datos de asignación de ADF consiste en escribir en un conjunto de datos que tiene un Blob o un servicio vinculado de ADLS. Debe establecer ese conjunto de datos para que apunte a una carpeta o contenedor, no es un archivo con nombre.
* Flujos de datos, use Azure Databricks Spark para su ejecución, lo que significa que se va a dividir a través de varios archivos basados en la salida predeterminada o creación de particiones de Spark o las particiones de esquema que ha elegido explícitamente.
* Una operación muy común en flujos de datos de ADF es elegir "A único archivo de salida" para que todos los archivos de parte de la salida se combinan en un único archivo de salida.
* Sin embargo, esta operación requiere que el resultado se reduce a una única partición en un único nodo de clúster.
* Téngalo en cuenta al elegir esta opción popular. Puede quedarse sin recursos del nodo de clúster si combina muchos archivos grandes de origen en una partición del archivo de salida única.
* Para evitar agotar los recursos del nodo de proceso, puede mantener el valor predeterminado o el esquema de partición explícito en ADF, que optimiza el rendimiento, y, a continuación, agregar una actividad de copia posteriores en la canalización que combina toda la parte archivos de la carpeta de salida a un único nuevo archivo. En esencia, esta técnica separa la acción de transformación de combinación de archivos y consigue el mismo resultado que la configuración de "salida de archivo único".

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos de flujo de datos:

- [Información general sobre el flujo de datos](concepts-data-flow-overview.md)
- [Actividad de flujo de datos](control-flow-execute-data-flow-activity.md)
- [Supervisar el rendimiento del flujo de datos](concepts-data-flow-monitoring.md)
