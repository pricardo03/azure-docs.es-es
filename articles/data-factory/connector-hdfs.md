---
title: Copia de datos desde HDFS mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde un origen HDFS —en la nube o en un entorno local— a almacenes de datos receptores compatibles a través de una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: c528f37c8970380678a318ec2d63babd37f89501
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228042"
---
# <a name="copy-data-from-hdfs-using-azure-data-factory"></a>Copia de datos de HDFS mediante Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-hdfs-connector.md)
> * [Versión actual](connector-hdfs.md)

En este artículo se describe cómo copiar datos desde el servidor HDFS. Para información sobre Azure Data Factory, lea el [artículo de introducción](introduction.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector HDFS se admite para las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [admite la matriz de origen/receptor](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

En concreto, este conector HDFS admite las siguientes funcionalidades:

- Copiar los archivos mediante la autenticación de **Windows** (Kerberos) o **anónima**.
- Copiar los archivos mediante el protocolo **webhdfs** o la herramienta **DistCp integrada**.
- Copiar los archivos tal cual, o bien analizarlos o generarlos, con los [códecs de compresión y los formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Requisitos previos

Para copiar datos desde una instancia de HDFS que no es accesible públicamente, debe configurar un entorno de ejecución de integración autohospedado. Consulte el artículo sobre el [entorno Integration Runtime (autohospedado)](concepts-integration-runtime.md) para obtener más información.

> [!NOTE]
> Asegúrese de que Integration Runtime puede obtener acceso a **TODOS** los [servidor de nodo de nombres]:[puerto de nodo de nombres] y [servidores de nodos de datos]:[puerto de nodo de datos] del clúster de Hadoop. El [puerto de nodo de nombres] predeterminado es 50070 y el [puerto de nodo de datos] es 50075.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas de HDFS.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de HDFS:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **Hdfs**. | Sí |
| url |Dirección URL a HDFS |Sí |
| authenticationType | Los valores permitidos son: **Anónima** o **Windows**. <br><br> Para usar la **autenticación Kerberos** para el conector HDFS, consulte [esta sección](#use-kerberos-authentication-for-hdfs-connector) a fin de configurar el entorno local en consecuencia. |Sí |
| userName |Nombre de usuario para la autenticación de Windows Para la autenticación Kerberos, especifique `<username>@<domain>.com`. |Sí (para la autenticación de Windows) |
| password |Contraseña para la autenticación de Windows Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |Sí (para la autenticación de Windows) |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) o Azure Integration Runtime (si el almacén de datos es accesible públicamente). Si no se especifica, se usará Azure Integration Runtime. |No |

**Ejemplo: Uso de autenticación anónima**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo: Uso de autenticación de Windows**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). 

- Para **Parquet y formato de texto delimitado**, consulte [conjunto de datos con formato Parquet y texto delimitado](#parquet-and-delimited-text-format-dataset) sección.
- Para otros formatos como **formato ORC/Avro/JSON/binario**, consulte [otro conjunto de datos de formato](#other-format-dataset) sección.

### <a name="parquet-and-delimited-text-format-dataset"></a>Conjunto de datos con formato parquet y texto delimitado

Para copiar datos desde HDFS en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo en el conjunto de datos de formato y compatibles Configuración. Las siguientes propiedades son compatibles con HDFS en `location` configuración en el conjunto de datos basado en el formato:

| Propiedad   | DESCRIPCIÓN                                                  | Obligatorio |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | La propiedad type en `location` en el conjunto de datos debe establecerse en **HdfsLocation**. | Sí      |
| folderPath | La ruta de acceso a la carpeta. Si desea usar el carácter comodín a la carpeta de filtro, omita esta configuración y especificar en la configuración del origen de actividad. | Sin        |
| fileName   | El nombre de archivo en folderPath determinado. Si desea usar el carácter comodín para filtrar los archivos, omita esta configuración y especificar en la configuración del origen de actividad. | No       |

> [!NOTE]
> **Recurso compartido de archivos** todavía se admite el tipo de conjunto de datos con formato Parquet/texto que se mencionan en la siguiente sección como-actividad de copia y búsqueda para la compatibilidad con versiones anteriores de. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HdfsLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Otro conjunto de datos de formato

Para copiar datos desde HDFS en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **FileShare** |Sí |
| folderPath | Ruta de acceso a la carpeta. Se admite el filtro de comodín, los caracteres comodín permitidos son: `*` (coincide con cero o más caracteres) y `?` (coincide con cero o carácter individual); use `^` para el escape si el nombre real del archivo tiene un carácter comodín o este carácter de escape dentro. <br/><br/>Ejemplos: rootfolder/subfolder/ver más en [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). |Sí |
| fileName |  **Filtro de nombre o de comodín** para los archivos de la ruta "folderPath" especificada. Si no especifica ningún valor para esta propiedad, el conjunto de datos apunta a todos los archivos de la carpeta. <br/><br/>Para filtrar, los caracteres comodín permitidos son: `*` (equivale a cero o a varios caracteres) y `?` (equivale a cero o a un único carácter).<br/>- Ejemplo 1: `"fileName": "*.csv"`<br/>- Ejemplo 2: `"fileName": "???20180427.txt"`<br/>Use `^` como escape si el nombre real de la carpeta contiene un carácter comodín o este carácter de escape. |No |
| modifiedDatetimeStart | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br/><br/> Tenga en cuenta que el rendimiento general del movimiento de datos se verán afectado por si habilita a esta configuración cuando desee del filtro de archivos de grandes cantidades de archivos. <br/><br/> Las propiedades pueden ser NULL que significa que no se aplicará ningún filtro de atributo de archivo para el conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora.| Sin  |
| modifiedDatetimeEnd | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br/><br/> Tenga en cuenta que el rendimiento general del movimiento de datos se verán afectado por si habilita a esta configuración cuando desee del filtro de archivos de grandes cantidades de archivos. <br/><br/> Las propiedades pueden ser NULL que significa que no se aplicará ningún filtro de atributo de archivo para el conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora.| Sin  |
| format | Si desea **copiar los archivos tal cual** entre los almacenes basados en archivos (copia binaria), omita la sección de formato en las definiciones de los conjuntos de datos de entrada y salida.<br/><br/>Si quiere analizar archivos con un formato concreto, se admiten los siguientes tipos de formato de archivo: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Establezca la propiedad **type** de formato en uno de los siguientes valores. Para más información, consulte las secciones [Formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [Formato Json](supported-file-formats-and-compression-codecs.md#json-format), [Formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) y [Formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |No (solo para el escenario de copia binaria) |
| compression | Especifique el tipo y el nivel de compresión de los datos. Para más información, consulte el artículo sobre [códecs de compresión y formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Estos son los tipos que se admiten: **GZip**, **Deflate**, **BZip2** y **ZipDeflate**.<br/>Estos son los niveles que se admiten: **Optimal** y **Fastest**. |Sin  |

>[!TIP]
>Para copiar todos los archivos en una carpeta, especifique solo **folderPath**.<br>Para copiar un único archivo con un nombre determinado, especifique **folderPath** con el elemento de carpeta y **fileName** con el nombre de archivo.<br>Para copiar un subconjunto de archivos en una carpeta, especifique **folderPath** con el elemento de carpeta y **fileName** con el filtro de comodín.

**Ejemplo:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen HDFS.

### <a name="hdfs-as-source"></a>HDFS como origen

- Para copiar desde **Parquet y formato de texto delimitado**, consulte [Parquet y origen del formato de texto delimitado](#parquet-and-delimited-text-format-source) sección.
- Para copiar desde otros formatos como **formato ORC/Avro/JSON/binario**, consulte [otro origen de formato](#other-format-source) sección.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet y origen del formato de texto delimitado

Para copiar datos desde HDFS en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo sobre el origen de la actividad de copia basada en el formato y configuraciones admitidas. Las siguientes propiedades son compatibles con HDFS en `storeSettings` configuración en el origen de copia basada en el formato:

| Propiedad                 | DESCRIPCIÓN                                                  | Obligatorio                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | La propiedad type en `storeSettings` debe establecerse en **HdfsReadSetting**. | Sí                                           |
| recursive                | Indica si los datos se leen de forma recursiva de las subcarpetas o solo de la carpeta especificada. Tenga en cuenta que cuando recursive se establece en true y el receptor es un almacén basado en archivos, no se crea una carpeta o una subcarpeta vacía en el receptor. Los valores permitidos son: **True** (valor predeterminado) y **False**. | Sin                                             |
| wildcardFolderPath       | La ruta de acceso de carpeta con caracteres comodín para filtrar las carpetas de origen. <br>Los caracteres comodín permitidos son: `*` (coincide con cero o más caracteres) y `?` (coincide con cero o carácter individual); use `^` para el escape si el nombre real de la carpeta tiene un carácter comodín o este carácter de escape dentro. <br>Ver más ejemplos en [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). | Sin                                             |
| wildcardFileName         | El nombre de archivo con caracteres comodín en el folderPath/wildcardFolderPath determinado para filtrar los archivos de origen. <br>Los caracteres comodín permitidos son: `*` (coincide con cero o más caracteres) y `?` (coincide con cero o carácter individual); use `^` para el escape si el nombre real de la carpeta tiene un carácter comodín o este carácter de escape dentro.  Ver más ejemplos en [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). | Sí si `fileName` no se especifica en el conjunto de datos |
| modifiedDatetimeStart    | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br> Las propiedades pueden ser NULL, lo que significa que no se aplicará ningún filtro de atributo de archivo al conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora. | Sin                                             |
| modifiedDatetimeEnd      | Igual que el anterior.                                               | Sin                                             |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de almacenamiento al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | No                                            |

> [!NOTE]
> Para Parquet/texto delimitado con **FileSystemSource** todavía se admite como origen de la actividad de copia tipo mencionado en la siguiente sección: sirve para la compatibilidad con versiones anteriores. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromHDFS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "HdfsReadSetting",
                    "recursive": true
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Otro origen de formato

Para copiar datos desde HDFS en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades en la actividad de copia **origen** sección:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **HdfsSource** |Sí |
| recursive | Indica si los datos se leen de forma recursiva de las subcarpetas o solo de la carpeta especificada. Tenga en cuenta que cuando recursive se establezca en true y el receptor sea un almacén basado en archivos, la carpeta o subcarpeta vacías no se copiarán ni crearán en el receptor.<br/>Los valores permitidos son: **True** (valor predeterminado) y **False** | No |
| distcpSettings | Grupo de propiedades al utilizar la herramienta DistCp de HDFS. | No |
| resourceManagerEndpoint | El punto de conexión de administrador de recursos de Yarn | Sí, se utiliza DistCp |
| tempScriptPath | Una ruta de acceso de carpeta que se usa para almacenar el script del comando DistCp. Data Factory se encarga de crear el archivo de script que se eliminará después de que haya finalizado el trabajo de copia. | Sí, se utiliza DistCp |
| distcpOptions | Opciones adicionales que se proporcionan al comando DistCp. | No |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de almacenamiento al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | No |

**Ejemplo: Origen HDFS en la actividad de copia mediante DistCp**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

Obtenga más información en la sección siguiente sobre cómo usar DistCp para copiar datos desde HDFS de forma eficaz.

### <a name="folder-and-file-filter-examples"></a>Ejemplos de filtros de carpetas y archivos

Esta sección describe el comportamiento resultante de la ruta de acceso de la carpeta y el nombre de archivo con los filtros de carácter comodín.

| folderPath | fileName             | recursive | Resultado de estructura de carpeta de origen y filtro (se recuperan los archivos en **negrita**) |
| :--------- | :------------------- | :-------- | :----------------------------------------------------------- |
| `Folder*`  | (vacío, usar el valor predeterminado) | false     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | (vacío, usar el valor predeterminado) | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | `*.csv`              | false     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | `*.csv`              | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="use-distcp-to-copy-data-from-hdfs"></a>Uso de DistCp para copiar datos desde HDFS

[DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) es una herramienta de línea de comandos nativa de Hadoop para realizar copias distribuidas en un clúster de Hadoop. Al ejecutar un comando Distcp, primero se enumerarán todos los archivos que se copiarán, se crearán varios trabajos de asignación en el clúster de Hadoop y cada uno de estos hará una copia binaria del origen al receptor.

La actividad de copia permite usar DistCp para copiar archivos tal cual en Azure Blob (incluida la [copia almacenada provisionalmente](copy-activity-performance.md) o Azure Data Lake Store, en cuyo caso puede aprovechar la potencia del clúster, en lugar de ejecutarse en el entorno Integration Runtime (autohospedado). De este modo, el rendimiento de la copia será mayor, sobre todo si el clúster es muy eficaz. Según la configuración de Azure Data Factory, la actividad de copia crea automáticamente un comando Distcp, lo envía al clúster de Hadoop y supervisa el estado de la copia.

### <a name="prerequisites"></a>Requisitos previos

Para usar DistCp con el fin de copiar los archivos tal cual desde HDFS a Azure Blob (incluida la copia almacenada provisionalmente) o Azure Data Lake Store, asegúrese de que el clúster de Hadoop cumpla los siguientes requisitos:

1. Los servicios de MapReduce y Yarn están habilitados.
2. La versión de Yarn es la 2.5 o una superior.
3. El servidor HDFS está integrado con el almacén de datos de destino: Azure Blob o Azure Data Lake Store:

    - El sistema de archivos de Azure Blob se admite de forma nativa en Hadoop 2.7 o superior. Solo debe especificar la ruta de acceso del archivo .jar en env-config de Hadoop.
    - El sistema de archivos de Azure Data Lake Store se empaqueta a partir de Hadoop 3.0.0-alpha1. Si la versión del clúster de Hadoop es inferior, debe importar manualmente los paquetes .jar relacionados de ADLS (azure-datalake-store.jar) en el clúster desde [aquí](https://hadoop.apache.org/releases.html), y especificar la ruta de acceso del archivo .jar en env config de Hadoop.

4. Prepare una carpeta temporal en HDFS. Esta carpeta temporal se usa para almacenar el script de shell de DistCp, por lo que ocupa varios kilobytes de espacio.
5. Asegúrese de que la cuenta de usuario proporcionada en el servicio vinculado de HDFS tiene permiso para a) enviar la aplicación de Yarn; b) crear la subcarpeta, y leer y escribir archivos en la carpeta temporal.

### <a name="configurations"></a>Configuraciones

Consulte DistCp relacionados con las configuraciones y los ejemplos de [HDFS como origen](#hdfs-as-source) sección.

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Uso de autenticación Kerberos para el conector HDFS

Existen dos opciones para configurar el entorno local para usar la autenticación Kerberos en el conector HDFS. Puede elegir la que mejor se adapte en su caso.
* Opción 1: [Unir la máquina Integration Runtime (autohospedado) en el dominio Kerberos](#kerberos-join-realm)
* Opción 2: [Habilitar la confianza mutua entre el dominio de Windows y el dominio Kerberos](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Opción 1: Unir la máquina Integration Runtime (autohospedado) en el dominio Kerberos

#### <a name="requirements"></a>Requisitos

* La máquina Integration Runtime (autohospedado) debe unirse al dominio Kerberos y no a ninguno de Windows.

#### <a name="how-to-configure"></a>Configuración

**Máquina Integration Runtime (autohospedado):**

1.  Ejecute la utilidad **Ksetup** para configurar el dominio y el servidor KDC de Kerberos.

    La máquina debe configurarse como miembro de un grupo de trabajo dado que un dominio Kerberos es diferente de un dominio de Windows. Para ello, establezca el dominio Kerberos y agregue un servidor KDC de la manera siguiente: Sustituya *REALM.COM* por su dominio respectivo, según sea necesario.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Reinicie** la máquina después de ejecutar estos 2 comandos.

2.  Compruebe la configuración con el comando **Ksetup**. La salida debe ser como la siguiente:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**En Azure Data Factory:**

* Configure el conector HDFS mediante la **autenticación de Windows** junto con el nombre y la contraseña de la entidad de seguridad de Kerberos para conectarse al origen de datos de HDFS. Compruebe la sección [HDFS Linked Service properties](#linked-service-properties) (Propiedades de servicio vinculado de HDFS) en los detalles de configuración.

### <a name="kerberos-mutual-trust"></a>Opción 2: Habilitar la confianza mutua entre el dominio de Windows y el dominio Kerberos

#### <a name="requirements"></a>Requisitos

*   La máquina Integration Runtime (autohospedado) debe unirse a un dominio Windows.
*   Necesita permiso para actualizar la configuración del controlador de dominio.

#### <a name="how-to-configure"></a>Configuración

> [!NOTE]
> Sustituya REALM.COM y AD.COM en el siguiente tutorial por su propio dominio y controlador de dominio, según sea necesario.

**En el servidor KDC:**

1. Edite la configuración de KDC en el archivo **krb5.conf** para permitir que KDC confíe en el dominio de Windows que hace referencia a la siguiente plantilla de configuración. De forma predeterminada, la configuración está ubicada en **/etc/krb5.conf**.

           [logging]
            default = FILE:/var/log/krb5libs.log
            kdc = FILE:/var/log/krb5kdc.log
            admin_server = FILE:/var/log/kadmind.log
            
           [libdefaults]
            default_realm = REALM.COM
            dns_lookup_realm = false
            dns_lookup_kdc = false
            ticket_lifetime = 24h
            renew_lifetime = 7d
            forwardable = true
            
           [realms]
            REALM.COM = {
             kdc = node.REALM.COM
             admin_server = node.REALM.COM
            }
           AD.COM = {
            kdc = windc.ad.com
            admin_server = windc.ad.com
           }
            
           [domain_realm]
            .REALM.COM = REALM.COM
            REALM.COM = REALM.COM
            .ad.com = AD.COM
            ad.com = AD.COM
            
           [capaths]
            AD.COM = {
             REALM.COM = .
            }

   **Reinicie** el servicio KDC después de la configuración.

2. Prepare una entidad de seguridad denominado **krbtgt/REALM.COM\@AD.COM** en el servidor KDC con el siguiente comando:

           Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3. En el archivo de configuración de servicio de HDFS **hadoop.security.auth_to_local**, agregue `RULE:[1:$1@$0](.*\@AD.COM)s/\@.*//`.

**En el controlador de dominio:**

1.  Ejecute los siguientes comandos **Ksetup** para agregar una entrada de dominio:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Establezca la confianza entre el dominio de Windows y el dominio Kerberos. [contraseña] es la contraseña de la entidad de seguridad **krbtgt/REALM.COM\@AD.COM**.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Seleccione el algoritmo de cifrado usado en Kerberos.

    1. Vaya a Administrador de servidores > Administración de directivas de grupo > Dominio > Objetos de directiva de grupo > Default or Active Domain Policy (Directiva de dominio predeterminada o activa) y haga clic en Editar.

    2. En la ventana emergente **Editor de administración de directivas de grupo**, vaya a Configuración del equipo > Directivas > Configuración de Windows > Configuración de seguridad > Directivas locales > Opciones de seguridad, y configure **Seguridad de red: Configurar tipos de cifrado permitidos para Kerberos**.

    3. Seleccione el algoritmo de cifrado que quiere usar al conectarse a KDC. Normalmente, puede seleccionar todas las opciones.

        ![Configuración de tipos de cifrado para Kerberos](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Use el comando **Ksetup** para especificar el algoritmo de cifrado que se usará en el dominio específico.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Cree la asignación entre la cuenta de dominio y la entidad de seguridad de Kerberos, a fin de usar la entidad de seguridad de Kerberos en el dominio de Windows.

    1. Inicie las herramientas administrativas > **Usuarios y equipos de Active Directory**.

    2. Configure características avanzadas; para ello, haga clic en **Ver** > **Características avanzadas**.

    3. Busque la cuenta a la que quiere crear asignaciones y haga clic con el botón derecho para ver **Asignaciones de nombres** > haga clic en la pestaña **Nombres de Kerberos**.

    4. Agregue una entidad de seguridad desde el dominio.

        ![Asignación de la identidad de seguridad](media/connector-hdfs/map-security-identity.png)

**Máquina Integration Runtime (autohospedado):**

* Ejecute los siguientes comandos **Ksetup** para agregar una entrada de dominio.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**En Azure Data Factory:**

* Configure el conector de HDFS mediante la **autenticación de Windows** en combinación con la cuenta de dominio o la entidad de seguridad de Kerberos para conectarse al origen de datos de HDFS. Compruebe la sección [HDFS Linked Service properties](#linked-service-properties) (Propiedades de servicio vinculado de HDFS) en los detalles de configuración.


## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
