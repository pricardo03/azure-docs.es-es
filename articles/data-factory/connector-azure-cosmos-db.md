---
title: Copia de datos desde o hacia Azure Cosmos DB (SQL API) mediante Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde almacenes de datos de origen compatibles desde o hacia Azure Cosmos DB (API de SQL) para admitir almacenes de receptor mediante Data Factory.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: jingwang
ms.openlocfilehash: eca5e4cc96996c35e7c2181746cdb3de2e5a602c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61259524"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-sql-api-by-using-azure-data-factory"></a>Copia de datos desde o hacia Azure Cosmos DB (SQL API) mediante Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-azure-documentdb-connector.md)
> * [Versión actual](connector-azure-cosmos-db.md)

En este artículo se resume el uso de la actividad de copia en Azure Data Factory para copiar datos con Azure Cosmos DB (API de SQL) como origen o destino. El artículo se basa en [Actividad de copia en Azure Data Factory](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

>[!NOTE]
>Este conector solo admite la copia de datos hacia y desde Cosmos DB SQL API. Para MongoDB API, consulte [Conector de la API de Azure Cosmos DB para MongoDB](connector-azure-cosmos-db-mongodb-api.md). Actualmente, no se admiten otros tipos de API.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde Azure Cosmos DB (API de SQL) a cualquier almacén de datos de receptor compatible, o desde cualquier almacén de datos de origen admitido a Azure Cosmos DB (API de SQL). Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).

Puede utilizar el conector de Azure Cosmos DB (API de SQL) para:

- Copiar datos desde y hacia la [API de SQL](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction) de Azure Cosmos DB.
- Escriba en Azure Cosmos DB como **insert** o **upsert**.
- Importe y exporte documentos JSON tal cual, o copie datos desde o hacia un conjunto de datos tabular. Entre los ejemplos se incluyen una base de datos SQL y un archivo CSV. Para copiar datos tal cual desde o hacia archivos JSON u otra colección de Azure Cosmos DB, consulte Importación o exportación de documentos JSON.

Data Factory se integra con la [biblioteca BulkExecutor en Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) para proporcionar el mejor rendimiento de escritura en Azure Cosmos DB.

> [!TIP]
> El [vídeo de migración de datos](https://youtu.be/5-SRNiC_qOU) le guía por los pasos para copiar datos desde Azure Blob Storage hasta Azure Cosmos DB. En el vídeo también se describen consideraciones sobre la optimización del rendimiento para la ingesta de datos a Azure Cosmos DB en general.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que puede usar para definir entidades de Data Factory específicas de Azure Cosmos DB (API de SQL).

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Cosmos DB (API de SQL):

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** debe establecerse en **CosmosDb**. | Sí |
| connectionString |Especifique la información necesaria para conectarse a la base de datos de Azure Cosmos DB.<br />**Nota**: Debe especificar la información de la base de datos en la cadena de conexión como se muestra en los ejemplos siguientes. <br/>Marque este campo como SecureString para almacenarlo de forma segura en Data Factory. También puede colocar la clave de cuenta en Azure Key Vault y extraer la configuración `accountKey` de la cadena de conexión. Consulte los siguientes ejemplos y el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) con información detallada. |Sí |
| connectVia | Instancia de [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Se puede usar Azure Integration Runtime o un entorno de ejecución de integración autohospedado (si el almacén de datos se encuentra en una red privada). Si no se especifica esta propiedad, se usa el valor predeterminado de Azure Integration Runtime. |Sin  |

**Ejemplo**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo: Almacenamiento de la clave de cuenta en Azure Key Vault**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;Database=<Database>"
            },
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
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

En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Azure Cosmos DB (API de SQL). 

Para ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos y servicios vinculados](concepts-datasets-linked-services.md). 

Para copiar datos con Azure Cosmos DB (API de SQL) como origen o destino, establezca la propiedad **type** del conjunto de datos en **DocumentDbCollection**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del conjunto de datos debe establecerse en **DocumentDbCollection**. |Sí |
| collectionName |Nombre de la colección de documentos de Azure Cosmos DB. |Sí |

**Ejemplo**

```json
{
    "name": "CosmosDbSQLAPIDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Esquema de Data Factory

En los almacenes de datos sin esquemas, como Azure Cosmos DB, la actividad de copia deduce el esquema de una de las maneras descritas en la lista siguiente. A menos que quiera [importar o exportar documentos JSON tal cual](#import-or-export-json-documents), el procedimiento recomendado consiste en especificar la estructura de datos en la sección **structure**.

* Si especifica la estructura de los datos mediante la propiedad **structure** en la definición del conjunto de datos, Data Factory respeta esta como la estructura del esquema. 

    Si una fila no contiene un valor para una columna, se proporciona un valor nulo para la columna.
* Si no especifica la estructura de los datos mediante la propiedad **structure** en la definición del conjunto de datos, el servicio Data Factory deduce el esquema utilizando la primera fila en los datos. 

    Si la primera fila no contiene el esquema completo, algunas columnas se pueden perder en el resultado de la operación de copia.

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

En esta sección se proporciona una lista de las propiedades que admiten el origen y el receptor de Azure Cosmos DB (API de SQL).

Para ver una lista completa de las secciones y propiedades que hay disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-sql-api-as-source"></a>Azure Cosmos DB (API de SQL) como origen

Para copiar datos desde Azure Cosmos DB (API de SQL), establezca el tipo de **origen** de la actividad de copia en **DocumentDbCollectionSource**. 

La sección **source** de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del origen de la actividad de copia debe establecerse en **DocumentDbCollectionSource**. |Sí |
| query |Especifique la consulta de Azure Cosmos DB para leer datos.<br/><br/>Ejemplo:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Sin  <br/><br/>Si no se especifica, se ejecuta la instrucción SQL: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Carácter especial que indica que el documento está anidado y cómo aplanar el conjunto de resultados.<br/><br/>Por ejemplo, si una consulta de Azure Cosmos DB devuelve el resultado anidado `"Name": {"First": "John"}`, la actividad de copia identifica el nombre de columna como `Name.First`, con el valor "John" cuando **nestedSeparator** sea **.** (punto). |Sin <br />(el valor predeterminado es **.** (punto)) |

**Ejemplo**

```json
"activities":[
    {
        "name": "CopyFromCosmosDBSQLAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cosmos DB SQL API input dataset name>",
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
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-sql-api-as-sink"></a>Azure Cosmos DB (API de SQL) como receptor

Para copiar datos en Azure Cosmos DB (API de SQL), establezca el tipo de **receptor** de la actividad de copia en **DocumentDbCollectionSink**. 

La sección **source** de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del receptor de la actividad de copia debe establecerse en **DocumentDbCollectionSink**. |Sí |
| writeBehavior |Describe cómo escribir datos en Azure Cosmos DB. Valores permitidos: **insert** y **upsert**.<br/><br/>El comportamiento de **upsert** consiste en reemplazar el documento si ya existe un documento con el mismo identificador; en caso contrario, inserta el documento.<br /><br />**Nota**: Data Factory genera automáticamente un identificador para un documento si no se especifica un identificador en el documento original o mediante la asignación de columnas. Esto significa que debe asegurarse de que, para que **upsert** funcione según lo esperado, el documento tenga un identificador. |Sin <br />(el valor predeterminado es **insert**) |
| writeBatchSize | Data Factory usa la [biblioteca BulkExecutor en Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) para escribir datos en Azure Cosmos DB. La propiedad **writeBatchSize** controla el tamaño de los documentos que ADF proporciona a la biblioteca. Puede aumentar el valor de **writeBatchSize** para mejorar el rendimiento y reducir el valor si el documento tiene un tamaño grande: a continuación encontrará sugerencias. |Sin <br />(el valor predeterminado es **10 000**) |
| nestingSeparator |Un carácter especial en el nombre de columna de **origen** que indica que se necesita un documento anidado. <br/><br/>Por ejemplo, `Name.First` en la estructura del conjunto de datos de salida genera la siguiente estructura JSON en el documento de Azure Cosmos DB cuando **nestedSeparator** es punto **.** (punto): `"Name": {"First": "[value maps to this column from source]"}`  |Sin <br />(el valor predeterminado es **.** (punto)) |

>[!TIP]
>Cosmos DB limita el tamaño de las solicitudes únicas a 2 MB. La fórmula es Tamaño de la solicitud = tamaño de documento único x tamaño de lote de escritura. Si aparece un error con el texto **"El tamaño de solicitud es demasiado grande."** , **reduzca el valor de `writeBatchSize`** en la configuración del receptor de copia.

**Ejemplo**

```json
"activities":[
    {
        "name": "CopyToCosmosDBSQLAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="import-or-export-json-documents"></a>Importación o exportación de documentos JSON

Puede utilizar este conector de Azure Cosmos DB (API de SQL) para hacer lo siguiente con facilidad:

* Importar documentos JSON desde varios orígenes a Azure Cosmos DB, incluido Azure Blob Storage, Azure Data Lake Store y otros almacenes basados en archivos compatibles con Azure Data Factory.
* Exportar documentos JSON de una colección de Azure Cosmos DB a varios almacenes basados en archivos.
* Copiar documentos entre dos colecciones de Azure Cosmos DB tal cual.

Para lograr una copia independiente del esquema:

* Cuando use la herramienta Copiar datos, seleccione la opción **Export as-is to JSON files or Cosmos DB collection** (Exportar tal cual a archivos JSON o colección de Cosmos DB).
* Al usar la creación de actividades, no especifique la sección **structure** (también denominada *schema*) en el conjunto de datos de Azure Cosmos DB. Además, no especifique la propiedad **nestingSeparator** en el origen ni el receptor de Azure Cosmos DB en la actividad de copia. Al importar o exportar a archivos JSON, en el conjunto de datos del almacén de archivos correspondiente, especifique el tipo **format** como **JsonFormat** y configure **filePattern** como se describe en la sección [formato JSON](supported-file-formats-and-compression-codecs.md#json-format). Luego, no especifique la sección **structure** y omita el resto de las opciones de formato.

## <a name="next-steps"></a>Pasos siguientes

Para ver una lista de los almacenes de datos que la actividad de copia admite como orígenes y receptores en Azure Data Factory, consulte los [almacenes de datos que se admiten](copy-activity-overview.md##supported-data-stores-and-formats).
