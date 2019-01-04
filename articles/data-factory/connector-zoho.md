---
title: Copiar datos de Zoho con Azure Data Factory (versión preliminar) | Microsoft Docs
description: Obtenga información sobre cómo copiar datos de Zoho en almacenes de datos receptores compatibles a través de una actividad de copia de una canalización de Azure Data Factory.
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
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: 75594bf77f9bde7549b14e3a154f18ba67ebac3d
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53103394"
---
# <a name="copy-data-from-zoho-using-azure-data-factory-preview"></a>Copiar datos de Zoho con Azure Data Factory (versión preliminar)

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos de Zoho. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

> [!IMPORTANT]
> Este conector está actualmente en versión preliminar. Puede probarlo y enviarnos sus comentarios. Si desea depender de los conectores de versión preliminar en la solución, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos de Zoho en cualquier almacén de datos de receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

Azure Data Factory proporciona un controlador integrado para habilitar la conectividad. Por lo tanto, no es necesario instalar manualmente ningún controlador mediante este conector.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector de Zoho.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Zoho:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type debe establecerse en: **Zoho** | SÍ |
| punto de conexión | Punto de conexión del servidor de Zoho (`crm.zoho.com/crm/private`). | SÍ |
| accessToken | Token de acceso para la autenticación de Zoho. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | SÍ |
| useEncryptedEndpoints | Especifica si los puntos de conexión de origen de datos se cifran mediante HTTPS. El valor predeterminado es true.  | Sin  |
| useHostVerification | Especifica si se requiere que el nombre de host del certificado del servidor coincida con el nombre de host del servidor al conectarse a través de SSL. El valor predeterminado es true.  | Sin  |
| usePeerVerification | Especifica si se debe verificar la identidad del servidor al conectarse a través de SSL. El valor predeterminado es true.  | Sin  |

**Ejemplo:**

```json
{
    "name": "ZohoLinkedService",
    "properties": {
        "type": "Zoho",
        "typeProperties": {
            "endpoint" : "crm.zoho.com/crm/private",
            "accessToken": {
                 "type": "SecureString",
                 "value": "<accessToken>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades compatibles con el conjunto de datos de Zoho.

Para copiar datos de Zoho, establezca la propiedad type del conjunto de datos en **ZohoObject**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del conjunto de datos debe establecerse en: **ZohoObject** | SÍ |
| tableName | Nombre de la tabla. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "ZohoDataset",
    "properties": {
        "type": "ZohoObject",
        "linkedServiceName": {
            "referenceName": "<Zoho linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el origen de Zoho.

### <a name="zoho-as-source"></a>Zoho como origen

Para copiar datos de Zoho, establezca el tipo de origen de la actividad de copia en **ZohoSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del origen de la actividad de copia debe establecerse en: **ZohoSource** | SÍ |
| query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM Accounts"`. | No (si se especifica "tableName" en el conjunto de datos) |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromZoho",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Zoho input dataset name>",
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
                "type": "ZohoSource",
                "query": "SELECT * FROM Accounts"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
