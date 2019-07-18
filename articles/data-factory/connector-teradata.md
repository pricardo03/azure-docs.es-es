---
title: Copia de datos de Teradata mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre el conector Teradata del servicio Data Factory que permite copiar datos desde una base de datos Teradata a almacenes de datos compatibles con Data Factory como receptores.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 63f28c8b6eaceed12e1f76e9c0c5984e3b63b500
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561423"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Copia de datos de Teradata mediante Azure Data Factory
> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
>
> * [Versión 1](v1/data-factory-onprem-teradata-connector.md)
> * [Versión actual](connector-teradata.md)

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos desde una base de datos Teradata. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde una base de datos Teradata a cualquier almacén de datos receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector Teradata admite las siguientes funcionalidades:

- Teradata **versión 14.10, 15.0, 15.10, 16.0, 16.10 y 16.20**.
- La copia de datos con autenticación **básica** o de **Windows**.
- Copia en paralelo desde el origen Teradata. Consulte la sección [Copia en paralelo desde Teradata](#parallel-copy-from-teradata) para obtener más detalles.

> [!NOTE]
>
> Azure Data Factory ha actualizado el conector Teradata desde IR autohospedado v3.18, que habilita un controlador ODBC integrado y ofrece opciones flexibles de conexión, así como una copia en paralelo lista para usar con el fin de aumentar el rendimiento. Las cargas de trabajo existentes mediante el conector Teradata anterior que habilita el Proveedor de datos .NET para Teradata todavía se admite tal cual, pero se sugiere usar una nueva en el futuro. Tenga en cuenta que la nueva ruta de acceso requiere un conjunto distinto de origen vinculado de servicio, conjunto de datos o copia. Para obtener más información sobre los detalles de configuración, consulte la sección correspondiente.

## <a name="prerequisites"></a>Requisitos previos

Si Teradata no está accesible públicamente, debe configurar un IR autohospedado. Para más información acerca de un entorno de ejecución de integración, consulte el artículo sobre [Entorno de ejecución de integración autohospedado](create-self-hosted-integration-runtime.md). Integration Runtime proporciona un controlador Teradata integrado a partir de la versión 3.18, por lo tanto, no es necesario que instale uno manualmente. El controlador requiere "Visual C++ Redistributable 2012 Update 4" en la máquina de IR autohospedado, descárguelo [aquí](https://www.microsoft.com/en-sg/download/details.aspx?id=30679) si aún no lo tiene instalado.

Si tiene una versión de IR autohospedado anterior a 3.18, debe instalar el [Proveedor de datos .NET para Teradata](https://go.microsoft.com/fwlink/?LinkId=278886), versión 14 o una posterior, en la máquina de Integration Runtime. 

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector Teradata.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado Teradata:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **Teradata** | Sí |
| connectionString | Especifica la información necesaria para conectarse a la instancia de Base de datos de Teradata. Consulte los ejemplos siguientes.<br/>También puede colocar la contraseña en Azure Key Vault y extraer la configuración de `password` de la cadena de conexión. Consulte el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) para obtener información detallada. | Sí |
| nombre de usuario | Especifique el nombre de usuario para conectarse a la base de datos Teradata. Se aplica cuando se usa autenticación de Windows. | Sin |
| contraseña | Especifique la contraseña de la cuenta de usuario especificada para el nombre de usuario. También puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). <br>Se aplica al utilizar la autenticación de Windows o hacer referencia a la contraseña en Key Vault para la autenticación básica. | Sin |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Tal y como se mencionó en los [requisitos previos](#prerequisites), se requiere un entorno Integration Runtime autohospedado. |Sí |

**Ejemplo: uso de la autenticación básica**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>;Uid=<username>;Pwd=<password>"
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
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>",
            "username": "<username>",
            "password": "<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

> [!NOTE]
>
> Si usa el servicio vinculado Teradata que habilita el Proveedor de datos .NET para Teradata con la siguiente carga, todavía se admite tal cual, aunque se aconseja usar la versión nueva en el futuro.

**Carga anterior:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<Basic/Windows>",
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre conjuntos de datos. En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Teradata.

Para copiar datos de Teradata, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del conjunto de datos debe establecerse en: **TeradataTable** | Sí |
| Base de datos | Nombre de la base de datos de Teradata. | No (si se especifica "query" en el origen de la actividad) |
| table | Nombre de la tabla de la base de datos Teradata. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "TeradataTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

> [!NOTE]
>
> Si estaba usando un conjunto de datos de tipo "RelationalTable" como el siguiente, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

**Carga anterior:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de Teradata.

### <a name="teradata-as-source"></a>Teradata como origen

> [!TIP]
>
> Obtenga más información en la sección [Copia en paralelo desde Teradata](#parallel-copy-from-teradata) sobre cómo cargar datos desde Teradata de forma eficaz con la creación de particiones de datos.

Para copiar datos desde Teradata, en la sección **source** de la actividad de copia se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del origen de la actividad de copia debe establecerse en: **TeradataSource** | Sí |
| query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM MyTable"`.<br>Si habilita la carga con particiones, deberá enlazar los parámetros de partición integrados correspondientes en la consulta. Consulte ejemplos en la sección [Copia en paralelo desde Teradata](#parallel-copy-from-teradata). | No (si se especifica la tabla en el conjunto de datos) |
| partitionOptions | Especifica las opciones de creación de particiones de datos que se usan para cargar datos desde Teradata. <br>Los valores permitidos son los siguientes: **None** (valor predeterminado), **Hash** y **DynamicRange**.<br>Si la opción de partición está habilitada (no es "None"), configure también el valor **[`parallelCopies`](copy-activity-performance.md#parallel-copy)** en la actividad de copia (por ejemplo, como 4), que determina el grado paralelo para cargar los datos simultáneamente desde la base de datos de Teradata. | Sin |
| partitionSettings | Especifique el grupo de configuración para la creación de particiones de datos. <br>Se aplica cuando la opción de partición no es `None`. | Sin |
| partitionColumnName | Especifique el nombre de la columna de origen **in integer type** que usará la creación de particiones por rangos para la copia en paralelo. Si no se especifica, se detectará automáticamente la clave principal de la tabla y se usará como columna de partición. <br>Se aplica cuando la opción de partición es `Hash` o `DynamicRange`. Si usa la consulta para recuperar datos de origen, enlace `?AdfHashPartitionCondition` o `?AdfRangePartitionColumnName` en la cláusula WHERE. Consulte un ejemplo en la sección [Copia en paralelo desde Teradata](#parallel-copy-from-teradata). | Sin |
| partitionUpperBound | Valor máximo de la columna de partición para copiar datos de salida. <br>Se aplica cuando la opción de partición es `DynamicRange`. Si usa la consulta para recuperar datos de origen, enlace `?AdfRangePartitionUpbound` en la cláusula WHERE. Consulte un ejemplo en la sección [Copia en paralelo desde Teradata](#parallel-copy-from-teradata). | Sin |
| PartitionLowerBound | Valor mínimo de la columna de partición para copiar datos de salida. <br>Se aplica cuando la opción de partición es `DynamicRange`. Si usa la consulta para recuperar datos de origen, enlace `?AdfRangePartitionLowbound` en la cláusula WHERE. Consulte un ejemplo en la sección [Copia en paralelo desde Teradata](#parallel-copy-from-teradata). | Sin |

> [!NOTE]
>
> Si estaba usando el origen de copia de tipo "RelationalSource", todavía se admite tal cual, pero no es compatible con la nueva carga en paralelo integrada desde Teradata (opciones de partición). Se sugiere usar este nuevo en el futuro.

**Ejemplo: copia de datos mediante una consulta básica sin partición**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
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
                "type": "TeradataSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="parallel-copy-from-teradata"></a>Copia en paralelo desde Teradata

El conector de Teradata de la factoría de datos proporciona la creación de particiones de datos integrados para copiar datos de Teradata en paralelo con un gran rendimiento. Puede encontrar las opciones de creación de particiones de datos en la actividad de copia -> Origen de Teradata:

![Opciones de partición](./media/connector-teradata/connector-teradata-partition-options.png)

Al habilitar la copia con particiones, la factoría de datos ejecuta consultas en paralelo en el origen de Teradata para cargar los datos mediante particiones. El grado en paralelo se configura y se controla mediante la configuración **[`parallelCopies`](copy-activity-performance.md#parallel-copy)** de la actividad de copia. Por ejemplo, si establece `parallelCopies` como cuatro, la factoría de datos genera y ejecuta al mismo tiempo cuatro consultas de acuerdo con la configuración y la opción de partición que ha especificado, y cada una recupera una porción de datos de la base de datos de Teradata.

Se le sugiere que habilite la copia en paralelo con la creación de particiones de datos, especialmente si carga grandes cantidades de datos de la base de datos de Teradata. Estas son algunas configuraciones sugeridas para diferentes escenarios:

| Escenario                                                     | Configuración sugerida                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Carga completa de una tabla grande                                   | **Opción de partición**: hash. <br><br/>Durante la ejecución, la factoría de datos detecta automáticamente la columna PK, le aplica hash y copia los datos mediante particiones. |
| Carga de grandes cantidades de datos mediante una consulta personalizada                 | **Opción de partición**: hash.<br>**Consulta**: `SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>`.<br>**Columna de partición**: especifique la columna usada para aplicar la partición hash. Si no se especifica, ADF detectará automáticamente la columna PK de la tabla que ha especificado en el conjunto de datos de Teradata.<br><br>Durante la ejecución, la factoría de datos reemplaza `?AdfHashPartitionCondition` por la lógica de partición hash y la envía a Teradata. |
| Carga de grandes cantidades de datos mediante una consulta personalizada, con una columna de enteros con valor distribuido uniformemente para la creación de particiones por rangos | **Opciones de partición**: partición por rangos dinámica.<br>**Consulta**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Columna de partición**: especifique la columna usada para crear la partición de datos. Puede crear particiones en columnas con un tipo de datos entero.<br>**Límite de partición superior** y **límite de partición inferior**: especifique si quiere filtrar en la columna de partición para recuperar solo los datos entre el intervalo inferior y el superior.<br><br>Durante la ejecución, la factoría de datos reemplaza `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound` y `?AdfRangePartitionLowbound` por el nombre real de la columna y los intervalos de valor de cada partición y se los envía a Teradata. <br>Por ejemplo, si establece la columna de partición "ID" con un límite inferior de 1 y un límite superior de 80, con la copia en paralelo establecida en 4, ADF recupera los datos mediante 4 particiones con ID entre [1, 20], [21, 40], [41, 60] y [61, 80]. |

**Ejemplo: consulta con partición hash**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "Hash",
    "partitionSettings": {
        "partitionColumnName": "<hash_partition_column_name>"
    }
}
```

**Ejemplo: consulta con partición por rangos dinámica**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<dynamic_range_partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-teradata"></a>Asignación de tipos de datos para Teradata

Al copiar datos desde Teradata, se usan las siguientes asignaciones de tipos de datos de Teradata en los tipos de datos provisionales de Azure Data Factory. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Base de datos Teradata | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| Char |Cadena |
| Clob |Cadena |
| Date |Datetime |
| Decimal |Decimal |
| Double |Double |
| Graphic |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Entero |Int32 |
| Interval Day |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Day To Hour |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Day To Minute |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Day To Second |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Hour |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Hour To Minute |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Hour To Second |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Minute |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Minute To Second |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Month |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Second |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Year |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Interval Year To Month |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Number |Double |
| Period (Date) |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Period (Time) |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Period (Time With Time Zone) |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Period (Timestamp) |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Period (Timestamp With Time Zone) |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| SmallInt |Int16 |
| Hora |TimeSpan |
| Time With Time Zone |TimeSpan |
| Timestamp |Datetime |
| Timestamp With Time Zone |Datetime |
| VarByte |Byte[] |
| VarChar |Cadena |
| VarGraphic |No compatible. Se aplica la conversión explícita en la consulta de origen. |
| Xml |No compatible. Se aplica la conversión explícita en la consulta de origen. |


## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
