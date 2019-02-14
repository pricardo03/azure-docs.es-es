---
title: Copia de datos con Azure Data Explorer como origen o destino mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar con Azure Data Explorer como origen o destino mediante una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: orspod
ms.openlocfilehash: 8f2a7a953ce2964645c281d9454a73b0cf1a8ff6
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55747195"
---
# <a name="copy-data-to-or-from-azure-data-explorer-using-azure-data-factory"></a>Copia de datos con Azure Data Explorer como origen o destino mediante Azure Data Factory

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos con [Azure Data Explorer](../data-explorer/data-explorer-overview.md) como origen o datos. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde cualquier almacén de datos de origen compatible a Azure Data Explorer. Además, puede copiar datos desde Azure Data Explorer a cualquier almacén de datos de receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

>[!NOTE]
>Actualmente, no se admite copiar datos hacia/desde Azure Data Explorer desde/hacia un almacén de datos local con entorno de ejecución de integración autohospedado.

El conector de Azure Data Explorer le permite hacer lo siguiente:

* Copiar datos mediante la autenticación de tokens de aplicaciones de Azure Active Directory (Azure AD) con una **entidad de servicio**.
* Como origen, recupere los datos mediante una consulta KQL (Kusto).
* Como receptor, anexe datos a una tabla de destino.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector de Azure Data Explorer.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Data Explorer:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** se debe establecer en **AzureDataExplorer** | Sí |
| punto de conexión | Dirección URL del punto de conexión del clúster de Azure Data Explorer, con el formato como `https://<clusterName>.<regionName>.kusto.windows.net `. | Sí |
| Base de datos | Nombre de la base de datos. | Sí |
| tenant | Especifique la información del inquilino (nombre de dominio o identificador de inquilino) en el que reside la aplicación. Para recuperarla, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. | Sí |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | Sí |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como [SecureString](store-credentials-in-key-vault.md) para almacenarlo de forma segura en Data Factory, o bien **para hacer referencia a un secreto almacenado en Azure Key Vault**. | Sí |

**Ejemplo de propiedades del servicio vinculado:**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
            "tenant": "<tenant name/id e.g. microsoft.onmicrosoft.com>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Azure Data Explorer.

Para copiar datos a Azure Data Explorer, establezca la propiedad type del conjunto de datos en **AzureDataExplorerTable**.

Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** debe establecerse en **AzureDataExplorerTable** | Sí |
| table | El nombre de la tabla a la que hace referencia el servicio vinculado. | Sí para el receptor, no para el origen |

**Ejemplo de propiedades del conjunto de datos**

```json
{
   "name": "AzureDataExplorerDataset",
    "properties": {
        "type": "AzureDataExplorerTable",
        "linkedServiceName": {
            "referenceName": "<Azure Data Explorer linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "table": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admiten el receptor y el origen de Azure Data Explorer.

### <a name="azure-data-explorer-as-source"></a>Azure Data Explorer como origen

Para copiar datos desde Azure Data Explorer, establezca la propiedad **type** del origen de la actividad de copia en **AzureDataExplorerSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del origen de la actividad de copia debe establecerse en: **AzureDataExplorerSource** | Sí |
| query | Una solicitud de solo lectura dada en un [formato KQL](/azure/kusto/query/). Use la consulta KQL personalizada como referencia. | Sí |
| queryTimeout | El tiempo de espera antes de que se agote el tiempo de espera de la solicitud de consulta. El valor predeterminado es de 10 minutos (00:10:00); el valor máximo permitido es de 1 hora (01: 00:00). | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataExplorerSource",
                "query": "TestTable1 | take 10",
                "queryTimeout": "00:10:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Explorer input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

### <a name="azure-data-explorer-as-sink"></a>Azure Data Explorer como receptor

Para copiar datos en Azure Data Explorer, establezca la propiedad type del receptor de la actividad de copia en **AzureDataExplorerSink**. Se admiten las siguientes propiedades en la sección **sink** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del receptor de la actividad de copia debe establecerse en: **AzureDataExplorerSink** | Sí |
| ingestionMappingName | Nombre de la [asignación csv](/azure/kusto/management/mappings#csv-mapping) creada previamente en una tabla de Kusto. Para asignar las columnas de origen a Azure Data Explorer, también puede usar la actividad de copia [asignación de columnas](copy-activity-schema-and-type-mapping.md). | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyToAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataExplorerSink",
                "ingestionMappingName": "<optional csv mapping name>"
            }
        },
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Explorer output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.