---
title: Copia de datos desde MongoDB mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde MongoDB en almacenes de datos receptores compatibles a través de una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 12/20/2018
ms.date: 04/22/2019
ms.author: v-jay
ms.openlocfilehash: ca6040bb74839f30a2f1b13297f6037f05240c67
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400447"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Copia de datos desde MongoDB mediante Azure Data Factory de Azure

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos desde una base de datos MongoDB. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

>[!IMPORTANT]
>ADF publica esta nueva versión del conector de MongoDB que proporciona una mejor compatibilidad nativa con MongoDB. Si usa la versión anterior del conector de MongoDB en la solución, lo cual se admite tal cual en el caso de la compatibilidad con versiones anteriores, consulte el artículo [MongoDB connector (legacy)](connector-mongodb-legacy.md) [Conector de MongoDB (heredado)].

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde la base de datos MongoDB en cualquier almacén de datos receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector de MongoDB admite hasta la **versión 3.4**.

## <a name="prerequisites"></a>Requisitos previos

Para usar los datos de la copia de una base de datos MongoDB que no es accesible públicamente, debe configurar un entorno Integration Runtime (autohospedado). Consulte el artículo sobre el [entorno Integration Runtime (autohospedado)](create-self-hosted-integration-runtime.md) para obtener más información.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector MongoDB.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de MongoDB:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type |La propiedad type debe establecerse en: **MongoDbV2** |Sí |
| connectionString |Especifique la cadena de conexión de MongoDB, por ejemplo `mongodb://[username:password@]host[:port][/[database][?options]]`. Consulte el [manual de MongoDB sobre cadenas de conexión](https://docs.mongodb.com/manual/reference/connection-string/) para más información. <br/><br />Marque este campo como de tipo **SecureString** para almacenarlo de forma segura en Data Factory. También puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |Sí |
| Base de datos | Nombre de la base de datos a la que desea acceder. | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) o Azure Integration Runtime (si el almacén de datos es accesible públicamente). Si no se especifica, se usará Azure Integration Runtime. |Sin  |

**Ejemplo:**

```json
{
    "name": "MongoDBLinkedService",
    "properties": {
        "type": "MongoDbV2",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "mongodb://[username:password@]host[:port][/[database][?options]]"
            },
            "database": "myDatabase"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Para ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos y servicios vinculados](concepts-datasets-linked-services.md). Las siguientes propiedades son compatibles con el conjunto de datos de MongoDB:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **MongoDbV2Collection** | Sí |
| collectionName |Nombre de la colección en la base de datos de MongoDB. |Sí |

**Ejemplo:**

```json
{
    "name": "MongoDbDataset",
    "properties": {
        "type": "MongoDbV2Collection",
        "linkedServiceName": {
            "referenceName": "<MongoDB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<Collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen MongoDB.

### <a name="mongodb-as-source"></a>MongoDB como origen

Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **MongoDbV2Source** | Sí |
| filter | Especifica el filtro de selección mediante operadores de consulta. Para que se devuelvan todos los documentos de una colección, omita este parámetro o pase un documento vacío ({}). | Sin  |
| cursorMethods.project | Especifica los campos a devolver en los documentos para la proyección. Para devolver todos los campos en los documentos coincidentes, omita este parámetro. | Sin  |
| cursorMethods.sort | Especifica el orden en que la consulta devuelve los documentos coincidentes. Consulte [cursor.sort()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort). | Sin  |
| cursorMethods.limit | Especifica el número máximo de documentos que devuelve el servidor. Consulte [cursor.limit()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit).  | Sin  |
| cursorMethods.skip | Especifica el número de documentos que se omitirán y desde donde empieza MongoDB a devolver resultados. Consulte [cursor.skip()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip). | Sin  |
| batchSize | Especifica el número de documentos a devolver en cada lote de la respuesta de la instancia de MongoDB. En la mayoría de los casos, modificar el tamaño del lote no afectará al usuario ni a la aplicación. Cosmos DB limita el tamaño de cada lote a 40 MB como máximo, que es la suma de los números de batchSize del tamaño de los documentos, por lo que debe reducir este valor si el tamaño del documento es mayor. | Sin <br/>(el valor predeterminado es **100**) |

>[!TIP]
>ADF admite el consumo de documentos BSON en **modo strict**. Asegúrese de que la consulta de filtro está en modo strict en lugar de en modo Shell. Se puede encontrar una descripción más detallada en el [manual de MongoDB](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html).

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromMongoDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MongoDB input dataset name>",
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
                "type": "MongoDbV2Source",
                "filter": "{datetimeData: {$gte: ISODate(\"2018-12-11T00:00:00.000Z\"),$lt: ISODate(\"2018-12-12T00:00:00.000Z\")}, _id: ObjectId(\"5acd7c3d0000000000000000\") }",
                "cursorMethods": {
                    "project": "{ _id : 1, name : 1, age: 1, datetimeData: 1 }",
                    "sort": "{ age : 1 }",
                    "skip": 3,
                    "limit": 3
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="export-json-documents-as-is"></a>Exportación de documentos JSON tal cual

Puede usar este conector de MongoDB para exportar documentos JSON tal cual desde una colección de MongoDB a varios almacenes basados en archivos o a Azure Cosmos DB. Para lograr esa copia independiente del esquema, omita la sección "structure" (también denominada *schema*) en el conjunto de datos y la asignación de esquemas en la actividad de copia.

## <a name="schema-mapping"></a>Asignación de esquemas

Para copiar datos desde MongoDB en un receptor tabular, consulte [asignación de esquemas](copy-activity-schema-and-type-mapping.md#schema-mapping).

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
