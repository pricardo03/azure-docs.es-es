---
title: Guía de escalabilidad y rendimiento de la actividad de copia
description: Conozca los factores más importantes que afectan al rendimiento del movimiento de datos en Azure Data Factory cuando se usa la actividad de copia.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/24/2019
ms.openlocfilehash: 28d0da369083d75bc175111d808828e186a366fc
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78358377"
---
# <a name="copy-activity-performance-and-scalability-guide"></a>Guía de escalabilidad y rendimiento de la actividad de copia

> [!div class="op_single_selector" title1="Seleccione la versión de Azure Data Factory que usa:"]
> * [Versión 1](v1/data-factory-copy-activity-performance.md)
> * [Versión actual](copy-activity-performance.md)

Si quiere realizar una migración de datos a gran escala desde un lago de datos o un almacenamiento de datos empresarial (EDW) a Azure, o si quiere ingerir datos a escala desde distintos orígenes en Azure para análisis de macrodatos, es fundamental lograr un rendimiento y escalabilidad óptimos.  Azure Data Factory proporciona un mecanismo eficaz, resistente y rentable para ingerir datos a escala, lo que lo convierte en una buena opción para los ingenieros de datos que buscan crear canalizaciones de ingesta de datos de alto rendimiento y escalabilidad.

Después de leer este artículo, podrá responder a las siguientes preguntas:

- ¿Qué nivel de rendimiento y escalabilidad puedo conseguir si uso la actividad de copia de ADF para escenarios de migración de datos y de ingesta de datos?

- ¿Qué pasos se deben seguir para optimizar el rendimiento de la actividad de copia de ADF?
- ¿Qué mecanismos de optimización del rendimiento de ADF puedo utilizar para optimizar el rendimiento de una única ejecución de la actividad de copia?
- ¿Qué otros factores ajenos a ADF se deben tener en cuenta al optimizar el rendimiento de la copia?

> [!NOTE]
> Si no está familiarizado con la actividad de copia en general, consulte la [información general de la actividad de copia](copy-activity-overview.md) antes de leer este artículo.

## <a name="copy-performance-and-scalability-achievable-using-adf"></a>Rendimiento y escalabilidad de copia que pueden lograrse mediante ADF

ADF ofrece una arquitectura sin servidor que permite paralelismo en diferentes niveles, lo que permite a los desarrolladores crear canalizaciones para aprovechar al máximo el ancho de banda de red, así como el ancho de banda y las IOPS de almacenamiento para maximizar el rendimiento del movimiento de datos para su entorno.  Esto significa que el rendimiento que puede lograr puede calcularse al medir el rendimiento mínimo ofrecido por el almacén de datos de origen, el almacén de datos de destino y el ancho de banda de red entre el origen y el destino.  En la tabla siguiente se calcula la duración de la copia en función del tamaño de los datos y el límite de ancho de banda de su entorno. 

| Tamaño de los datos / <br/> bandwidth | 50 Mbps    | 100 Mbps  | 500 Mbps  | 1 Gbps   | 5 Gbps   | 10 Gbps  | 50 Gbps   |
| --------------------------- | ---------- | --------- | --------- | -------- | -------- | -------- | --------- |
| **1 GB**                    | 2,7 min    | 1,4 min   | 0,3 min   | 0,1 min  | 0,03 min | 0,01 min | 0,0 min   |
| **10 GB**                   | 27,3 min   | 13,7 min  | 2,7 min   | 1,3 min  | 0,3 min  | 0,1 min  | 0,03 min  |
| **100 GB**                  | 4,6 h    | 2,3 h   | 0,5 h   | 0,2 h  | 0,05 h | 0,02 h | 0,0 h   |
| **1 TB**                    | 46,6 h   | 23,3 h  | 4,7 h   | 2,3 h  | 0,5 h  | 0,2 h  | 0,05 h  |
| **10 TB**                   | 19,4 días  | 9,7 días  | 1,9 días  | 0,9 días | 0,2 días | 0,1 días | 0,02 días |
| **100 TB**                  | 194,2 días | 97,1 días | 19,4 días | 9,7 días | 1,9 días | 1 día   | 0,2 días  |
| **1 PB**                    | 64,7 meses    | 32,4 meses   | 6,5 meses    | 3,2 meses   | 0,6 meses   | 0,3 meses   | 0,06 meses   |
| **10 PB**                   | 647,3 meses   | 323,6 meses  | 64,7 meses   | 31,6 meses  | 6,5 meses   | 3,2 meses   | 0,6 meses    |

La copia de ADF puede escalarse en diferentes niveles:

![Cómo se escala la copia de ADF](media/copy-activity-performance/adf-copy-scalability.png)

- El flujo de control de ADF puede iniciar varias actividades de copia en paralelo, por ejemplo, mediante un [bucle ForEach](control-flow-for-each-activity.md).
- Una única actividad de copia puede aprovechar recursos de proceso escalables: si usa Azure Integration Runtime, puede especificar [hasta 256 DIU](#data-integration-units) para cada actividad de copia en modo sin servidor; si usa un entorno de ejecución de integración autohospedado, puede escalar verticalmente la máquina o escalar horizontalmente a varias máquinas ([hasta 4 nodos](create-self-hosted-integration-runtime.md#high-availability-and-scalability)) manualmente y una única actividad de copia creará particiones de su conjunto de archivos en todos los nodos.
- Una única actividad de copia lee y escribe en el almacén de datos mediante varios subprocesos [en paralelo](#parallel-copy).

## <a name="performance-tuning-steps"></a>Pasos de optimización del rendimiento

Para optimizar el rendimiento del servicio Azure Data Factory con la actividad de copia, siga estos pasos.

1. **Seleccione un conjunto de datos de prueba y establezca una línea de base**. Durante la fase de desarrollo, pruebe la canalización, para lo que debe usar la actividad de copia con unos datos de ejemplo representativos. El conjunto de datos que elija debe representar los patrones de datos habituales (estructura de carpetas, patrón de archivo, esquema de datos, etc.) y es lo suficientemente grande como para evaluar el rendimiento de las copias; por ejemplo, tarda 10 minutos o más en completarse la actividad de copia. Recopile los detalles de la ejecución y las características del rendimiento después de la [supervisión de la actividad de copia](copy-activity-overview.md#monitoring).

2. **Cómo maximizar el rendimiento de una única actividad de copia**:

   Para empezar, se recomienda maximizar primero el rendimiento mediante una única actividad de copia.

   **Si la actividad de copia se va a ejecutar en una instancia de Azure Integration Runtime:**

   Comience con los valores predeterminados de las [unidades de integración de datos (DIU)](#data-integration-units) y la configuración de [copia paralela](#parallel-copy).  Realice una serie de pruebas de rendimiento y tome nota del rendimiento conseguido, así como de los valores reales usados para las DIU y las copias paralelas.  Consulte la [supervisión de la actividad de copia](copy-activity-overview.md#monitoring) para obtener información sobre cómo recopilar los resultados de las pruebas y la configuración de rendimiento.

   Ahora realice una serie de pruebas de rendimiento adicionales y duplique el valor de la configuración de DIU cada vez.  Como alternativa, si cree que el rendimiento conseguido con la configuración predeterminada es mucho menor que el esperado, puede aumentar la configuración de DIU de forma más drástica en las pruebas siguientes.

   La actividad de copia debe escalarse casi perfectamente de forma lineal a medida que aumenta el valor de DIU.  Si duplica el valor de DIU, pero el rendimiento no parece duplicarse, puede tener una de dos causas:

   - El patrón de copia específico que se está ejecutando no se beneficia de agregar más DIU.  Aunque especificó un valor de DIU mayor, el DIU real usado sigue siendo el mismo y, por tanto, se obtiene el mismo rendimiento que antes.  En este caso, maximice el rendimiento agregado mediante la ejecución de varias copias de manera simultánea en referencia al paso 3.
   - Si agrega más DIU (más potencia) y, por lo tanto, ocasiona una mayor tasa de extracción, transferencia y carga de datos, el almacén de datos de origen, la red intermedia o el almacén de datos de destino alcanzaron un cuello de botella y posiblemente se están limitando.  Si este es el caso, intente ponerse en contacto con el administrador del almacén de datos o con el administrador de red para aumentar el límite. O bien reduzca el valor de DIU hasta que se produzca la limitación.

   **Si la actividad de copia se va a ejecutar en un entorno de ejecución de integración autohospedado:**

   se recomienda usar un equipo dedicado independiente del servidor que hospeda el almacén de datos para hospedar el entorno de ejecución de integración.

   Comience con los valores predeterminados para la configuración de [copia en paralelo](#parallel-copy) y use un solo nodo para el IR autohospedado.  Realice una serie de pruebas de rendimiento y tome nota del rendimiento conseguido.

   Si quiere lograr un mayor rendimiento, puede escalar verticalmente u horizontalmente el IR autohospedado:

   - si la memoria disponible y la CPU del nodo de IR autohospedado no se utilizan totalmente, pero la ejecución de trabajos simultáneos está alcanzando el límite, debe realizar un escalado vertical aumentando el número de trabajos simultáneos que se pueden ejecutar en un nodo.  Consulte [este enlace](create-self-hosted-integration-runtime.md#scale-up) para obtener instrucciones.
   - Por otro lado, si la CPU tiene un uso elevado en el nodo de IR autohospedado o la memoria disponible es baja, puede agregar un nuevo nodo para ayudar a escalar horizontalmente la carga en varios nodos.  Consulte [este enlace](create-self-hosted-integration-runtime.md#high-availability-and-scalability) para obtener instrucciones.

   A medida que escale verticalmente u horizontalmente la capacidad del IR autohospedado, repita la serie de pruebas de rendimiento para ver si obtiene un rendimiento cada vez mayor.  Si el rendimiento deja de mejorar, lo más probable es que el almacén de datos de origen, la red intermedia o el almacén de datos de destino hayan un cuello de botella y estén comenzado a limitarse. Si este es el caso, intente ponerse en contacto con el administrador del almacén de datos o con el administrador de red para aumentar el límite. O bien vuelva a la configuración de escalado anterior del IR autohospedado. 

3. **Cómo maximizar el rendimiento agregado mediante la ejecución de varias copias simultáneamente:**

   Ahora que ha maximizado el rendimiento de una única actividad de copia, si aún no ha logrado alcanzar los límites de rendimiento de su entorno (red, almacén de datos de origen y almacén de datos de destino), puede ejecutar varias actividades de copia en paralelo mediante las construcciones de flujo de control de ADF, como el [bucle For Each](control-flow-for-each-activity.md).

4. **Sugerencias de optimización del rendimiento y características de optimización**. En algunos casos, cuando se ejecuta una actividad de copia en Azure Data Factory, se ve el mensaje "Sugerencias para la optimización del rendimiento" en la parte superior de la [supervisión de actividad de copia](copy-activity-overview.md#monitor-visually), como se muestra en el ejemplo siguiente. El mensaje indica que se identificó un cuello de botella para la ejecución de una copia determinada. También sirve de guía para realizar los cambios necesarios para mejorar el rendimiento de la copia. Las sugerencias de optimización de rendimiento actualmente proporcionan sugerencias como:

   - Use PolyBase cuando copie datos en Azure SQL Data Warehouse.
   - Aumente las unidades de solicitud de Azure Cosmos DB o las DTU (unidades de procesamiento de base de datos) de Azure SQL Database cuando el recurso del almacén de datos es el cuello de botella.
   - Quite la copia almacenada provisionalmente innecesaria.

   Las reglas de optimización del rendimiento también se enriquecerán de forma gradual.

   **Ejemplo: copia en Azure SQL DB con sugerencias de optimización del rendimiento**

   En este ejemplo, durante la ejecución de una copia, Azure Data Factory observa que la instancia de Azure SQL Database receptora alcanza una utilización elevada de la unidad de procesamiento de base de datos, lo que ralentiza las operaciones de escritura. La sugerencia es aumentar el nivel de Azure SQL Database con más DTU. 

   ![Supervisión de copia con sugerencias de optimización del rendimiento](media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

   Además, las siguientes son algunas características de optimización del rendimiento que debe tener en cuenta:

   - [Copia paralela](#parallel-copy)
   - [Unidades de integración de datos](#data-integration-units)
   - [Copias almacenadas provisionalmente](#staged-copy)
   - [Escalabilidad de Integration Runtime autohospedado](concepts-integration-runtime.md#self-hosted-integration-runtime)

5. **Expanda la configuración a todo el conjunto de datos.** Cuando esté satisfecho con los resultados y el rendimiento de la ejecución, puede expandir la definición y la canalización para cubrir todo el conjunto de datos.

## <a name="copy-performance-optimization-features"></a>Características de optimización del rendimiento

Azure Data Factory proporciona las siguientes características de optimización del rendimiento:

- [Copia paralela](#parallel-copy)
- [Unidades de integración de datos](#data-integration-units)
- [Copias almacenadas provisionalmente](#staged-copy)

### <a name="data-integration-units"></a>Unidades de integración de datos

Una unidad de integración de datos es una medida que representa la eficacia (una combinación de CPU, memoria y asignación de recursos de red) de una única unidad en Azure Data Factory. Una unidad de integración de datos solo se aplica a [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime), pero no a [Integration Runtime autohospedado](concepts-integration-runtime.md#self-hosted-integration-runtime).

Se le cobrará **el n.° de DIU usadas\* la duración de la copia \* el precio unitario/hora de DIU**. Consulte [aquí](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) los precios actuales. La moneda local y el descuento independiente pueden aplicarse según el tipo de suscripción.

El número permitido de DIU para impulsar la ejecución de una actividad de copia se encuentra **entre 2 y 256**. Si no se especifica o elige "automático" en la interfaz de usuario, Data Factory aplicará dinámicamente la configuración de DIU óptima según el modelo de datos y el par origen-receptor. En la tabla siguiente se muestran las DIU predeterminadas que se usan en distintos escenarios de copia.

| Escenario de copia | DIU predeterminadas que determina el servicio |
|:--- |:--- |
| Copia de datos entre almacenes basados en archivos | Entre 4 y 32, según el número y tamaño de los archivos |
| Copia de datos en Azure SQL Database o Azure Cosmos DB |Entre 4 y 16 en función del nivel de Azure SQL Database o Cosmos DB (número de RU o DTU) del receptor |
| Los restantes escenarios de copia | 4 |

Para reemplazar esta configuración predeterminada, especifique un valor para la propiedad **dataIntegrationUnits** de la manera siguiente. El *número real de DIU* que usa la operación de copia en tiempo de ejecución es igual o inferior al valor configurado, según el patrón de datos.

El número de unidades de integración de datos utilizadas en cada ejecución de copia se puede ver en la salida de la actividad de copia al supervisar una ejecución de actividad. Para más información, consulte el artículo [Supervisión de la actividad de copia](copy-activity-overview.md#monitoring).

> [!NOTE]
> Actualmente, un valor de DIU superior a cuatro solo se aplica cuando se copian varios archivos desde Azure Blob/ADLS Gen1/ADLS Gen2/Amazon S3/Google Cloud Storage/FTP en la nube/SFTP en la nube o desde un almacén de datos relacional en la nube con la opción de partición habilitada (incluidos [Oracle](connector-oracle.md#oracle-as-source)/[Netezza](connector-netezza.md#netezza-as-source)/[Teradata](connector-teradata.md#teradata-as-source)) a cualquier otro almacén de datos en la nube.

**Ejemplo**:

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

### <a name="parallel-copy"></a>Copia en paralelo

Puede usar la propiedad **parallelCopies** para indicar el paralelismo que desea que use la actividad de copia. Esta propiedad se puede considerar como el número máximo de subprocesos dentro de la actividad de copia que se pueden leer del origen o escribir en los almacenes de datos receptores en paralelo.

Para cada ejecución de una actividad de copia, Azure Data Factory determina el número de copias paralelas que se usarán para copiar datos del almacén de datos de origen al de destino. El número predeterminado de copias en paralelo que utilice depende del tipo de origen y receptor que se use.

| Escenario de copia | Recuento predeterminado de copias en paralelo determinado por servicio |
| --- | --- |
| Copia de datos entre almacenes basados en archivos |Depende del tamaño de los archivos y del número de unidades de integración de datos (DIU) que se usen para copiar datos entre dos almacenes de datos en la nube, o de la configuración física de la máquina en que se encuentre Integration Runtime autohospedado. |
| Copie desde el almacén de datos relacional con la opción de partición habilitada (incluyendo [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source) y [Open Hub de SAP](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)).|4 |
| Copia de datos desde cualquier almacén de origen a Azure Table Storage |4 |
| Todos los demás escenarios de copia |1 |

> [!TIP]
> Al copiar datos entre almacenes basados en archivos, normalmente el comportamiento predeterminado es el que ofrece el mejor rendimiento. El comportamiento predeterminado es determina automáticamente en función del patrón del archivo de origen.

Para controlar la carga en las máquinas que hospedan los almacenes de datos o para optimizar el rendimiento de la copia puede reemplazar el valor predeterminado y especificar un valor para la propiedad **parallelCopies**. El valor debe ser un entero mayor o igual que 1. En tiempo de ejecución, y para obtener el mejor rendimiento, la actividad de copia usa un valor inferior o igual al valor que ha establecido.

**Puntos a tener en cuenta:**

- Cuando se copian datos entre almacenes basados en archivos, **parallelCopies** determina el paralelismo en el nivel de archivo. La fragmentación dentro de un archivo individual se produce por debajo de forma automática y transparente. Está diseñada para utilizar el tamaño de fragmento más adecuado para un tipo de almacén de datos de origen determinado para cargar datos en paralelo y ortogonales a **parallelCopies**. El número real de copias en paralelo que usa el servicio de movimiento de datos para la operación de copia en tiempo de ejecución no es superior al número de archivos que tenga. Si el comportamiento de copia es **mergeFile**, la actividad de copia no puede aprovechar las ventajas del paralelismo de nivel de archivo.
- Al copiar datos de almacenes no basados en archivos (salvo si el conector de [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source) y [Open Hub de SAP](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source) es el origen y tiene habilitada la creación de particiones de datos) en almacenes que sí se basan en archivos, el servicio de movimiento de datos omite la propiedad **parallelCopies**. Aunque se especifica el paralelismo, no se aplica en este caso.
- La propiedad **parallelCopies** es ortogonal para **dataIntegrationUnits**. La información anterior se cuenta en todas las unidades de integración de datos.
- Cuando especifique un valor para la propiedad **parallelCopies**, tenga en cuenta el aumento de la carga en los almacenes de datos de origen y receptor. Tenga también en cuenta el aumento de carga para IR autohospedado si la actividad de copia tiene autorización, por ejemplo, para la copia híbrida. Dicho aumento sucede especialmente si tiene varias actividades o ejecuciones simultáneas de las mismas actividades que se ejecutan en el mismo almacén de datos. Si observa que el almacén de datos o Integration Runtime autohospedado están sobrecargados, disminuya el valor de **parallelCopies** para aliviar la carga.

**Ejemplo**:

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

### <a name="staged-copy"></a>copia almacenada provisionalmente

Al copiar datos de un almacén de datos de origen a un almacén de datos receptor, podría elegir usar Almacenamiento de blobs como almacenamiento provisional. El almacenamiento provisional es especialmente útil en los siguientes casos:

- **Desea ingerir datos de varios almacenes de datos en SQL Data Warehouse mediante PolyBase.** SQL Data Warehouse emplea PolyBase como mecanismo de alto rendimiento para cargar una gran cantidad de datos en SQL Data Warehouse. Los datos de origen deben estar en Blob Storage o Azure Data Lake Store y deben satisfacer criterios adicionales. Al cargar datos desde un almacén de datos distinto de Blob Storage o Azure Data Lake Store, puede activar la copia de datos mediante Blob Storage de almacenamiento provisional. En ese caso, Azure Data Factory realiza las transformaciones de datos necesarias para garantizar que se cumplen los requisitos de PolyBase. A continuación, se usa PolyBase para cargar datos en SQL Data Warehouse de manera eficaz. Para más información, consulte [Uso de PolyBase para cargar datos en Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **En ocasiones, se tarda un tiempo en realizar un movimiento de datos híbridos (es decir, copiar desde un almacén de datos local a un almacén de datos en la nube) a través de una conexión de red lenta.** Para mejorar el rendimiento se puede usar una copia almacenada provisionalmente para comprimir los datos de forma local, con el fin de que se tarde menos tiempo en mover datos al almacén de datos provisional en la nube. Luego, puede descomprimir los datos en dicho almacén antes de cargarlos en el almacén de datos de destino.
- **Solo desea abrir los puertos 80 y el 443 en el firewall, a causa de las directivas de TI corporativas**. Por ejemplo, al copiar datos de un almacén de datos local a un receptor de Azure SQL Database o a un receptor de Azure SQL Data Warehouse, debe activar la comunicación TCP saliente en el puerto 1433 tanto para el firewall de Windows como para el firewall corporativo. En ese escenario, una copia almacenada provisionalmente puede aprovechar la ventaja de IR autohospedado para copiar primero los datos en una instancia provisional de Blob Storage a través de HTTP o HTTPS en el puerto 443. Luego, puede cargar dichos datos en SQL Database o en SQL Data Warehouse desde la instancia provisional de Blob Storage. En este flujo, no es necesario habilitar el puerto 1433.

#### <a name="how-staged-copy-works"></a>Funcionamiento de las copias almacenadas provisionalmente

Al activar la característica de almacenamiento provisional, primero se copian los datos desde el almacén de datos de origen a la instancia de Blob Storage provisional (use su propio). A continuación, los datos se copian desde el almacén de datos provisional al almacén de datos receptor. Azure Data Factory administra automáticamente el flujo de dos fases. Azure Data Factory también limpia los datos temporales del almacenamiento provisional cuando se completa el movimiento de los datos.

![copia almacenada provisionalmente](media/copy-activity-performance/staged-copy.png)

Cuando activa el movimiento de datos mediante un almacén provisional, puede especificar si quiere que los datos se compriman antes de moverlos del almacén de datos de origen a uno provisional o intermedio, y luego descomprimirlos antes de moverlos desde este último al almacén de datos receptor.

Actualmente, no es posible copiar datos entre dos almacenes de datos que estén conectados a través de distintos IR autohospedados, ni con copia almacenada provisionalmente ni sin ella. Para dicho escenario, puede configurar dos actividades de copia explícitamente encadenadas, con el fin de copiar desde el origen al almacenamiento provisional y, después, desde este al receptor.

#### <a name="configuration"></a>Configuración

Configure el valor **enableStaging** de la actividad de copia para especificar si desea que los datos se almacenen provisionalmente en Blob Storage antes de cargarlos en un almacén de datos de destino. Cuando establezca **enableStaging** en `TRUE`, especifique las propiedades adicionales que se muestran en la tabla siguiente. Si no tiene un servicio vinculado a la firma de acceso compartido de Azure Storage o de Storage, debe crearlo.

| Propiedad | Descripción | Valor predeterminado | Obligatorio |
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

#### <a name="staged-copy-billing-impact"></a>Impacto en la facturación de copia almacenada provisionalmente

Los cargos que se le realizan se basan en dos pasos: duración de la copia y tipo de copia.

* Cuando use el almacenamiento provisional durante una copia en la nube, que copia de datos de un almacén de datos en la nube a otro, donde ambos almacenamientos provisionales usan Azure Integration Runtime, se le cobrará de la siguiente manera: [suma de la duración de la copia de los pasos 1 y 2] x [precio unitario de la copia en la nube].
* Cuando use el almacenamiento provisional durante una copia híbrida, que copia de datos de un almacén de datos local a uno en la nube, un almacenamiento provisional que usa Integration Runtime autohospedado, se le cobrará de la siguiente manera: [duración de la copia híbrida] x [precio unitario de la copia híbrida] + [duración de la copia de nube] x [precio unitario de la copia de nube].

## <a name="references"></a>Referencias

Estas son algunas referencias para la supervisión y la optimización del rendimiento para algunos de los almacenes de datos admitidos:

* Azure Blob Storage: [Objetivos de escalabilidad y rendimiento de Blob Storage](../storage/blobs/scalability-targets.md) y [Lista de comprobación de escalabilidad y rendimiento para Blob Storage](../storage/blobs/storage-performance-checklist.md).
* Azure Table Storage: [Objetivos de escalabilidad y rendimiento de Blob Storage](../storage/tables/scalability-targets.md) y [Lista de comprobación de rendimiento y de escalabilidad para Table Storage](../storage/tables/storage-performance-checklist.md).
* Azure SQL Database: puede [supervisar el rendimiento](../sql-database/sql-database-single-database-monitor.md) y comprobar el porcentaje de la unidad de transacción de base de datos (DTU).
* Azure SQL Data Warehouse: su funcionalidad se mide en unidades de Data Warehouse (DWU). Consulte [Administración de la potencia de proceso en Azure SQL Data Warehouse (información general)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Niveles de rendimiento en Azure Cosmos DB](../cosmos-db/performance-levels.md).
* SQL Server local: [Supervisión y optimización del rendimiento](https://msdn.microsoft.com/library/ms189081.aspx)
* Servidor de archivos local: [Performance tuning for file servers](https://msdn.microsoft.com/library/dn567661.aspx) (Ajuste del rendimiento para los servidores de archivos).

## <a name="next-steps"></a>Pasos siguientes
Consulte los restantes artículos acerca de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Uso de Azure Data Factory para migrar datos del lago de datos y el almacenamiento de datos a Azure](data-migration-guidance-overview.md)
- [Migración de datos de AWS S3 a Azure Storage](data-migration-guidance-s3-azure-storage.md)
