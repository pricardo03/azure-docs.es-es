---
title: Copia de datos desde MySQL mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre el conector MySQL de Azure Data Factory que permite copiar los datos desde una base de datos MySQL a un almacén de datos admitido como receptor.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: jingwang
ms.openlocfilehash: c14bf7c108251a4ec00f5e2f0b1254f83121866e
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321116"
---
# <a name="copy-data-from-mysql-using-azure-data-factory"></a>Copia de datos desde MySQL mediante Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-onprem-mysql-connector.md)
> * [Versión actual](connector-mysql.md)

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos desde una base de datos MySQL. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde la base de datos MySQL en cualquier almacén de datos receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector MySQL es compatible con las **versiones 5.6 y 5.7** de MySQL.

## <a name="prerequisites"></a>Requisitos previos

Si la base de datos MySQL que no está accesible públicamente, debe configurar un entorno Integration Runtime (autohospedado). Para obtener información sobre los entornos Integration Runtime (autohospedado), consulte el artículo sobre los [entornos Integration Runtime (autohospedado)](create-self-hosted-integration-runtime.md). El entorno Integration Runtime proporciona un controlador MySQL integrado a partir de la versión 3.7, por lo tanto, no es necesario que instale uno manualmente.

Para una versión de IR autohospedado anterior a 3.7, debe instalar una versión entre 6.6.5 y 6.10.7 del [conector MySQL o Net para Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) en la máquina de Integration Runtime. Este controlador de 32 bits es compatible con IR de 64 bits.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector MySQL.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de MySQL:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type debe establecerse en: **MySQL** | SÍ |
| connectionString | Especifique la información necesaria para conectarse a la instancia de Azure Database for MySQL. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | SÍ |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) o Azure Integration Runtime (si el almacén de datos es accesible públicamente). Si no se especifica, se usará Azure Integration Runtime. |Sin  |

Una cadena de conexión típica es `Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>`. Más propiedades que puede establecer para su caso:

| Propiedad | DESCRIPCIÓN | Opciones | Obligatorio |
|:--- |:--- |:--- |:--- |:--- |
| SSLMode | Esta opción especifica si el controlador utiliza cifrado SSL y comprobación durante la conexión a MySQL. Por ejemplo, `SSLMode=<0/1/2/3/4>`| DISABLED (0) / PREFERRED (1) **(valor predeterminado)** / REQUIRED (2) / VERIFY_CA (3) / VERIFY_IDENTITY (4) | Sin  |
| UseSystemTrustStore | Esta opción concreta si se usa un certificado de entidad de certificación del almacén de confianza del sistema o de un archivo PEM especificado. Por ejemplo, `UseSystemTrustStore=<0/1>;`| Habilitado (1) / Deshabilitado (0) **(valor predeterminado)** | Sin  |

**Ejemplo:**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Si usaba el servicio MySQL vinculado con la siguiente carga, todavía se admite tal cual, aunque se aconseja usar la nueva en el futuro.

**Carga anterior:**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "username": "<username>",
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre conjuntos de datos. En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de MySQL.

Para copiar datos desde MySQL, establezca la propiedad type del conjunto de datos en **RelationalTable**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del conjunto de datos debe establecerse en: **RelationalTable** | SÍ |
| tableName | Nombre de la tabla de la base de datos MySQL. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "MySQLDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<MySQL linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen MySQL.

### <a name="mysql-as-source"></a>MySQL como origen

Para copiar datos desde MySQL, establezca el tipo de origen de la actividad de copia como **RelationalSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del origen de la actividad de copia debe establecerse en: **RelationalSource** | SÍ |
| query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM MyTable"`. | No (si se especifica "tableName" en el conjunto de datos) |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromMySQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MySQL input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-mysql"></a>Asignación de tipos de datos de MySQL

Al copiar datos desde MySQL, se utilizan las siguientes asignaciones de tipos de datos de MySQL en los tipos de datos provisionales de Azure Data Factory. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipo de datos de MySQL | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| `bigint` |`Int64` |
| `bigint unsigned` |`Decimal` |
| `bit(1)` |`Boolean` |
| `bit(M), M>1`|`Byte[]`|
| `blob` |`Byte[]` |
| `bool` |`Int16` |
| `char` |`String` |
| `date` |`Datetime` |
| `datetime` |`Datetime` |
| `decimal` |`Decimal, String` |
| `double` |`Double` |
| `double precision` |`Double` |
| `enum` |`String` |
| `float` |`Single` |
| `int` |`Int32` |
| `int unsigned` |`Int64`|
| `integer` |`Int32` |
| `integer unsigned` |`Int64` |
| `long varbinary` |`Byte[]` |
| `long varchar` |`String` |
| `longblob` |`Byte[]` |
| `longtext` |`String` |
| `mediumblob` |`Byte[]` |
| `mediumint` |`Int32` |
| `mediumint unsigned` |`Int64` |
| `mediumtext` |`String` |
| `numeric` |`Decimal` |
| `real` |`Double` |
| `set` |`String` |
| `smallint` |`Int16` |
| `smallint unsigned` |`Int32` |
| `text` |`String` |
| `time` |`TimeSpan` |
| `timestamp` |`Datetime` |
| `tinyblob` |`Byte[]` |
| `tinyint` |`Int16` |
| `tinyint unsigned` |`Int16` |
| `tinytext` |`String` |
| `varchar` |`String` |
| `year` |`Int` |

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
