---
title: Asignación de esquemas en la actividad de copia | Microsoft Docs
description: Obtenga información acerca de cómo la actividad de copia de Azure Data Factory asigna esquemas y tipos de datos desde datos de origen hasta datos receptores al copiar datos.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 9108f83e854b51720c64c5a74a828543cc5e7688
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64875814"
---
# <a name="schema-mapping-in-copy-activity"></a>Asignación de esquemas en la actividad de copia

Este artículo describe el modo en que la actividad de copia de Azure Data Factory realiza la asignación de esquemas y la asignación del tipo de datos desde datos de origen hasta datos receptores al ejecutar la copia de datos.

## <a name="schema-mapping"></a>Asignación de esquemas

Asignación de columnas se aplica cuando se copian datos de origen a receptor. De forma predeterminada, la actividad de copia **asignar datos de origen al receptor por los nombres de columna**. Puede especificar [asignación explícita](#explicit-mapping) para personalizar la asignación de columna según sus necesidades. Más concretamente, la actividad de copia:

1. Lee los datos desde el origen y determina el esquema de origen.
2. Utilice la asignación de columnas predeterminado para asignar columnas por nombre o aplicar la asignación de columnas explícita si se especifica.
3. Escritura de datos en el receptor

### <a name="explicit-mapping"></a>Asignación explícita

Puede especificar las columnas que desea asignar en la actividad de copia -> `translator`  ->  `mappings` propiedad. El ejemplo siguiente define una actividad de copia en una canalización para copiar datos de texto delimitado para Azure SQL Database.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [{
        "referenceName": "DelimitedTextInput",
        "type": "DatasetReference"
    }],
    "outputs": [{
        "referenceName": "AzureSqlOutput",
        "type": "DatasetReference"
    }],
    "typeProperties": {
        "source": { "type": "DelimitedTextSource" },
        "sink": { "type": "SqlSink" },
        "translator": {
            "type": "TabularTranslator",
            "mappings": [
                {
                    "source": {
                        "name": "UserId",
                        "type": "Guid"
                    },
                    "sink": {
                        "name": "MyUserId"
                    }
                }, 
                {
                    "source": {
                        "name": "Name",
                        "type": "String"
                    },
                    "sink": {
                        "name": "MyName"
                    }
                }, 
                {
                    "source": {
                        "name": "Group",
                        "type": "String"
                    },
                    "sink": {
                        "name": "MyGroup"
                    }
                }
            ]
        }
    }
}
```

Se admiten las siguientes propiedades en `translator`  ->  `mappings` -> objeto con `source` y `sink`:

| Propiedad | DESCRIPCIÓN                                                  | Obligatorio |
| -------- | ------------------------------------------------------------ | -------- |
| name     | Nombre de la columna de origen o receptor.                           | Sí      |
| ordinal  | Índice de columna. Iniciar con 1. <br>Aplicar y será necesario cuando uso delimitado por texto sin línea de encabezado. | Sin        |
| path     | Expresión de ruta de acceso JSON para cada campo extraer o asignar. Se aplican para los datos jerárquicos, por ejemplo, MongoDB o REST.<br>Para los campos en el objeto raíz, ruta de acceso JSON se inicia por root $; para los campos dentro de la matriz elegida mediante `collectionReference` propiedad, la ruta de acceso JSON se inicia desde el elemento de matriz. | Sin        |
| type     | Tipo de datos provisionales de factoría de datos de la columna de origen o receptor. | Sin        |
| culture  | Referencia cultural de la columna de origen o receptor. <br>Se aplican cuando el tipo es `Datetime` o `Datetimeoffset`. El valor predeterminado es `en-us`. | Sin        |
| format   | Cadena de formato que se usa cuando el tipo es `Datetime` o `Datetimeoffset`. Consulte [Cadenas con formato de fecha y hora personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) para obtener información sobre el formato de fecha y hora. | Sin        |

Se admiten las siguientes propiedades en `translator`  ->  `mappings` además de objeto con `source` y `sink`:

| Propiedad            | DESCRIPCIÓN                                                  | Obligatorio |
| ------------------- | ------------------------------------------------------------ | -------- |
| collectionReference | Admite solo cuando los datos jerárquicos, por ejemplo, MongoDB o REST están el origen.<br>Si desea iterar y extraer datos de los objetos **dentro de un campo de matriz** con el mismo patrón y convertir al modo por fila por objeto, especifique la ruta de acceso JSON de esa matriz para realizar la aplicación cruzada. | Sin        |

### <a name="alternative-column-mapping"></a>Asignación de columna alternativos

Puede especificar copia -> actividad `translator`  ->  `columnMappings` para asignar entre los datos en forma tabular. En este caso, la sección "structure" es necesario para los conjuntos de datos de entrada y salida. La asignación de columnas admite **la asignación de todas las columnas o un subconjunto de ellas del conjunto de datos de origen "structure" a todas las columnas del conjunto de datos receptor "structure"**. Las siguientes son las condiciones de error que tienen como resultado una excepción:

* El resultado de la consulta del almacén de datos de origen no tiene un nombre de columna que se especifique en la sección "structure" del conjunto de datos de entrada.
* El almacén de datos receptor (si está en el esquema predefinido) no tiene un nombre de columna que se especifique en la sección "structure" del conjunto de datos de salida.
* O bien menos columnas o más columnas en "structure" del conjunto de datos receptor de las que se especifican en la asignación.
* Asignación duplicada.

En el ejemplo siguiente, el conjunto de datos de entrada tiene una estructura y apunta a una tabla en una base de datos de Oracle en el entorno local.

```json
{
    "name": "OracleDataset",
    "properties": {
        "structure":
         [
            { "name": "UserId"},
            { "name": "Name"},
            { "name": "Group"}
         ],
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "OracleLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTable"
        }
    }
}
```

En este ejemplo, el conjunto de datos de salida tiene una estructura y apunta a una tabla en Salesfoce.

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "structure":
        [
            { "name": "MyUserId"},
            { "name": "MyName" },
            { "name": "MyGroup"}
        ],
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "SalesforceLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SinkTable"
        }
    }
}
```

El siguiente fragmento JSON define una actividad de copia en una canalización. Las columnas de origen se asignan a columnas del receptor mediante el uso de la **traductor** -> **columnMappings** propiedad.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [
        {
            "referenceName": "OracleDataset",
            "type": "DatasetReference"
        }
    ],
    "outputs": [
        {
            "referenceName": "SalesforceDataset",
            "type": "DatasetReference"
        }
    ],
    "typeProperties":    {
        "source": { "type": "OracleSource" },
        "sink": { "type": "SalesforceSink" },
        "translator":
        {
            "type": "TabularTranslator",
            "columnMappings":
            {
                "UserId": "MyUserId",
                "Group": "MyGroup",
                "Name": "MyName"
            }
        }
    }
}
```

Si estaba usando la sintaxis de `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` para especificar una asignación de columnas, esta se sigue admitiendo tal cual.

### <a name="alternative-schema-mapping"></a>Asignación de esquema alternativo

Puede especificar copia -> actividad `translator`  ->  `schemaMapping` entre los datos en forma jerárquica y datos tabulares en forma de mapa, por ejemplo, copiar de MongoDB/REST al archivo de texto y copie desde Oracle a la API de Azure Cosmos DB para MongoDB. Se admiten las siguientes propiedades en la sección `translator` de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del traductor de la actividad de copia debe establecerse en: **TabularTranslator** | Sí |
| schemaMapping | Una colección de pares de clave-valor, que representa la relación de asignación **desde el lado del origen al receptor del lado**.<br/>- **Clave:** origen representa. Para **origen tabular**, especifique el nombre de columna tal como se define en la estructura del conjunto de datos; para **origen jerárquica**, especifique la expresión de ruta de acceso JSON para cada campo extraer y asignar.<br>- **Valor:** representa receptor. Para **receptor tabular**, especifique el nombre de columna tal como se define en la estructura del conjunto de datos; para **receptor jerárquica**, especifique la expresión de ruta de acceso JSON para cada campo extraer y asignar. <br>En el caso de los datos jerárquicos, para los campos en el objeto raíz, la ruta de acceso JSON se inicia por root $; para los campos dentro de la matriz elegida mediante `collectionReference` propiedad, la ruta de acceso JSON se inicia desde el elemento de matriz.  | Sí |
| collectionReference | Si desea iterar y extraer datos de los objetos **dentro de un campo de matriz** con el mismo patrón y convertir al modo por fila por objeto, especifique la ruta de acceso JSON de esa matriz para realizar la aplicación cruzada. Esta propiedad solo se admite si el origen son datos jerárquicos. | Sin  |

**Ejemplo: copia de MongoDB a Oracle:**

Por ejemplo, si tiene un documento de MongoDB con el siguiente contenido:

```json
{
    "id": {
        "$oid": "592e07800000000000000000"
    },
    "number": "01",
    "date": "20170122",
    "orders": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "name": "Seattle" } ]
}
```

y desea copiarlo en una tabla de Azure SQL del formato siguiente, puede hacerlo mediante el acoplamiento de los datos dentro de la matriz *(order_pd y order_price)* y la combinación cruzada con la información de la raíz habitual *(número, fecha y ciudad)*:

| orderNumber | orderDate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | Seattle |
| 01 | 20170122 | P2 | 13 | Seattle |
| 01 | 20170122 | P3 | 231 | Seattle |

Configure la regla de asignación de esquemas como el siguiente ejemplo JSON de actividad de copia:

```json
{
    "name": "CopyFromMongoDBToOracle",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "MongoDbV2Source"
        },
        "sink": {
            "type": "OracleSink"
        },
        "translator": {
            "type": "TabularTranslator",
            "schemaMapping": {
                "orderNumber": "$.number",
                "orderDate": "$.date",
                "order_pd": "prod",
                "order_price": "price",
                "city": " $.city[0].name"
            },
            "collectionReference":  "$.orders"
        }
    }
}
```

## <a name="data-type-mapping"></a>Asignación de tipos de datos

La actividad de copia realiza la asignación de tipos de origen a tipos receptores con este proceso de dos pasos:

1. Conversión de los tipos de origen nativo a tipos de datos provisionales de Azure Data Factory
2. Conversión de los tipos de datos provisionales de Azure Data Factory al tipo de receptor nativo

Puede encontrar la asignación entre un tipo nativo y un tipo provisional en la sección "Asignación de tipos de datos" en cada tema de conector.

### <a name="supported-data-types"></a>Tipos de datos admitidos

Data Factory admite los siguientes tipos de datos provisionales: Puede especificar los siguientes valores cuando configure la información del tipo en la configuración de la [estructura del conjunto de datos](concepts-datasets-linked-services.md#dataset-structure-or-schema):

* Byte[]
* Boolean
* Datetime
* Datetimeoffset
* Decimal
* Double
* Guid
* Int16
* Int32
* Int64
* Single
* String
* Timespan

## <a name="next-steps"></a>Pasos siguientes
Consulte los otros artículos de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
