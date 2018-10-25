---
title: Copia de datos de Netezza con Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos de Netezza en almacenes de datos receptores compatibles a través de una actividad de copia de una canalización de Azure Data Factory.
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
ms.date: 06/15/2018
ms.author: jingwang
ms.openlocfilehash: 1b7499990a049f276bf1af9e31b639ea4944d8f7
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167579"
---
# <a name="copy-data-from-netezza-by-using-azure-data-factory"></a>Copia de datos de Netezza con Azure Data Factory 

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos de Netezza. El artículo se basa en [Actividad de copia en Azure Data Factory](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos de Netezza en cualquier almacén de datos de receptor admitido. Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory proporciona un controlador integrado para permitir la conectividad. No es necesario instalar manualmente uno para usar este conector.

## <a name="get-started"></a>Introducción

Puede crear una canalización que use una actividad de copia mediante el SDK de .NET, el SDK de Python, Azure PowerShell, la API de REST o una plantilla de Azure Resource Manager. Consulte el [tutorial de la actividad de copia](quickstart-create-data-factory-dot-net.md) para obtener instrucciones paso a paso sobre cómo crear una canalización que tenga una actividad de copia.

En las secciones siguientes se proporcionan detalles sobre las propiedades que puede usar para definir entidades de Data Factory específicas del conector de Netezza.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Netezza:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** debe establecerse en **Netezza**. | SÍ |
| connectionString | Cadena de conexión de ODBC para conectarse a Netezza. Marque este campo como de tipo **SecureString** para almacenarlo de forma segura en Data Factory. También puede [hacen referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | SÍ |
| connectVia | Instancia de [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede elegir una instancia autohospedada de Integration Runtime o Azure Integration Runtime (si el almacén de datos es accesible públicamente). Si no se especifica, se usa el valor predeterminado de Azure Integration Runtime. |Sin  |

Una cadena de conexión típica es `Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>`. En la tabla siguiente se describen más propiedades que puede establecer:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| SecurityLevel | El nivel de seguridad (SSL/TLS) que usa el controlador para la conexión con el almacén de datos. Ejemplo: `SecurityLevel=preferredSecured`. Los valores admitidos son:<br/>- **Only unsecured** (**onlyUnSecured**): el controlador no usa SSL.<br/>- **Preferred unsecured (preferredUnSecured) (valor predeterminado)**: si el servidor proporciona una elección, el controlador no usa SSL. <br/>- **Preferred secured (preferredSecured)**: si el servidor proporciona una elección, el controlador usa SSL. <br/>- **Only secured (onlySecured)**: el controlador no se conectará a menos que haya una conexión SSL disponible. | Sin  |
| CaCertFile | La ruta de acceso completa al certificado SSL que usa el servidor. Ejemplo: `CaCertFile=<cert path>;`| Sí, si se ha habilitado SSL |

**Ejemplo**

```json
{
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
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

En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Netezza.

Para ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos](concepts-datasets-linked-services.md). 

Para copiar datos desde Netezza, establezca la propiedad **type** del conjunto de datos en **NetezzaTable**. No hay ninguna propiedad específica de tipo adicional en este tipo de conjunto de datos.

**Ejemplo**

```json
{
    "name": "NetezzaDataset",
    "properties": {
        "type": "NetezzaTable",
        "linkedServiceName": {
            "referenceName": "<Netezza linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

En esta sección se proporciona una lista de las propiedades que admite el origen de Netezza.

Para ver una lista completa de las secciones y propiedades que hay disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md). 

### <a name="netezza-as-source"></a>Netezza como origen

Para copiar datos desde Netezza, establezca el tipo **source** en la actividad de copia en **NetezzaSource**. La sección **source** de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del origen de la actividad de copia debe establecerse en **NetezzaSource**. | SÍ |
| query | Use la consulta SQL personalizada para leer los datos. Ejemplo: `"SELECT * FROM MyTable"` | SÍ |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromNetezza",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Netezza input dataset name>",
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
                "type": "NetezzaSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Pasos siguientes

Para ver una lista de los almacenes de datos que la actividad de copia admite como orígenes y receptores en Azure Data Factory, consulte los [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
