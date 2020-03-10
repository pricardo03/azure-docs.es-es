---
title: Actividad de copia en Azure Data Factory
description: Aprenda sobre la actividad de copia en Azure Data Factory. Puede usarla para copiar datos de un almacén de datos de origen admitido a un almacén de datos receptor compatible.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: jingwang
ms.openlocfilehash: 0e138e954501df3cf3c3c8819d0198ad9a9288f0
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78358495"
---
# <a name="copy-activity-in-azure-data-factory"></a>Actividad de copia en Azure Data Factory

> [!div class="op_single_selector" title1="Seleccione la versión de Data Factory que va a usar:"]
> * [Versión 1](v1/data-factory-data-movement-activities.md)
> * [Versión actual](copy-activity-overview.md)

En Azure Data Factory, puede usar la actividad de copia para copiar datos entre almacenes de datos locales y en la nube. Después de copiar los datos, puede usar otras actividades para luego transformarlos y analizarlos. La actividad de copia también puede usarse para publicar los resultados de transformación y análisis de inteligencia empresarial (BI) y el consumo de la aplicación.

![El rol de la actividad de copia](media/copy-activity-overview/copy-activity.png)

La actividad de copia se ejecuta en un [entorno de ejecución de integración](concepts-integration-runtime.md). Puede usar diferentes tipos de entornos de ejecución de integración para diferentes escenarios de copia de datos:

* Al copiar datos entre dos almacenes de datos a los que se puede acceder públicamente mediante Internet desde cualquier dirección IP, puede usar el entorno de ejecución de integración de Azure para la actividad de copia. Este entorno de ejecución de integración es seguro, confiable, escalable y está [globalmente disponible](concepts-integration-runtime.md#integration-runtime-location).
* Cuando vaya a copiar datos con almacenes de datos como origen o destino ubicados en el entorno local o en una red con control de acceso (por ejemplo, una red virtual de Azure), debe configurar un entorno de ejecución de integración autohospedado.

Un entorno de ejecución de integración debe estar asociado con todos los almacenes de datos receptores y de origen. Para información sobre cómo la actividad de copia determina qué entorno de ejecución de integración se va a usar, consulte [Determinar qué entorno de ejecución de integración usar](concepts-integration-runtime.md#determining-which-ir-to-use).

Para copiar datos de un origen a un receptor, el servicio que ejecuta la actividad de copia realiza estos pasos:

1. Lee datos desde un almacén de datos de origen.
2. Realiza procesos de serialización y deserialización, compresión y descompresión, asignación de columnas, etc. Lleva a cabo estas operaciones según la configuración del conjunto de datos de entrada y salida, y la actividad de copia.
3. Escribe datos en el almacén de datos de receptor o destino.

![Introducción a la actividad de copia](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Almacenes de datos y formatos que se admiten

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Formatos de archivos admitidos

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

La actividad de copia se puede usar para copiar archivos tal cual entre dos almacenes de datos basados en archivos, en cuyo caso los datos se copian de manera eficaz sin serialización ni deserialización. Además, también puede analizar o generar archivos con un formato determinado; por ejemplo, puede realizar lo siguiente:

* Copiar datos de una base de datos de SQL Server local y escribirlos en Azure Data Lake Storage Gen2 en formato Parquet.
* Copiar archivos en formato de texto (CSV) desde el sistema de archivos local y escribirlos en Azure Blob Storage en formato Avro
* Copiar archivos comprimidos del sistema de archivos local, descomprimirlos sobre la marcha y escribirlos en Azure Data Lake Storage Gen2.
* Copiar datos en formato de texto comprimido Gzip (CSV) de Azure Blob Storage y escribirlos en Azure SQL Database
* Muchas más actividades que requieren serialización y deserialización o compresión y descompresión.

## <a name="supported-regions"></a>Regiones admitidas

El servicio que permite la actividad de copia está disponible globalmente en las regiones y zonas geográficas enumeradas en [Ubicaciones de Azure Integration Runtime](concepts-integration-runtime.md#integration-runtime-location). La topología disponible globalmente garantiza un movimiento de datos eficiente que, normalmente, evita saltos entre regiones. Consulte [Productos por región](https://azure.microsoft.com/regions/#services) para comprobar la disponibilidad de Data Factory y el movimiento de datos en una región específica.

## <a name="configuration"></a>Configuración

Para usar la actividad de copia en Azure Data Factory, debe:

1. **Crear servicios vinculados para el almacén de datos de origen y el almacén de datos receptor**. Consulte la sección "Propiedades del servicio vinculado" del artículo sobre conectores para información sobre la configuración y las propiedades admitidas. Puede encontrar la lista de conectores admitidos en la sección [Almacenes de datos y formatos que se admiten](#supported-data-stores-and-formats) de este artículo.
2. **Crear conjuntos de datos para el origen y el receptor**. Consulte las secciones "Propiedades del conjunto de datos" de los artículos sobre los conectores de origen y receptor para información sobre la configuración y las propiedades admitidas.
3. **Crear una canalización con la actividad de copia.** Se proporciona un ejemplo en la sección siguiente.

### <a name="syntax"></a>Sintaxis

La plantilla siguiente de una actividad de copia contiene una lista completa de todas las propiedades admitidas. Especifique las que se adapten a su escenario.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "dataIntegrationUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

#### <a name="syntax-details"></a>Detalles de la sintaxis

| Propiedad | Descripción | ¿Necesario? |
|:--- |:--- |:--- |
| type | Para una actividad de copia, establezca esta propiedad en `Copy`. | Sí |
| inputs | Especifique el conjunto de datos que creó y que señala a los datos de origen. La actividad de copia admite solo una entrada. | Sí |
| outputs | Especifique el conjunto de datos que creó y que señala a los datos del receptor. La actividad de copia admite solo una salida. | Sí |
| typeProperties | Especifique las propiedades para configurar la actividad de copia. | Sí |
| source | Especifique el tipo de origen de copia y las propiedades correspondientes para la recuperación de datos.<br/>Para más información, consulte la sección "Propiedades de la actividad de copia" del artículo sobre conectores que aparece en [Almacenes de datos y formatos que se admiten](#supported-data-stores-and-formats). | Sí |
| sink | Especifique el tipo de receptor de copia y las propiedades correspondientes para escribir datos.<br/>Para más información, consulte la sección "Propiedades de la actividad de copia" del artículo sobre conectores que aparece en [Almacenes de datos y formatos que se admiten](#supported-data-stores-and-formats). | Sí |
| translator | Especifique asignaciones de columna explícitas de origen a receptor. Esta propiedad se aplica cuando el comportamiento de copia predeterminado no satisface sus necesidades.<br/>Para más información, consulte [Asignación de esquemas en la actividad de copia](copy-activity-schema-and-type-mapping.md). | Sin |
| dataIntegrationUnits | Especifique una medida que represente la cantidad de potencia que emplea el [entorno de ejecución de integración de Azure](concepts-integration-runtime.md) para la copia de datos. Estas unidades se conocían anteriormente como unidades de movimiento de datos de nube (DMU). <br/>Para más información, consulte [Unidades de integración de datos](copy-activity-performance.md#data-integration-units). | Sin |
| parallelCopies | Especifique el paralelismo que quiere que use la actividad de copia al leer datos del origen y escribirlos en el receptor.<br/>Para obtener más información, consulte [Copia paralela](copy-activity-performance.md#parallel-copy). | Sin |
| preservar | Especifique si desea conservar los metadatos o las ACL durante la copia de datos. <br/>Para obtener más información, vea [Conservación de metadatos](copy-activity-preserve-metadata.md). |Sin |
| enableStaging<br/>stagingSettings | Especifique si quiere almacenar provisionalmente los datos en una instancia de Blob Storage en lugar de copiarlos directamente del origen al receptor.<br/>Para información sobre escenarios útiles y detalles de configuración, consulte [Copia almacenada provisionalmente](copy-activity-performance.md#staged-copy). | Sin |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Elija cómo controlar las filas incompatibles al copiar datos del origen al receptor.<br/>Para más información, consulte [Tolerancia a errores](copy-activity-fault-tolerance.md). | Sin |

## <a name="monitoring"></a>Supervisión

Puede supervisar la ejecución de la actividad de copia en la interfaz de usuario **Author & Monitor** (Creación y supervisión) de Azure Data Factory o mediante programación.

### <a name="monitor-visually"></a>Supervisión visual

Para supervisar visualmente la ejecución de la actividad de copia, vaya a su factoría de datos y, luego, a **Author & Monitor** (Creación y supervisión). En la pestaña **Monitor** (Supervisar), verá una lista de ejecuciones de canalización con un botón **View Activity Run** (Ver ejecución de actividad) en la columna **Actions** (Acciones):

![La supervisión de la canalización se ejecuta](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)

Seleccione **View Activity Runs** (Ver ejecuciones de actividad) para ver la lista de actividades en la ejecución de canalización. En la columna **Actions** (Acciones), verá vínculos a la entrada, la salida, los errores (si se producen errores en la ejecución de la actividad de copia) y los detalles de la actividad de copia.

![Supervisión de las ejecuciones de actividad](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

Seleccione el botón **Details** (Detalles) de la columna **Actions** (Acciones) para ver los detalles de ejecución y las características de rendimiento de la actividad de copia. Verá información como el volumen, el número de filas o el número de archivos de datos copiados del origen al receptor, el rendimiento, los pasos de la actividad de copia con las duraciones correspondientes y las configuraciones que se usan en el escenario de copia.

>[!TIP]
>En algunos escenarios, también verá **sugerencias para la optimización del rendimiento** en la parte superior de la página de supervisión de la copia. Estas sugerencias le indican los cuellos de botella identificados y proporcionan información sobre lo que debe cambiar para aumentar el rendimiento de copia. Para ver un ejemplo, consulte la sección [Rendimiento y optimización](#performance-and-tuning) de este artículo.

**Ejemplo: Copia de Amazon S3 en Azure Data Lake Store**
![Detalles de la supervisión de la ejecución de actividad](./media/copy-activity-overview/monitor-activity-run-details-adls.png)

**Ejemplo: Copia de Azure SQL Database en Azure SQL Data Warehouse con una copia de almacenamiento temporal**
![Detalles de la supervisión de la ejecución de actividad](./media/copy-activity-overview/monitor-activity-run-details-sql-dw.png)

### <a name="monitor-programmatically"></a>Supervisión mediante programación

Los detalles de la ejecución de la actividad de copia y las características de rendimiento también se devuelven en la sección **Copy Activity run result** > **Output** (Resultado de la ejecución de la actividad de copia -> Salida). A continuación se muestra una lista completa de las propiedades que pueden devolverse. Solo verá las propiedades que se aplican a su escenario de copia. Para información sobre cómo supervisar las ejecuciones de actividad, consulte [Supervisión de una ejecución de canalización](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run).

| Nombre de propiedad  | Descripción | Unidad |
|:--- |:--- |:--- |
| dataRead | Cantidad de datos leídos del origen. | Valor Int64 en bytes |
| dataWritten | Cantidad de datos escritos en el receptor. | Valor Int64 en bytes |
| filesRead | Número de archivos copiados durante la copia desde File Storage. | Valor Int64 (sin unidad) |
| filesWritten | Número de archivos copiados durante la copia en File Storage. | Valor Int64 (sin unidad) |
| sourcePeakConnections | Número máximo de conexiones simultáneas establecidas en el almacén de datos de origen durante la ejecución de la actividad de copia. | Valor Int64 (sin unidad) |
| sinkPeakConnections | Número máximo de conexiones simultáneas establecidas en el almacén de datos receptor durante la ejecución de la actividad de copia. | Valor Int64 (sin unidad) |
| rowsRead | Número de filas que se leen del origen (no se aplica a la copia binaria). | Valor Int64 (sin unidad) |
| rowsCopied | Número de filas que se copian en el receptor (no se aplica a la copia binaria). | Valor Int64 (sin unidad) |
| rowsSkipped | Número de filas incompatibles que se han omitido. Puede permitir que se omitan las filas incompatibles; para ello, establezca `enableSkipIncompatibleRow` en true. | Valor Int64 (sin unidad) |
| copyDuration | Duración de la ejecución de copia. | Valor Int32 en segundos |
| throughput | Velocidad de transferencia de datos. | Número de punto flotante en KBps |
| sourcePeakConnections | Número máximo de conexiones simultáneas establecidas en el almacén de datos de origen durante la ejecución de la actividad de copia. | Valor Int32 (sin unidad) |
| sinkPeakConnections| Número máximo de conexiones simultáneas establecidas en el almacén de datos receptor durante la ejecución de la actividad de copia.| Valor Int32 (sin unidad) |
| sqlDwPolyBase | Si se usará PolyBase cuando se copian datos en SQL Data Warehouse. | Boolean |
| redshiftUnload | Si se usará UNLOAD cuando se copian datos de Redshift. | Boolean |
| hdfsDistcp | Si se usará DistCp cuando se copian datos de HDFS. | Boolean |
| effectiveIntegrationRuntime | El entorno de ejecución de integración (IR) o los tiempos de ejecución que se usan para aumentar la potencia de la ejecución de la actividad, en el formato `<IR name> (<region if it's Azure IR>)`. | Texto (cadena) |
| usedDataIntegrationUnits | Unidades de integración de datos vigentes durante la copia. | Valor Int32 |
| usedParallelCopies | El número de parallelCopies efectivo durante la copia. | Valor Int32 |
| redirectRowPath | Ruta de acceso al registro de filas incompatibles omitidas en el almacenamiento de blobs que se configura en la propiedad `redirectIncompatibleRowSettings`. Consulte [Tolerancia a errores](#fault-tolerance) más adelante en este artículo. | Texto (cadena) |
| executionDetails | Más información sobre las fases por las que pasa la actividad de copia y los pasos, la duración, las configuraciones usadas, etc. correspondientes. No se recomienda analizar esta sección porque podría cambiar.<br/><br/>Data Factory también informa de las duraciones detalladas (en segundos) empleadas en varias fases en `detailedDurations`. Las duraciones de estos pasos son exclusivas. Solo aparecen las duraciones que se aplican a la ejecución de la actividad de copia dada:<br/>**Duración de puesta en cola** (`queuingDuration`): tiempo hasta que la actividad de copia se inicia realmente en el entorno de ejecución de integración. Si usa un entorno de ejecución de integración autohospedado y este valor es grande, compruebe la capacidad y el uso del entorno, y escálelo vertical u horizontalmente según la carga de trabajo. <br/>**Duración del script anterior a la copia** (`preCopyScriptDuration`): tiempo transcurrido entre el momento en que la actividad de copia comienza en el entorno de ejecución de integración y el momento en que la actividad de copia termina de ejecutar el script anterior a la copia en el almacén de datos receptor. Se aplica cuando se configura el script anterior a la copia. <br/>**Tiempo hasta el primer byte** (`timeToFirstByte`): tiempo transcurrido entre el final del paso anterior y el momento en que la instancia de IR recibe el primer byte del almacén de datos de origen. Se aplica a un origen no basado en archivos. Si este valor es grande, compruebe y optimice la consulta o el servidor.<br/>**Duración de la transferencia** (`transferDuration`): tiempo transcurrido entre el final del paso anterior y el momento en que el entorno de ejecución de integración transfiere todos los datos del origen al receptor. | Array |
| perfRecommendation | Sugerencias de optimización del rendimiento de la copia. Consulte [Rendimiento y optimización](#performance-and-tuning) para más información. | Array |

```json
"output": {
    "dataRead": 6198358,
    "dataWritten": 19169324,
    "filesRead": 1,
    "sourcePeakConnections": 1,
    "sinkPeakConnections": 2,
    "rowsRead": 39614,
    "rowsCopied": 39614,
    "copyDuration": 1325,
    "throughput": 4.568,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedDataIntegrationUnits": 4,
    "usedParallelCopies": 1,
    "executionDetails": [
        {
            "source": {
                "type": "AzureBlobStorage"
            },
            "sink": {
                "type": "AzureSqlDatabase"
            },
            "status": "Succeeded",
            "start": "2019-08-06T01:01:36.7778286Z",
            "duration": 1325,
            "usedDataIntegrationUnits": 4,
            "usedParallelCopies": 1,
            "detailedDurations": {
                "queuingDuration": 2,
                "preCopyScriptDuration": 12,
                "transferDuration": 1311
            }
        }
    ],
    "perfRecommendation": [
        {
            "Tip": "Sink Azure SQL Database: The DTU utilization was high during the copy activity run. To achieve better performance, you are suggested to scale the database to a higher tier than the current 1600 DTUs.",
            "ReferUrl": "https://go.microsoft.com/fwlink/?linkid=2043368",
            "RuleName": "AzureDBTierUpgradePerfRecommendRule"
        }
    ]
}
```

## <a name="incremental-copy"></a>Copia incremental

Data Factory permite la copia incremental de datos diferenciales de un almacén de datos de origen a un almacén de datos receptor. Para más información, consulte [Tutorial: Copia de datos de forma incremental](tutorial-incremental-copy-overview.md).

## <a name="performance-and-tuning"></a>Rendimiento y optimización

En el artículo [Guía de escalabilidad y rendimiento de la actividad de copia](copy-activity-performance.md) se describen los factores claves que afectan al rendimiento del movimiento de datos mediante la actividad de copia en Azure Data Factory. También se muestran los valores de rendimiento observados durante las pruebas y se describe cómo optimizar el rendimiento de la actividad de copia.

En algunos casos, cuando se ejecuta una actividad de copia en Data Factory, se ve el mensaje **Performance tuning tips** (Sugerencias para la optimización del rendimiento) en la parte superior de la página [Copy activity monitoring](#monitor-visually) (Supervisión de la actividad de copia), como se muestra en el ejemplo siguiente. Las sugerencias indican el cuello de botella identificado de la ejecución de la copia dada. También proporcionan información sobre lo que se debe cambiar para aumentar el rendimiento de la copia. Las sugerencias para la optimización del rendimiento actualmente ofrecen recomendaciones, como usar PolyBase al copiar datos en Azure SQL Data Warehouse, aumentar las unidades de solicitud (RU) de Azure Cosmos DB o las unidades de transacción de base de datos (DTU) de Azure SQL Database cuando el recurso del almacén de datos forma el cuello de botella, así como quitar las copias preconfiguradas innecesarias.

**Ejemplo: Copia en Azure SQL Database con una sugerencia de optimización del rendimiento**

En este ejemplo, durante la ejecución de una copia, Data Factory realiza un seguimiento de la utilización de un número elevado de DTU en la instancia de Azure SQL Database del receptor. Esta condición reduce la velocidad de las operaciones de escritura. La sugerencia es aumentar el número de DTU en el nivel de Azure SQL Database.

![Supervisión de copia con sugerencias de optimización del rendimiento](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

## <a name="resume-from-last-failed-run"></a>Reanudación desde el último error de ejecución

La actividad de copia admite la reanudación desde el último error de ejecución cuando copia un gran tamaño de archivos tal cual con un formato binario entre almacenes basados en archivos y elige conservar la jerarquía carpeta/archivo del origen al receptor, por ejemplo, para migrar datos de Amazon S3 a Azure Data Lake Storage Gen2. Se aplica a los conectores siguientes basados en archivos: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [File System](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md) y [SFTP](connector-sftp.md).

Puede aprovechar la reanudación de la actividad de copia de las dos maneras siguientes:

- **Reintento de nivel de actividad:** Puede establecer el número de reintentos en la actividad de copia. Durante la ejecución de la canalización, si se produce un error en la ejecución de la actividad de copia, el siguiente reintento automático se iniciará desde el punto de error de la última prueba.
- **Volver a ejecutar desde la actividad con errores:** Una vez finalizada la ejecución de la canalización, también puede desencadenar una nueva ejecución desde la actividad con errores en la vista de supervisión de la interfaz de usuario de ADF o mediante programación. Si la actividad con errores es una actividad de copia, la canalización no solo se volverá a ejecutar desde esta actividad, sino que también se reanudará desde el punto de error de la ejecución anterior.

    ![Reanudación de copia](media/copy-activity-overview/resume-copy.png)

Algunos puntos que se deben tener en cuenta:

- La reanudación se produce en el nivel de archivo. Si se produce un error en la actividad de copia al copiar un archivo, este archivo específico se volverá a copiar en la siguiente ejecución.
- Para que la reanudación funcione correctamente, no cambie la configuración de la actividad de copia entre las reactivaciones.
- Al copiar datos desde Amazon S3, Azure Blob, Azure Data Lake Storage Gen2 y Google Cloud Storage, la actividad de copia se puede reanudar a partir de un número arbitrario de archivos copiados. Por su parte, cuando los orígenes son el resto de conectores basados en archivos, la actividad de copia actualmente admite la reanudación desde un número limitado de archivos, normalmente en el intervalo de decenas de miles y varía en función de la longitud de las rutas de acceso de archivo. Los archivos que superen este número se volverán a copiar durante la nueva ejecución.

En el caso de otros escenarios de copia de archivos binarios, la ejecución de la actividad de copia comienza desde el principio.

## <a name="preserve-metadata-along-with-data"></a>Conservación de los metadatos junto con los datos

Al copiar datos desde el origen al receptor, en escenarios como la migración de Data Lake, también puede optar por conservar los metadatos y las ACL junto con los datos mediante la actividad de copia. Consulte [Conservación de metadatos](copy-activity-preserve-metadata.md) para obtener más información.

## <a name="schema-and-data-type-mapping"></a>Asignación de tipo de datos y esquema

Para información sobre cómo la actividad de copia asigna los datos de origen al receptor, consulte [Asignación de tipo de datos y esquema](copy-activity-schema-and-type-mapping.md).

## <a name="fault-tolerance"></a>Tolerancia a errores

De forma predeterminada, la actividad de copia detiene la copia de datos y devuelve un error cuando las filas de datos de origen no son compatibles con las filas de datos del receptor. Para que la copia se realice correctamente, puede configurar la actividad de copia para omitir y registrar las filas incompatibles y copiar solo los datos compatibles. Para más información, consulte [Tolerancia a errores de la actividad de copia en Azure Data Factory](copy-activity-fault-tolerance.md).

## <a name="next-steps"></a>Pasos siguientes
Consulte las guías de inicio rápido, los tutoriales y los ejemplos siguientes:

- [Copia de datos de una ubicación a otra en la misma cuenta de Azure Blob Storage](quickstart-create-data-factory-dot-net.md)
- [Copia de datos de Azure Blob Storage a Azure SQL Database](tutorial-copy-data-dot-net.md)
- [Copia de datos de una base de datos de SQL Server local a Azure Blob Storage](tutorial-hybrid-copy-powershell.md)
