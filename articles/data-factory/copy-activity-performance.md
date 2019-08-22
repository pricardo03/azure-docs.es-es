---
title: Guía de optimización y rendimiento de la actividad de copia en Azure Data Factory | Microsoft Docs
description: Conozca los factores más importantes que afectan al rendimiento del movimiento de datos en Azure Data Factory cuando se usa la actividad de copia.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: d8ce0a4f6bacdd1c8c858d474e6f3957a23c6357
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967355"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Guía de optimización y rendimiento de la actividad de copia
> [!div class="op_single_selector" title1="Seleccione la versión de Azure Data Factory que usa:"]
> * [Versión 1](v1/data-factory-copy-activity-performance.md)
> * [Versión actual](copy-activity-performance.md)


La actividad de copia de Azure Data Factory ofrece una solución de carga de datos de alto rendimiento fiable y segura de primera clase. Puede usarla para copiar decenas de terabytes de datos al día en una amplia variedad de almacenes de datos locales y en la nube. Un rendimiento acelerado de la carga de datos es clave para garantizar que puede centrarse en el problema principal de los macrodatos: crear soluciones de análisis avanzadas y profundizar en todos esos datos.

Azure proporciona un conjunto de soluciones de almacenamiento de datos de nivel empresarial. La actividad de copia ofrece una experiencia de carga de datos altamente optimizada que es fácil de configurar e instalar. Con solo una actividad de copia, puede cargar datos en:

* Azure SQL Data Warehouse a 1,2 GBps.
* Azure Blob Storage a 1,0 GBps.
* Azure Data Lake Store a 1.0 GBps.

En este artículo se describe:

* [Números de referencia de rendimiento](#performance-reference) para los almacenes de datos origen y receptor admitidos que le ayudan a planear su proyecto.
* Características que pueden potenciar el rendimiento de la copia en diferentes escenarios, lo que incluye [unidades de integración de datos](#data-integration-units) (DIU), [copias en paralelo](#parallel-copy) y [copias almacenadas provisionalmente](#staged-copy).
* [Guía para el ajuste de rendimiento](#performance-tuning-steps), donde se explica cómo optimizar el rendimiento y los factores clave que pueden afectar al rendimiento de la copia.

> [!NOTE]
> Si no está familiarizado con la actividad de copia en general, consulte la [información general de la actividad de copia](copy-activity-overview.md) antes de leer este artículo.
>

## <a name="performance-reference"></a>Referencia de rendimiento

Como referencia, la siguiente tabla muestra la cantidad de procesamiento de copias, en MBps, de los pares origen-receptor dados en una ejecución individual de la actividad de copia de unas pruebas internas. A efectos de comparación, también muestra cómo distintas opciones de configuración de [unidades de integración de datos](#data-integration-units) o [escalabilidad de Integration Runtime autohospedado](concepts-integration-runtime.md#self-hosted-integration-runtime) (varios nodos) pueden contribuir al rendimiento de la realización de copias.

![Matriz de rendimiento](./media/copy-activity-performance/CopyPerfRef.png)

> [!IMPORTANT]
> Cuando la actividad de copia se ejecuta en una instancia de Azure Integration Runtime, la cantidad mínima permitida de unidades de integración de datos (antes conocidas como unidades de movimiento de datos) es dos. Si no se especifica, en [Unidades de integración de datos](#data-integration-units) puede ver las unidades de integración de datos predeterminadas que se usan.

**Puntos a tener en cuenta:**

* La capacidad de procesamiento se calcula mediante la siguiente fórmula: [tamaño de los datos leídos del origen]/[duración de la ejecución de la actividad de copia].
* Los números de referencia de rendimiento de la tabla se midieron mediante un conjunto de datos [TPC-H](http://www.tpc.org/tpch/) en una ejecución de actividad de copia única. Los archivos de prueba de los almacenes basados en archivos son varios archivos con un tamaño de 10 GB.
* En los almacenes de datos de Azure, el origen y el receptor se encuentran en la misma región de Azure.
* Para la copia híbrida entre almacenes de datos locales y en la nube, cada nodo de Integration Runtime autohospedado se ejecutaba en una máquina independiente del almacén de datos con la especificación siguiente. Si se ejecutó una única actividad, la operación de copia solo consumió una pequeña parte del ancho de banda de red, memoria o CPU de la máquina de prueba.
    <table>
    <tr>
        <td>CPU</td>
        <td>Intel Xeon E5-2660 v2 de 32 núcleos a 2,20 GHz</td>
    </tr>
    <tr>
        <td>Memoria</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Red</td>
        <td>Interfaz de Internet: 10 Gbps. Interfaz de intranet: 40 Gbps</td>
    </tr>
    </table>


> [!TIP]
> Para conseguir un mayor rendimiento es preciso usar más DIU. Por ejemplo, con 100 DIU, es posible copiar datos de Azure Blob a Azure Data Lake Store a una velocidad de 1,0 GBps. Para más información acerca de esta característica y del escenario admitido, consulte la sección [Unidades de integración de datos](#data-integration-units). 

## <a name="data-integration-units"></a>Unidades de integración de datos

Una unidad de integración de datos es una medida que representa la eficacia (una combinación de CPU, memoria y asignación de recursos de red) de una única unidad en Azure Data Factory. Una unidad de integración de datos solo se aplica a [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime), pero no a [Integration Runtime autohospedado](concepts-integration-runtime.md#self-hosted-integration-runtime).

El número mínimo de DIU para impulsar la ejecución de una actividad de copia es dos. Si no se especifica, en la tabla siguiente se muestran las DIU predeterminadas que se usan en distintos escenarios de copia:

| Escenario de copia | DIU predeterminadas que determina el servicio |
|:--- |:--- |
| Copia de datos entre almacenes basados en archivos | Entre 4 y 32, según el número y tamaño de los archivos |
| Copia de datos en Azure SQL Database o Azure Cosmos DB |Entre 4 y 16 en función del nivel de Azure SQL Database o Cosmos DB (número de RU o DTU) del receptor |
| Los restantes escenarios de copia | 4 |

Para reemplazar esta configuración predeterminada, especifique un valor para la propiedad **dataIntegrationUnits** de la manera siguiente. Los *valores admitidos* para la propiedad **dataIntegrationUnits** son 256 como máximo. El *número real de DIU* que usa la operación de copia en tiempo de ejecución es igual o inferior al valor configurado, según el patrón de datos. Para más información sobre el nivel de ganancia de rendimiento que puede obtener al configurar más unidades para un origen y un receptor de copia específicos, consulte la [referencia de rendimiento](#performance-reference).

El número de unidades de integración de datos utilizadas en cada ejecución de copia se puede ver en la salida de la actividad de copia al supervisar una ejecución de actividad. Para más información, consulte el artículo [Supervisión de la actividad de copia](copy-activity-overview.md#monitoring).

> [!NOTE]
> El establecimiento de más de cuatro unidades de integración de datos actualmente solo se aplica cuando se copian varios archivos desde Azure Storage, Azure Data Lake Storage, Amazon S3, Google Cloud Storage, FTP en la nube o SFTP en la nube a cualquier otro almacén de datos en la nube.
>

**Ejemplo**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "dataIntegrationUnits": 32
        }
    }
]
```

### <a name="data-integration-units-billing-impact"></a>Impacto de facturación de las unidades de integración de datos

Recuerde que se cobra en función del tiempo total de la operación de copia. La duración total que se factura por el movimiento de datos es la suma de la duración de todas las DIU. Si un trabajo de copia solía tardar una hora con dos unidades de nube y ahora tarda 15 minutos con ocho, la factura general sigue siendo casi igual.

## <a name="parallel-copy"></a>Copia en paralelo

Puede usar la propiedad **parallelCopies** para indicar el paralelismo que desea que use la actividad de copia. Esta propiedad se puede considerar como el número máximo de subprocesos dentro de la actividad de copia que se pueden leer del origen o escribir en los almacenes de datos receptores en paralelo.

Para cada ejecución de una actividad de copia, Azure Data Factory determina el número de copias paralelas que se usarán para copiar datos del almacén de datos de origen al de destino. El número predeterminado de copias en paralelo que utilice depende del tipo de origen y receptor que se use.

| Escenario de copia | Recuento predeterminado de copias en paralelo determinado por servicio |
| --- | --- |
| Copia de datos entre almacenes basados en archivos |Depende del tamaño de los archivos y del número de unidades de integración de datos (DIU) que se usen para copiar datos entre dos almacenes de datos en la nube, o de la configuración física de la máquina en que se encuentre Integration Runtime autohospedado. |
| Copia de datos desde cualquier almacén de origen a Azure Table Storage |4 |
| Todos los demás escenarios de copia |1 |

> [!TIP]
> Al copiar datos entre almacenes basados en archivos, normalmente el comportamiento predeterminado es el que ofrece el mejor rendimiento. El comportamiento predeterminado es determina automáticamente en función del patrón del archivo de origen.

Para controlar la carga en las máquinas que hospedan los almacenes de datos o para optimizar el rendimiento de la copia puede reemplazar el valor predeterminado y especificar un valor para la propiedad **parallelCopies**. El valor debe ser un entero mayor o igual que 1. En tiempo de ejecución, y para obtener el mejor rendimiento, la actividad de copia usa un valor inferior o igual al valor que ha establecido.

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

**Puntos a tener en cuenta:**

* Cuando se copian datos entre almacenes basados en archivos, **parallelCopies** determina el paralelismo en el nivel de archivo. La fragmentación dentro de un archivo individual se produce por debajo de forma automática y transparente. Está diseñada para utilizar el tamaño de fragmento más adecuado para un tipo de almacén de datos de origen determinado para cargar datos en paralelo y ortogonales a **parallelCopies**. El número real de copias en paralelo que usa el servicio de movimiento de datos para la operación de copia en tiempo de ejecución no es superior al número de archivos que tenga. Si el comportamiento de copia es **mergeFile**, la actividad de copia no puede aprovechar las ventajas del paralelismo de nivel de archivo.
* Al copiar datos de almacenes no basados en archivos (salvo si el conector de [Oracle](connector-oracle.md#oracle-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source) y [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source) es el origen y tiene habilitada la creación de particiones de datos) en almacenes que sí se basan en archivos, el servicio de movimiento de datos omite la propiedad **parallelCopies**. Aunque se especifica el paralelismo, no se aplica en este caso.
* La propiedad **parallelCopies** es ortogonal para **dataIntegrationUnits**. La información anterior se cuenta en todas las unidades de integración de datos.
* Cuando especifique un valor para la propiedad **parallelCopies**, tenga en cuenta el aumento de la carga en los almacenes de datos de origen y receptor. Tenga también en cuenta el aumento de carga para IR autohospedado si la actividad de copia tiene autorización, por ejemplo, para la copia híbrida. Dicho aumento sucede especialmente si tiene varias actividades o ejecuciones simultáneas de las mismas actividades que se ejecutan en el mismo almacén de datos. Si observa que el almacén de datos o Integration Runtime autohospedado están sobrecargados, disminuya el valor de **parallelCopies** para aliviar la carga.

## <a name="staged-copy"></a>copia almacenada provisionalmente

Al copiar datos de un almacén de datos de origen a un almacén de datos receptor, podría elegir usar Almacenamiento de blobs como almacenamiento provisional. El almacenamiento provisional es especialmente útil en los siguientes casos:

- **Desea ingerir datos de varios almacenes de datos en SQL Data Warehouse mediante PolyBase.** SQL Data Warehouse emplea PolyBase como mecanismo de alto rendimiento para cargar una gran cantidad de datos en SQL Data Warehouse. Los datos de origen deben estar en Blob Storage o Azure Data Lake Store y deben satisfacer criterios adicionales. Al cargar datos desde un almacén de datos distinto de Blob Storage o Azure Data Lake Store, puede activar la copia de datos mediante Blob Storage de almacenamiento provisional. En ese caso, Azure Data Factory realiza las transformaciones de datos necesarias para garantizar que se cumplen los requisitos de PolyBase. A continuación, se usa PolyBase para cargar datos en SQL Data Warehouse de manera eficaz. Para más información, consulte [Uso de PolyBase para cargar datos en Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **En ocasiones, se tarda un tiempo en realizar un movimiento de datos híbridos (es decir, copiar desde un almacén de datos local a un almacén de datos en la nube) a través de una conexión de red lenta.** Para mejorar el rendimiento se puede usar una copia almacenada provisionalmente para comprimir los datos de forma local, con el fin de que se tarde menos tiempo en mover datos al almacén de datos provisional en la nube. Luego, puede descomprimir los datos en dicho almacén antes de cargarlos en el almacén de datos de destino.
- **Solo desea abrir los puertos 80 y el 443 en el firewall, a causa de las directivas de TI corporativas**. Por ejemplo, al copiar datos de un almacén de datos local a un receptor de Azure SQL Database o a un receptor de Azure SQL Data Warehouse, debe activar la comunicación TCP saliente en el puerto 1433 tanto para el firewall de Windows como para el firewall corporativo. En ese escenario, una copia almacenada provisionalmente puede aprovechar la ventaja de IR autohospedado para copiar primero los datos en una instancia provisional de Blob Storage a través de HTTP o HTTPS en el puerto 443. Luego, puede cargar dichos datos en SQL Database o en SQL Data Warehouse desde la instancia provisional de Blob Storage. En este flujo, no es necesario habilitar el puerto 1433.

### <a name="how-staged-copy-works"></a>Funcionamiento de las copias almacenadas provisionalmente

Al activar la característica de almacenamiento provisional, primero se copian los datos desde el almacén de datos de origen a la instancia de Blob Storage provisional (use su propio). A continuación, los datos se copian desde el almacén de datos provisional al almacén de datos receptor. Azure Data Factory administra automáticamente el flujo de dos fases. Azure Data Factory también limpia los datos temporales del almacenamiento provisional cuando se completa el movimiento de los datos.

![copia almacenada provisionalmente](media/copy-activity-performance/staged-copy.png)

Cuando activa el movimiento de datos mediante un almacén provisional, puede especificar si quiere que los datos se compriman antes de moverlos del almacén de datos de origen a uno provisional o intermedio, y luego descomprimirlos antes de moverlos desde este último al almacén de datos receptor.

Actualmente, no es posible copiar datos entre dos almacenes de datos que estén conectados a través de distintos IR autohospedados, ni con copia almacenada provisionalmente ni sin ella. Para dicho escenario, puede configurar dos actividades de copia explícitamente encadenadas, con el fin de copiar desde el origen al almacenamiento provisional y, después, desde este al receptor.

### <a name="configuration"></a>Configuración

Configure el valor **enableStaging** de la actividad de copia para especificar si desea que los datos se almacenen provisionalmente en Blob Storage antes de cargarlos en un almacén de datos de destino. Cuando establezca **enableStaging** en `TRUE`, especifique las propiedades adicionales que se muestran en la tabla siguiente. Si no tiene un servicio vinculado a la firma de acceso compartido de Azure Storage o de Storage, debe crearlo.

| Propiedad | Description | Valor predeterminado | Obligatorio |
| --- | --- | --- | --- |
| enableStaging |Especifique si desea copiar los datos a través de un almacén provisional. |False |Sin |
| linkedServiceName |Especifique el nombre de un servicio vinculado [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) que haga referencia a la instancia de Storage que se usa como almacenamiento provisional. <br/><br/> Storage no se puede usar con una firma de acceso compartido para cargar datos en SQL Data Warehouse mediante PolyBase. Puede usarlo en todos los demás casos. |N/D |Sí, cuando el valor de **enableStaging** está establecido en True. |
| path |Especifique la ruta de acceso de Almacenamiento de blobs que quiere que contenga los datos almacenados provisionalmente. Si no se proporciona una ruta de acceso, el servicio crea un contenedor para almacenar los datos temporales. <br/><br/> Especifique una ruta de acceso solo si usa Almacenamiento con una firma de acceso compartido o si necesita que los datos temporales estén en una ubicación específica. |N/D |Sin |
| enableCompression |Especifica si se deben comprimir los datos antes de copiarlos en el destino. Esta configuración reduce el volumen de datos que se va a transferir. |False |Sin |

>[!NOTE]
> Si usa una copia almacenada provisionalmente con la compresión habilitada, no se admite la autenticación de MSI o de la entidad de servicio para el almacenamiento provisional de un servicio vinculado de blob.

Este es un ejemplo de definición de una actividad de copia con las propiedades que se han descrito en la tabla anterior:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Impacto en la facturación de copia almacenada provisionalmente

Los cargos que se le realizan se basan en dos pasos: duración de la copia y tipo de copia.

* Cuando use el almacenamiento provisional durante una copia en la nube, que copia de datos de un almacén de datos en la nube a otro, donde ambos almacenamientos provisionales usan Azure Integration Runtime, se le cobrará de la siguiente manera: [suma de la duración de la copia de los pasos 1 y 2] x [precio unitario de la copia en la nube].
* Cuando use el almacenamiento provisional durante una copia híbrida, que copia de datos de un almacén de datos local a uno en la nube, un almacenamiento provisional que usa Integration Runtime autohospedado, se le cobrará de la siguiente manera: [duración de la copia híbrida] x [precio unitario de la copia híbrida] + [duración de la copia de nube] x [precio unitario de la copia de nube].

## <a name="performance-tuning-steps"></a>Pasos de optimización del rendimiento

Para optimizar el rendimiento del servicio Azure Data Factory con la actividad de copia, siga estos pasos.

1. **Establezca una línea de base.** Durante la fase de desarrollo, pruebe la canalización, para lo que debe usar la actividad de copia con unos datos de ejemplo representativos. Recopile los detalles de la ejecución y las características del rendimiento después de la [supervisión de la actividad de copia](copy-activity-overview.md#monitoring).

2. **Diagnostique y optimice el rendimiento.** Si el rendimiento que observa no cumple sus expectativas, identifique los cuellos de botella. A continuación, optimice el rendimiento para eliminar o reducir el efecto de los cuellos de botella.

    En algunos casos, cuando se ejecuta una actividad de copia en Azure Data Factory, se ve el mensaje "Sugerencias para la optimización del rendimiento" en la parte superior de la [página de supervisión de actividad de copia](copy-activity-overview.md#monitor-visually), como se muestra en el ejemplo siguiente. El mensaje indica que se identificó un cuello de botella para la ejecución de una copia determinada. También sirve de guía para realizar los cambios necesarios para mejorar el rendimiento de la copia. Las sugerencias de optimización de rendimiento actualmente proporcionan sugerencias como:

    - Use PolyBase cuando copie datos en Azure SQL Data Warehouse.
    - Aumente las unidades de solicitud de Azure Cosmos DB o las DTU (unidades de procesamiento de base de datos) de Azure SQL Database cuando el recurso del almacén de datos es el cuello de botella.
    - Quite la copia almacenada provisionalmente innecesaria.

    Las reglas de optimización del rendimiento también se enriquecerán de forma gradual.

    **Ejemplo: copia en Azure SQL DB con sugerencias de optimización del rendimiento**

    En este ejemplo, durante la ejecución de una copia, Azure Data Factory observa que la instancia de Azure SQL Database receptora alcanza una utilización elevada de la unidad de procesamiento de base de datos, lo que ralentiza las operaciones de escritura. La sugerencia es aumentar el nivel de Azure SQL Database con más DTU. 

    ![Supervisión de copia con sugerencias de optimización del rendimiento](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

    Además, existen las siguientes consideraciones comunes. Una descripción completa del diagnóstico de rendimiento va más allá del ámbito de este artículo.

   * Características de rendimiento:
     * [Copia paralela](#parallel-copy)
     * [Unidades de integración de datos](#data-integration-units)
     * [Copias almacenadas provisionalmente](#staged-copy)
     * [Escalabilidad de Integration Runtime autohospedado](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Integration Runtime autohospedado](#considerations-for-self-hosted-integration-runtime)
   * [Origen](#considerations-for-the-source)
   * [Sink](#considerations-for-the-sink)
   * [Serialización y deserialización](#considerations-for-serialization-and-deserialization)
   * [Compresión](#considerations-for-compression)
   * [Asignación de columnas](#considerations-for-column-mapping)
   * [Otras consideraciones](#other-considerations)

3. **Expanda la configuración a todo el conjunto de datos.** Cuando esté satisfecho con los resultados y el rendimiento de la ejecución, puede expandir la definición y la canalización para cubrir todo el conjunto de datos.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Consideraciones acerca de Integration Runtime autohospedado

Si la actividad de copia se ejecuta en una instancia de Integration Runtime autohospedado, tenga en cuenta lo siguiente:

**Configuración**: se recomienda usar una máquina dedicada para hospedar Integration Runtime. Consulte [Consideraciones acerca del uso de Integration Runtime autohospedado](concepts-integration-runtime.md).

**Escalado horizontal**: una instancia lógica individual de Integration Runtime autohospedado con uno o varios nodos puede atender a varias ejecuciones de actividad de copia simultáneamente. Si depende mucho del movimiento de datos híbridos con un gran número de ejecuciones de actividad de copia simultáneas o con grandes volúmenes de datos que copiar, considere la posibilidad de [escalar horizontalmente Integration Runtime autohospedado](create-self-hosted-integration-runtime.md#high-availability-and-scalability) para aprovisionar más recursos para ampliar la capacidad de copia.

## <a name="considerations-for-the-source"></a>Consideraciones sobre el origen

### <a name="general"></a>General

Asegúrese de que el almacén de datos subyacente no esté saturado con otras cargas de trabajo que se ejecutan en él.

Para información sobre los almacenes de datos de Microsoft, consulte los [temas sobre supervisión y optimización](#performance-reference) específicos de los almacenes de datos. Estos temas pueden ayudarle a comprender las características de rendimiento de los almacenes de datos y a saber cómo reducir los tiempos de respuesta y aumentar la capacidad de proceso.

* Si copia los datos de Blob Storage a SQL Data Warehouse, considere la posibilidad de usar PolyBase para mejorar el rendimiento. Para más información, consulte [Uso de PolyBase para cargar datos en Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Si copia datos de HDFS a Azure Blob Storage o Azure Data Lake Store, considere la posibilidad de usar DistCp para mejorar el rendimiento. Para más información, consulte [Uso de DistCp para copiar datos desde HDFS](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Si copia datos de Redshift a Azure SQL Data Warehouse, Azure Blob Storage o Azure Data Lake Store, considere la posibilidad de usar UNLOAD para potenciar el rendimiento. Para más información, consulte [Uso de UNLOAD para copiar datos de Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Almacenes de datos basados en archivos

* **Tamaño de archivo promedio y recuento de archivos**: la actividad de copia transfiere los datos de archivo en archivo. Siendo la misma cantidad de datos la que se va a mover, el rendimiento general es menor si los datos constan de muchos archivos pequeños en lugar de algunos grandes, debido a la fase de arranque de cada archivo. Si es posible, combine archivos pequeños para formar otros mayores, con el fin de obtener una mayor capacidad de procesamiento.
* **Formato de archivo y compresión**: para ver más formas de mejorar el rendimiento, consulte las secciones [Consideraciones sobre serialización y deserialización](#considerations-for-serialization-and-deserialization) y [Consideraciones sobre la compresión](#considerations-for-compression).

### <a name="relational-data-stores"></a>Almacenes de datos relacionales

* **Patrón de datos**: El esquema de tabla afecta al rendimiento de la copia. Un tamaño de fila grande ofrece un mejor rendimiento que un tamaño de fila pequeño para copiar la misma cantidad de datos. El motivo es que la base de datos puede recuperar más eficazmente menos lotes de datos que contienen menos filas.
* **Consulta o procedimiento almacenado**: optimice la lógica de la consulta o del procedimiento almacenado que se especifica en el origen de la actividad de copia para que capture los datos de forma más eficiente.

## <a name="considerations-for-the-sink"></a>Consideraciones sobre el receptor

### <a name="general"></a>General

Asegúrese de que el almacén de datos subyacente no esté saturado con otras cargas de trabajo que se ejecutan en él.

Para información sobre los almacenes de datos de Microsoft, consulte los [temas sobre supervisión y optimización](#performance-reference) específicos de los almacenes de datos. Estos temas pueden ayudarle a comprender las características de rendimiento de los almacenes de datos y a saber cómo reducir los tiempos de respuesta y aumentar la capacidad de proceso.

* Si copia los datos de cualquier almacén de datos a SQL Data Warehouse, considere la posibilidad de uso de PolyBase para mejorar el rendimiento. Para más información, consulte [Uso de PolyBase para cargar datos en Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Si copia datos de HDFS a Azure Blob Storage o Azure Data Lake Store, considere la posibilidad de usar DistCp para mejorar el rendimiento. Para más información, consulte [Uso de DistCp para copiar datos desde HDFS](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Si copia datos de Redshift a Azure SQL Data Warehouse, Azure Blob Storage o Azure Data Lake Store, considere la posibilidad de usar UNLOAD para potenciar el rendimiento. Para más información, consulte [Uso de UNLOAD para copiar datos de Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Almacenes de datos basados en archivos

* **Comportamiento de copia**: si copia datos de otro almacén de datos basado en archivos, la actividad de copia presenta tres opciones mediante la propiedad **copyBehavior**. Conserva la jerarquía, aplana la jerarquía o combina archivos. Conservar o aplanar la jerarquía supone poca o ninguna sobrecarga sobre el rendimiento, mientras que combinar archivos hace que aumente dicha sobrecarga.
* **Formato de archivo y compresión**: para ver más formas de mejorar el rendimiento, consulte las secciones [Consideraciones sobre serialización y deserialización](#considerations-for-serialization-and-deserialization) y [Consideraciones sobre la compresión](#considerations-for-compression).

### <a name="relational-data-stores"></a>Almacenes de datos relacionales

* **Comportamiento de la copia e implicaciones de rendimiento**: Hay diferentes maneras de escribir datos en un receptor de SQL. Más información en [Procedimiento recomendado para cargar datos en Azure SQL Database](connector-azure-sql-database.md#best-practice-for-loading-data-into-azure-sql-database).

* **Patrón de datos y tamaño de lote**:
  * El esquema de tabla afecta al rendimiento de la copia. Para copiar la misma cantidad de datos, un tamaño de fila grande proporcionará un mejor rendimiento que un tamaño de fila pequeño porque la base de datos puede confirmar de forma más eficiente menos lotes de datos.
  * La actividad de copia inserta datos en una serie de lotes. Puede establecer el número de filas en un lote mediante la propiedad **writeBatchSize** . Si los datos tienen filas pequeñas, puede establecer la propiedad **writeBatchSize** con un valor más alto para beneficiarse de una menor sobrecarga de los lotes y un mayor rendimiento. Si el tamaño de fila de los datos es grande, tenga cuidado al aumentar **writeBatchSize**. Un valor alto podría provocar un error de copia provocado por la sobrecarga de la base de datos.

### <a name="nosql-stores"></a>Almacenes NoSQL

* Para **Almacenamiento de tablas**:
  * **Partición**: escribir datos en particiones intercaladas degrada considerablemente el rendimiento. Ordene los datos de origen por la clave de partición para que los datos se inserten de forma eficiente en una partición después de otra. O bien, puede ajustar la lógica para escribir los datos en una sola partición.

## <a name="considerations-for-serialization-and-deserialization"></a>Consideraciones sobre serialización y deserialización

La serialización y deserialización pueden tener lugar cuando el conjunto de datos de entrada o el conjunto de datos de salida es un archivo. Para más información acerca de los formatos de archivo que se admiten, consulte [Formatos de archivo y códecs de compresión admitidos](supported-file-formats-and-compression-codecs.md).

**Comportamiento de copia**:

* Al copiar archivos entre almacenes de datos basados en archivos:
  * Cuando los conjuntos de datos de entrada y de salida tienen la misma configuración en cuanto al formato de los archivos, o carecen de dicha configuración, el servicio de movimiento de datos ejecuta una *copia binaria* sin ninguna serialización o deserialización. En este caso, el rendimiento es mayor en comparación con el escenario en el que la configuración del formato de archivo de origen y receptor es diferente.
  * Cuando los conjuntos de datos de entrada y salida están en formato de texto y solo es diferente el tipo de codificación, el servicio de movimiento de datos solo realiza la conversión de la codificación. No se realiza ninguna serialización y deserialización, lo que provoca cierta sobrecarga en el rendimiento en comparación con una copia binaria.
  * Cuando los conjuntos de datos de entrada y salida tienen formatos de archivo diferentes o configuraciones diferentes, por ejemplo, los delimitadores, el servicio de movimiento de datos deserializa los datos de origen para transmitirlos, transformarlos y luego serializarlos en el formato de salida indicado. Esta operación da como resultado una sobrecarga mucho mayor sobre el rendimiento en comparación con otros escenarios.
* Al copiar archivos a un almacén de datos, o desde él, que no se basa en archivos, por ejemplo, desde un almacén basado en archivos hasta un almacén relacional, se requiere el paso de serialización o deserialización. Este paso produce una sobrecarga considerable sobre el rendimiento.

**Formato de archivo**: el formato de archivo que elija puede afectar al rendimiento de la copia. Por ejemplo, Avro es un formato binario compacto que almacena metadatos con datos. Es ampliamente admitido para el procesamiento y la realización de consultas en el ecosistema de Hadoop. Avro resulta más caro para la serialización y deserialización, lo que da lugar a un menor rendimiento de la copia, en comparación con el formato de texto. 

Elija el formato de archivo para el flujo de procesamiento de forma holística. Comience por:

- La forma en que se almacenan los datos en los almacenes de datos de origen o en que se van a extraer de sistemas externos.
- El mejor formato para el almacenamiento, el procesamiento analítico y la realización de consultas.
- En qué formato se deben exportar los datos de data marts para las herramientas de visualización y generación de informes.

A veces, un formato de archivo que no es óptimo desde el punto de vista del rendimiento de lectura y escritura podría ser una buena opción al considerar el proceso analítico en general.

## <a name="considerations-for-compression"></a>Consideraciones sobre la compresión

Cuando el conjunto de datos de entrada o salida es un archivo, se puede establecer que la actividad de copia realice la compresión y descompresión de los datos a medida que los escribe en el destino. Si elige compresión, está buscando el equilibrio entre entrada/salida (E/S) y CPU. La compresión de los datos supone un costo adicional en recursos de proceso. Pero, a cambio, se reduce la E/S de red y el almacenamiento. En función de los datos, puede que observe un aumento en el rendimiento general de la copia.

**Códec**: Cada códec de compresión tiene sus ventajas. Por ejemplo, bzip2 tiene el rendimiento de copia más bajo, pero obtiene el mejor rendimiento de consulta de Hive porque puede dividirlo para el procesamiento. Gzip es la opción más equilibrada y la que se usa con mayor frecuencia. Elija el códec que mejor se adapte a su escenario de principio a fin.

**Nivel**: para cada códec de compresión, puede elegir entre dos opciones: compresión más rápida y compresión más óptima. La operación de compresión más rápida comprime lo antes posible, aunque ello suponga que la compresión del archivo resultante no sea óptima. La opción de compresión óptima dedica más tiempo a la compresión y produce una cantidad mínima de datos. Puede probar ambas opciones para ver cuál proporciona un mejor rendimiento general en su caso.

**Una consideración**: para copiar una gran cantidad de datos entre un almacén local y la nube, considere usar la [copia almacenada provisionalmente](#staged-copy) con compresión habilitada. El uso del almacenamiento provisional resulta útil cuando el ancho de banda de la red corporativa y los servicios de Azure son el factor limitador, y se desea que tanto el conjunto de datos de entrada como el de salida estén en formato sin comprimir.

## <a name="considerations-for-column-mapping"></a>Consideraciones sobre la asignación de columnas

La propiedad **columnMappings** se puede establecer en una actividad de copia para asignar todas las columnas de entrada, o un subconjunto de ellas, a las columnas de salida. Después de que el servicio de movimiento de datos lee los datos del origen, debe realizar la asignación de columnas en los datos antes de escribirlos en el receptor. Este procesamiento adicional reduce la capacidad de proceso de la copia.

Si el almacén de datos de origen es consultable, por ejemplo, si es un almacén relacional, como SQL Database o SQL Server, o es un almacén NoSQL como Table Storage o Azure Cosmos DB, considere la posibilidad de insertar la lógica de filtrado y reordenación de columnas en la propiedad **query**, en lugar de usar la asignación de columnas. De esta forma, la proyección se produce mientras el servicio de movimiento de datos lee los datos del almacén de datos de origen, donde es mucho más eficaz.

Más información en [Asignación de esquemas de la actividad de copia](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Otras consideraciones

Si el tamaño de los datos que desea copiar es grande, puede ajustar la lógica empresarial para particionar los datos aún más. Puede programar la actividad de copia para que se ejecute con mayor frecuencia y así reducir el tamaño de los datos con cada ejecución de actividad de copia.

Tenga cuidado con el número de conjuntos de datos y actividades de copia que necesitan Azure Data Factory para conectarse al mismo almacén de datos a la vez. Muchos trabajos de copia simultáneos podrían limitar un almacén de datos y llevar a una degradación en el rendimiento, reintentos internos de trabajos de copia y, en ocasiones, errores de ejecución.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Escenario de ejemplo: Copia de una instancia local de SQL Server a Blob Storage

**Escenario**: se crea una canalización para copiar datos de una instancia de SQL Server local a Blob Storage en formato CSV. Para acelerar el trabajo de copia, los archivos CSV se deben comprimir en formato bzip2.

**Análisis y pruebas**: el rendimiento de la actividad de copia es inferior a 2 MBps, que es mucho más lento que la referencia de rendimiento.

**Análisis y ajuste del rendimiento**: para solucionar el problema de rendimiento, primero veremos cómo se procesan y se mueven los datos.

- **Lectura de datos**: Integration Runtime abre una conexión a SQL Server y envía la consulta. SQL Server responde enviando el flujo de datos a Integration Runtime a través de la intranet.
- **Serialización y compresión de datos**: Integration Runtime serializa el flujo de datos en formato CSV y comprime los datos en una secuencia bzip2.
- **Escritura de datos**: Integration Runtime carga la secuencia bzip2 en Blob Storage a través de Internet.

Como puede ver, los datos se procesan y se mueven en streaming de manera secuencial: SQL Server > LAN > Integration Runtime > WAN > Blob Storage. El rendimiento general viene determinado por el procesamiento mínimo a través de la canalización.

![flujo de datos](./media/copy-activity-performance/case-study-pic-1.png)

Puede que uno o varios de los siguientes factores provoquen el cuello de botella en el rendimiento:

* **Origen**: el propio SQL Server tiene un rendimiento bajo debido a cargas intensas.
* **Integration Runtime autohospedado**:
  * **LAN**: Integration Runtime se encuentra lejos de la máquina de SQL Server y tiene una conexión de ancho de banda bajo.
  * **Integration Runtime**: Integration Runtime alcanzó sus limitaciones de carga para llevar a cabo las siguientes operaciones:
    * **Serialización**: la serialización del flujo de datos a formato CSV tiene un rendimiento bajo.
    * **Compresión**: ha elegido un códec de compresión lenta (por ejemplo, bzip2, que funciona a 2,8 MBps con un procesador i7).
  * **WAN**: el ancho de banda entre la red corporativa y los servicios de Azure es bajo, por ejemplo, T1 = 1544 kbps. T2 = 6312 kbps.
* **Receptor**: Blob Storage tiene un rendimiento bajo. Este escenario es improbable porque su contrato de nivel de servicio garantiza un mínimo de 60 MBps.

En este caso, la compresión de datos bzip2 podría estar ralentizando la canalización entera. El cambio a un códec de compresión gzip podría aliviar este cuello de botella.

## <a name="references"></a>Referencias

Estas son algunas referencias para la supervisión y la optimización del rendimiento para algunos de los almacenes de datos admitidos:

* Azure Storage, que incluye Blob Storage y Table Storage: [Objetivos de escalabilidad de Azure Storage](../storage/common/storage-scalability-targets.md) y [Lista de comprobación de rendimiento y escalabilidad de Azure Storage](../storage/common/storage-performance-checklist.md).
* Azure SQL Database: puede [supervisar el rendimiento](../sql-database/sql-database-single-database-monitor.md) y comprobar el porcentaje de la unidad de transacción de base de datos (DTU).
* Azure SQL Data Warehouse: su funcionalidad se mide en unidades de Data Warehouse (DWU). Consulte [Administración de la potencia de proceso en Azure SQL Data Warehouse (información general)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Niveles de rendimiento en Azure Cosmos DB](../cosmos-db/performance-levels.md).
* SQL Server local: [Supervisión y optimización del rendimiento](https://msdn.microsoft.com/library/ms189081.aspx).
* Servidor de archivos local: [Performance tuning for file servers](https://msdn.microsoft.com/library/dn567661.aspx) (Ajuste del rendimiento para los servidores de archivos).

## <a name="next-steps"></a>Pasos siguientes
Consulte los restantes artículos acerca de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Asignación de esquemas en la actividad de copia](copy-activity-schema-and-type-mapping.md)
- [Tolerancia a errores de la actividad de copia](copy-activity-fault-tolerance.md)
