---
title: Copia de datos de Google Cloud Storage con Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde Google Cloud Storage a almacenes de datos de receptor compatibles mediante Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 9c9a4b41dbb9b9acc2982ae2af1f3a611f9d3beb
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228277"
---
# <a name="copy-data-from-google-cloud-storage-using-azure-data-factory"></a>Copia de datos de Google Cloud Storage con Azure Data Factory

En este artículo se describe cómo copiar datos de almacenamiento en la nube de Google. Para información sobre Azure Data Factory, lea el [artículo de introducción](introduction.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Google Cloud Storage es compatible con las siguientes actividades:

- [Actividad de copia](copy-activity-overview.md) con [admite la matriz de origen/receptor](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)

Concretamente, este conector de Google Cloud Storage admite la copia de archivos tal cual, o el análisis de estos con los [códecs de compresión y los formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md).

>[!NOTE]
>La funcionalidad de copia de datos de Google Cloud Storage usa el [conector de Amazon S3](connector-amazon-simple-storage-service.md) con el correspondiente punto de conexión de S3 personalizado, ya que Google Cloud Storage proporciona una interoperabilidad compatible con S3.

## <a name="required-permissions"></a>Permisos necesarios

Para copiar datos de Google Cloud Storage, asegúrese de que se han concedido los siguientes permisos:

- **Para la ejecución de la actividad de copia:**: `s3:GetObject` y `s3:GetObjectVersion` para Object Operations.
- **Para la creación de la interfaz gráfica de usuario de Data Factory**: `s3:ListAllMyBuckets` y `s3:ListBucket`/`s3:GetBucketLocation` se requieren adicionalmente para los permisos de Bucket Operations para operaciones como probar una conexión y explorar o navegar por rutas de acceso de archivos. Si no quiere conceder estos permisos, omita la conexión de prueba en la página de creación de servicios vinculados y especifique la ruta de acceso directamente en la configuración del conjunto de datos.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas para el conector de Google Cloud Storage.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Google Cloud Storage:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **AmazonS3**. | Sí |
| accessKeyId | Id. de la clave de acceso secreta. Para encontrar la clave de acceso y el secreto, vaya a **Google Cloud Storage** > **Configuración** > **Interoperabilidad**. |Sí |
| secretAccessKey | La propia clave de acceso secreta. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |Sí |
| serviceUrl | Especifique el punto de conexión personalizado de S3 como **`https://storage.googleapis.com`**. | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) (si el almacén de datos se encuentra en una red privada) o Azure Integration Runtime. Si no se especifica, se usará Azure Integration Runtime. |No |

Este es un ejemplo:

```json
{
    "name": "GoogleCloudStorageLinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                "type": "SecureString",
                "value": "<secret access key>"
            },
            "serviceUrl": "https://storage.googleapis.com"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

- Para **Parquet y formato de texto delimitado**, consulte [conjunto de datos con formato Parquet y texto delimitado](#parquet-and-delimited-text-format-dataset) sección.
- Para otros formatos como **formato ORC/Avro/JSON/binario**, consulte [otro conjunto de datos de formato](#other-format-dataset) sección.

### <a name="parquet-and-delimited-text-format-dataset"></a>Conjunto de datos con formato parquet y texto delimitado

Para copiar datos de almacenamiento en la nube de Google en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo en el conjunto de datos de formato y configuraciones admitidas. Se admiten las siguientes propiedades para el almacenamiento en la nube de Google en `location` configuración en el conjunto de datos basado en el formato:

| Propiedad   | DESCRIPCIÓN                                                  | Obligatorio |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | La propiedad type en `location` en el conjunto de datos debe establecerse en **AmazonS3Location**. | Sí      |
| bucketName | Nombre del depósito de S3.                                          | Sí      |
| folderPath | La ruta de acceso a la carpeta en el depósito especificado. Si desea usar el carácter comodín a la carpeta de filtro, omita esta configuración y especificar en la configuración del origen de actividad. | Sin        |
| fileName   | El nombre de archivo en el depósito determinado + folderPath. Si desea usar el carácter comodín para filtrar los archivos, omita esta configuración y especificar en la configuración del origen de actividad. | No       |

> [!NOTE]
> **AmazonS3Object** todavía se admite el tipo de conjunto de datos con formato Parquet/texto que se mencionan en la siguiente sección como-actividad de copia/búsqueda/GetMetadata para compatibilidad con versiones anteriores de. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Google Cloud Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AmazonS3Location",
                "bucketName": "bucketname",
                "folderPath": "folder/subfolder"
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

Para copiar datos de almacenamiento en la nube de Google en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **AmazonS3Object** |Sí |
| bucketName | Nombre del depósito de S3. No se admiten filtros con caracteres comodín. |Sí, para la actividad de copia y búsqueda; No, para la actividad GetMetadata |
| key | El **filtro de nombre o de comodín** de la clave del objeto S3 del cubo especificado. Solo se aplica cuando no se especifica la propiedad "prefix". <br/><br/>El filtro de comodín se admite tanto para el elemento de carpeta como para el elemento de nombre de archivo. Los caracteres comodín permitidos son: `*` (equivale a cero o a varios caracteres) y `?` (equivale a cero o a un único carácter).<br/>- Ejemplo 1: `"key": "rootfolder/subfolder/*.csv"`<br/>- Ejemplo 2: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>Ver más ejemplos en [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). Use `^` como escape si el nombre real del archivo o la carpeta contiene un carácter comodín o este carácter de escape. |Sin  |
| prefix | Prefijo de la clave del objeto S3. Se seleccionan objetos cuyas claves comienzan por este prefijo. Solo se aplica cuando no se especifica la propiedad "key". |Sin  |
| version | La versión del objeto S3 si está habilitado el control de versiones de S3. |No |
| modifiedDatetimeStart | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br/><br/> Las propiedades pueden ser NULL, lo que significa que no se aplicará ningún filtro de atributo de archivo al conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora.| Sin  |
| modifiedDatetimeEnd | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br/><br/> Las propiedades pueden ser NULL, lo que significa que no se aplicará ningún filtro de atributo de archivo al conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora.| Sin  |
| format | Si desea **copiar los archivos tal cual** entre los almacenes basados en archivos (copia binaria), omita la sección de formato en las definiciones de los conjuntos de datos de entrada y salida.<br/><br/>Si desea analizar o generar archivos con un formato concreto, se admiten los siguientes tipos de formato de archivo: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Establezca la propiedad **type** de formato en uno de los siguientes valores. Para más información, consulte las secciones [Formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [Formato Json](supported-file-formats-and-compression-codecs.md#json-format), [Formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) y [Formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |No (solo para el escenario de copia binaria) |
| compression | Especifique el tipo y el nivel de compresión de los datos. Para más información, consulte el artículo sobre [códecs de compresión y formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Estos son los tipos que se admiten: **GZip**, **Deflate**, **BZip2** y **ZipDeflate**.<br/>Estos son los niveles que se admiten: **Optimal** y **Fastest**. |Sin  |

>[!TIP]
>Para copiar todos los archivos en una carpeta, especifique **bucketName** para el cubo y **prefix** para el elemento de carpeta.<br>Para copiar un único archivo con un nombre determinado, especifique **bucketName** para el cubo y **key** para el elemento de carpeta más el nombre del archivo.<br>Para copiar un subconjunto de archivos en una carpeta, especifique **bucketName** para el cubo y **key** para el elemento de carpeta más el filtro de comodín.

**Ejemplo: uso de prefijo**

```json
{
    "name": "GoogleCloudStorageDataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el origen de Google Cloud Storage.

### <a name="google-cloud-storage-as-source"></a>Google Cloud Storage como origen

- Para copiar desde **Parquet y formato de texto delimitado**, consulte [Parquet y origen del formato de texto delimitado](#parquet-and-delimited-text-format-source) sección.
- Para copiar desde otros formatos como **formato ORC/Avro/JSON/binario**, consulte [otro origen de formato](#other-format-source) sección.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet y origen del formato de texto delimitado

Para copiar datos de almacenamiento en la nube de Google en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo sobre la actividad de copia basada en el formato configuraciones admitidas y el origen. Se admiten las siguientes propiedades para el almacenamiento en la nube de Google en `storeSettings` configuración en el origen de copia basada en el formato:

| Propiedad                 | DESCRIPCIÓN                                                  | Obligatorio                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| type                     | La propiedad type en `storeSettings` debe establecerse en **AmazonS3ReadSetting**. | Sí                                                         |
| recursive                | Indica si los datos se leen de forma recursiva de las subcarpetas o solo de la carpeta especificada. Tenga en cuenta que cuando recursive se establece en true y el receptor es un almacén basado en archivos, no se crea una carpeta o una subcarpeta vacía en el receptor. Los valores permitidos son: **True** (valor predeterminado) y **False**. | Sin                                                           |
| prefix                   | Prefijo para la clave del objeto S3 en el depósito especificado configurado en el conjunto de datos para filtrar objetos de origen. Se seleccionan objetos cuyas claves comienzan por este prefijo. Solo se aplica cuando `wildcardFolderPath` y `wildcardFileName` no se especifican propiedades. |                                                             |
| wildcardFolderPath       | La ruta de acceso de carpeta con caracteres comodín en el depósito especificado configurado en el conjunto de datos a las carpetas de origen del filtro. <br>Los caracteres comodín permitidos son: `*` (coincide con cero o más caracteres) y `?` (coincide con cero o carácter individual); use `^` para el escape si el nombre real de la carpeta tiene un carácter comodín o este carácter de escape dentro. <br>Ver más ejemplos en [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). | Sin                                                           |
| wildcardFileName         | El nombre de archivo con caracteres comodín en el depósito determinado + folderPath/wildcardFolderPath para filtrar los archivos de origen. <br>Los caracteres comodín permitidos son: `*` (coincide con cero o más caracteres) y `?` (coincide con cero o carácter individual); use `^` para el escape si el nombre real de la carpeta tiene un carácter comodín o este carácter de escape dentro.  Ver más ejemplos en [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). | Sí si `fileName` en el conjunto de datos y `prefix` no se han especificado |
| modifiedDatetimeStart    | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br> Las propiedades pueden ser NULL, lo que significa que no se aplicará ningún filtro de atributo de archivo al conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora. | Sin                                                           |
| modifiedDatetimeEnd      | Igual que el anterior.                                               | Sin                                                           |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de almacenamiento al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | No                                                          |

> [!NOTE]
> Para Parquet/texto delimitado con **FileSystemSource** todavía se admite como origen de la actividad de copia tipo mencionado en la siguiente sección: sirve para la compatibilidad con versiones anteriores. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromGoogleCloudStorage",
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
                    "type": "AmazonS3ReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
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

Para copiar datos de almacenamiento en la nube de Google en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades en la actividad de copia **origen** sección:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **FileSystemSource** |Sí |
| recursive | Indica si los datos se leen de forma recursiva de las subcarpetas o solo de la carpeta especificada. Tenga en cuenta que cuando recursive se establezca en true y el receptor sea un almacén basado en archivos, la carpeta o subcarpeta vacías no se copiarán ni crearán en el receptor.<br/>Los valores permitidos son: **True** (valor predeterminado) y **False** | Sin  |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de almacenamiento al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromGoogleCloudStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
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
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Ejemplos de filtros de carpetas y archivos

Esta sección describe el comportamiento resultante de la ruta de acceso de la carpeta y el nombre de archivo con los filtros de carácter comodín.

| bucket | key | recursive | Resultado de estructura de carpeta de origen y filtro (se recuperan los archivos en negrita)|
|:--- |:--- |:--- |:--- |
| bucket | `Folder*/*` | false | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| bucket | `Folder*/*` | true | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| bucket | `Folder*/*.csv` | false | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| bucket | `Folder*/*.csv` | true | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
