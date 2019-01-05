---
title: Copia de datos a Azure Data Lake Storage Gen1 o desde él mediante Azure Data Factory | Microsoft Docs
description: Aprenda a copiar datos desde almacenes de datos de origen compatibles a Azure Data Lake Store o desde Data Lake Store a almacenes de receptores compatibles mediante Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: jingwang
ms.openlocfilehash: 411223c2ef7b1400f2019ebc6704b3c4a5d41235
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53387036"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-by-using-azure-data-factory"></a>Copia de datos con Azure Data Lake Storage Gen1 como origen o destino mediante Azure Data Factory
> [!div class="op_single_selector" title1="Seleccione la versión del servicio de Data Factory que está utilizando:"]
> * [Versión 1](v1/data-factory-azure-datalake-connector.md)
> * [Versión actual](connector-azure-data-lake-store.md)

En este artículo se resume el uso de la actividad de copia de Azure Data Factory para copiar datos con Azure Data Lake Storage Gen1 (anteriormente conocido como Azure Data Lake Store) como origen o destino. Se basa en la [introducción a la actividad de copia](copy-activity-overview.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde cualquier almacén de datos de origen compatible a Azure Data Lake Store o desde Azure Data Lake Store a cualquier almacén de datos del receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats).

En concreto, este conector de Azure Data Lake Store admite las siguientes funcionalidades:

- La copia de archivos mediante uno de los siguientes métodos de autenticación: **entidad de servicio** o **identidades administradas para recursos de Azure**.
- La copia de archivos tal cual, o bien el análisis o generación de archivos con los [códecs de compresión y los formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md).

> [!IMPORTANT]
> Si copia datos con el entorno de ejecución de integración autohospedado, configure el firewall corporativo para que permita el tráfico saliente a `<ADLS account name>.azuredatalakestore.net` y a `login.microsoftonline.com/<tenant>/oauth2/token` en el puerto 443. El último es Azure Security Token Service, con el que el entorno de ejecución de integración necesita comunicarse para obtener el token de acceso.

## <a name="get-started"></a>Introducción

> [!TIP]
> Para ver un tutorial del uso de conector de Azure Data Lake Store, consulte [Carga de datos en Azure Data Lake Storage Gen1 mediante Azure Data Factory](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles acerca de las propiedades que se usan para definir entidades de Data Factory específicas para Azure Data Lake Store.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Data Lake Store:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad `type` debe establecerse en **AzureDataLakeStore**. | SÍ |
| dataLakeStoreUri | Información sobre la cuenta de Azure Data Lake Store. Esta información adopta uno de los siguientes formatos: `https://[accountname].azuredatalakestore.net/webhdfs/v1` o `adl://[accountname].azuredatalakestore.net/`. | SÍ |
| subscriptionId | El identificador de la suscripción de Azure al que pertenece la cuenta de Data Lake Store. | Necesario para el receptor |
| resourceGroupName | El nombre del grupo de recursos de Azure al que pertenece la cuenta de Data Lake Store. | Necesario para el receptor |
| connectVia | El [entorno de ejecución de integración](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar el entorno de ejecución de integración de Azure o el entorno de ejecución de integración autohospedado (si el almacén de datos se encuentra en una red privada). Si no se especifica esta propiedad, usa el entorno de ejecución de integración de Azure predeterminado. |Sin  |

### <a name="use-service-principal-authentication"></a>Uso de la autenticación de entidad de servicio

Para usar la autenticación de entidad de servicio, registre una entidad de aplicación en Azure Active Directory y concédale acceso a Data Lake Store. Consulte [Autenticación entre servicios](../data-lake-store/data-lake-store-authenticate-using-active-directory.md) para ver los pasos detallados. Anote los siguientes valores; los usará para definir el servicio vinculado:

- Identificador de aplicación
- Clave de la aplicación
- Id. de inquilino

>[!IMPORTANT]
> Asegúrese de que concede el permiso adecuado a la entidad de servicio en Data Lake Store:
>- **Como origen**: En **Explorador de datos** > **Acceso**, conceda al menos permiso de **lectura y ejecución** permiso para enumerar y copiar los archivos de las carpetas y subcarpetas. O bien, puede conceder permiso de **lectura** para copiar un único archivo. Puede elegir agregarlo a **Esta carpeta y todos los elementos secundarios** si desea que sea recursivo y agregarlo como **un permiso de acceso y una entrada de permiso predeterminada**. No hay ningún requisito en el control de acceso de nivel de cuenta (IAM).
>- **Como receptor**: En **Explorador de datos** > **Acceso**, conceda al menos permiso de **escritura y ejecución** para crear elementos secundarios en la carpeta. Puede elegir agregarlo a **Esta carpeta y todos los elementos secundarios** si desea que sea recursivo y agregarlo como **un permiso de acceso y una entrada de permiso predeterminada**. Si usa el entorno de ejecución de integración de Azure para realizar la copia (tanto el origen como el receptor están en la nube), en IAM, conceda al menos el rol de **lector** para que Data Factory pueda detectar la región de Data Lake Store. Si desea no usar este rol de IAM, [cree explícitamente una instancia de Azure Integration Runtime](create-azure-integration-runtime.md#create-azure-ir) con la ubicación de Data Lake Store. Asócielo en el servicio vinculado de Data Lake Storage como se muestra en el ejemplo siguiente.

>[!NOTE]
>En la lista de carpetas que comienza en la raíz, debe establecer el permiso de la entidad de servicio que se concede **a nivel raíz con el permiso de "ejecución"**. Esto es válido cuando se usa la:
>- **Herramienta Copy Data** para crear la canalización de la copia.
>- **Interfaz de usuario de Data Factory** para probar la conexión y el desplazamiento por las carpetas durante la creación.

Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | SÍ |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como `SecureString`para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | SÍ |
| tenant | Especifique la información del inquilino (nombre de dominio o identificador de inquilino) en el que reside la aplicación. Para recuperarlo, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. | SÍ |

**Ejemplo:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Uso de identidades administradas para la autenticación de recursos de Azure

Una factoría de datos se puede asociar con una [identidad administrada para recursos de Azure](data-factory-service-identity.md), que representa esa factoría de datos concreta. Puede usar directamente esta identidad de servicio para la autenticación en Data Lake Store, de forma similar a como usa su propia entidad de servicio. Permite que esta fábrica designada acceda a datos los copie a o desde Data Lake Store.

Para usar identidades administradas para la autenticación de recursos de Azure:

1. [Recupere la identidad del servicio Data Factory](data-factory-service-identity.md#retrieve-service-identity) mediante la copia del valor de "Id. de la aplicación de identidad de servicio" generado junto con su fábrica.
2. Conceda a la identidad de servicio acceso a Data Lake Store de la misma forma que lo hace para la entidad de servicio, con las notas siguientes.

>[!IMPORTANT]
> Asegúrese de conceder el permiso adecuado a la identidad de servicio de la factoría de datos en Data Lake Store:
>- **Como origen**: En **Explorador de datos** > **Acceso**, conceda al menos permiso de **lectura y ejecución** permiso para enumerar y copiar los archivos de las carpetas y subcarpetas. O bien, puede conceder permiso de **lectura** para copiar un único archivo. Puede elegir agregarlo a **Esta carpeta y todos los elementos secundarios** si desea que sea recursivo y agregarlo como **un permiso de acceso y una entrada de permiso predeterminada**. No hay ningún requisito en el control de acceso de nivel de cuenta (IAM).
>- **Como receptor**: En **Explorador de datos** > **Acceso**, conceda al menos permiso de **escritura y ejecución** para crear elementos secundarios en la carpeta. Puede elegir agregarlo a **Esta carpeta y todos los elementos secundarios** si desea que sea recursivo y agregarlo como **un permiso de acceso y una entrada de permiso predeterminada**. Si usa Azure Integration Runtime para realizar la copia (tanto el origen como el receptor están en la nube), en IAM, conceda al menos el rol de **lector** para que Data Factory pueda detectar la región de Data Lake Store. Si desea no usar este rol de IAM, [cree explícitamente una instancia de Azure Integration Runtime](create-azure-integration-runtime.md#create-azure-ir) con la ubicación de Data Lake Store. Asócielo en el servicio vinculado de Data Lake Storage como se muestra en el ejemplo siguiente.

>[!NOTE]
>En la lista de carpetas que comienza en la raíz, debe establecer el permiso de la entidad de servicio que se concede **a nivel raíz con el permiso de "ejecución"**. Esto es válido cuando se usa la:
>- **Herramienta Copy Data** para crear la canalización de la copia.
>- **Interfaz de usuario de Data Factory** para probar la conexión y el desplazamiento por las carpetas durante la creación.

En Azure Data Factory no es necesario especificar ninguna propiedad, más allá de la información general de Data Lake Store, en el servicio vinculado.

**Ejemplo:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Para copiar datos con Azure Data Lake Store como origen o destino, establezca la propiedad `type` del conjunto de datos en **AzureDataLakeStoreFile**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del conjunto de datos debe establecerse en: **AzureDataLakeStoreFile** |SÍ |
| folderPath | Ruta de acceso a la carpeta de Data Lake Store. No se admiten filtros con caracteres comodín. Si no se especifica, apunta a la raíz. Ejemplo: rootfolder/subfolder/ |Sin  |
| fileName | **Filtro de nombre o de comodín** para los archivos de la ruta "folderPath" especificada. Si no especifica ningún valor para esta propiedad, el conjunto de datos apunta a todos los archivos de la carpeta. <br/><br/>Para filtrar, los caracteres comodín permitidos son: `*` (equivale a cero o a varios caracteres) y `?` (equivale a cero o a un único carácter).<br/>- Ejemplo 1: `"fileName": "*.csv"`<br/>- Ejemplo 2: `"fileName": "???20180427.txt"`<br/>Use `^` como escape si el nombre de archivo real contiene un comodín o este carácter de escape.<br/><br/>Si fileName no se especifica para un conjunto de datos de salida y **preserveHierarchy** no se determina en el receptor de la actividad, la actividad de copia generará automáticamente el nombre de archivo con el siguiente patrón: "*Data.[GUID de id. de ejecución de actividad].[GUID si FlattenHierarchy].[formato, si está configurado].[compresión, si está configurada]*". Un ejemplo es "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz". Si realiza una copia desde el origen tabular utilizando el nombre de la tabla en lugar de la consulta, el patrón del nombre es "*[nombre de tabla].[formato].[compresión, si está configurada]*". Por ejemplo, "MyTable.csv". |Sin  |
| formato | Si desea **copiar los archivos tal cual** entre los almacenes basados en archivos (copia binaria), omita la sección de formato en las definiciones de los conjuntos de datos de entrada y salida.<br/><br/>Si desea analizar o generar archivos con un formato concreto, se admiten los siguientes tipos de formato de archivo: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Establezca la propiedad **type** de formato en uno de los siguientes valores. Para más información, consulte las secciones [Formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [Formato Json](supported-file-formats-and-compression-codecs.md#json-format), [Formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) y [Formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |No (solo para el escenario de copia binaria) |
| compresión | Especifique el tipo y el nivel de compresión de los datos. Para más información, consulte el artículo sobre [códecs de compresión y formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Estos son los tipos que se admiten: **GZip**, **Deflate**, **BZip2** y **ZipDeflate**.<br/>Estos son los niveles que se admiten: **Optimal** y **Fastest**. |Sin  |


>[!TIP]
>Para copiar todos los archivos en una carpeta, especifique solo **folderPath**.<br>Para copiar un único archivo con un nombre concreto, especifique **folderPath** con la parte de la carpeta y **fileName** con el nombre de archivo.<br>Para copiar un subconjunto de archivos en una carpeta, especifique **folderPath** con el elemento de carpeta y **fileName** con el filtro de comodín. 

**Ejemplo:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
            "fileName": "myfile.csv.gz",
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

Para ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admiten el receptor y el origen de Azure Data Lake Store.

### <a name="azure-data-lake-store-as-source"></a>Azure Data Lake Store como origen

Para copiar datos desde Data Lake Store, establezca el tipo de origen de la actividad de copia en **AzureDataLakeStoreSource**. La sección **source** de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad `type` del origen de la actividad de copia debe establecerse en: **AzureDataLakeStoreSource**. |SÍ |
| recursive | Indica si los datos se leen de forma recursiva de las subcarpetas o solo de la carpeta especificada. Tenga en cuenta que cuando `recursive` se establece en true y el receptor es un almacén basado en archivos, no se copia ni crea ninguna carpeta o subcarpeta vacías en el receptor. Los valores permitidos son: **true** (valor predeterminado) y **false**. | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS input dataset name>",
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
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Azure Data Lake Store como receptor

Para copiar datos a Data Lake Store, establezca el tipo de receptor de la actividad de copia en **AzureDataLakeStoreSink**. Se admiten las siguientes propiedades en la sección **sink**:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad `type` del receptor de la actividad de copia debe establecerse en: **AzureDataLakeStoreSink** |SÍ |
| copyBehavior | Define el comportamiento de copia cuando el origen son archivos de un almacén de datos basados en archivos.<br/><br/>Los valores permitidos son:<br/><b>- PreserveHierarchy (valor predeterminado)</b>: conserva la jerarquía de archivos en la carpeta de destino. La ruta de acceso relativa del archivo de origen a la carpeta de origen es idéntica que la ruta de acceso relativa del archivo de destino a la carpeta de destino.<br/><b>- FlattenHierarchy</b>: todos los archivos de la carpeta de origen están en el primer nivel de la carpeta de destino. Los archivos de destino tienen nombres generados automáticamente. <br/><b>- MergeFiles</b>: combina todos los archivos de la carpeta de origen en un archivo. Si se especifica el nombre del blob o del archivo, el nombre de archivo combinado es el nombre especificado. En caso contrario, el nombre de archivo se genera automáticamente. | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyToADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="examples-of-behavior-of-the-copy-operation"></a>Ejemplos de comportamiento de la operación de copia

En esta sección se describe el comportamiento resultante de la operación de copia para diferentes combinaciones de los valores `recursive` y `copyBehavior`.

| recursive | copyBehavior | Estructura de carpetas de origen | Destino resultante |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | El destino Folder1 se crea con la misma estructura que el origen:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5. |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File5 |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;El contenido de File1 + File2 + File3 + File4 + File 5 se combina en un solo archivo, cuyo nombre se genera automáticamente. |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File2<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;El contenido de File1 + File2 se combina en un archivo con un nombre de archivo generado automáticamente. Nombre de archivo generado automáticamente para File1<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |

## <a name="next-steps"></a>Pasos siguientes
Para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores, consulte los [almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats).
