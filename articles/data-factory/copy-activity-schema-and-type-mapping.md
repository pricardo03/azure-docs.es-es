---
title: Asignación de esquemas en la actividad de copia | Microsoft Docs
description: Obtenga información acerca de cómo la actividad de copia de Azure Data Factory asigna esquemas y tipos de datos desde datos de origen hasta datos receptores al copiar datos.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 1a3855b7b95224e0f872764f6710f9fa907780a7
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025460"
---
# <a name="schema-mapping-in-copy-activity"></a>Asignación de esquemas en la actividad de copia
Este artículo describe el modo en que la actividad de copia de Azure Data Factory realiza la asignación de esquemas y la asignación del tipo de datos desde datos de origen hasta datos receptores al ejecutar la copia de datos.

## <a name="column-mapping"></a>Asignación de columnas

La asignación de columnas se aplica al copiar datos con forma tabular. De forma predeterminada, la actividad de copia **asigna datos de origen a datos receptores por nombres de columna**, a menos que esté configurada la [asignación de columnas explícita](#explicit-column-mapping). Más concretamente, la actividad de copia:

1. Lee los datos desde el origen y determina el esquema de origen.

    * En el caso de los orígenes de datos con un esquema predefinido en el almacén de datos o el formato de archivo, como las bases de datos o los archivos con metadatos (Avro/ORC/Parquet/Texto con encabezado), el esquema de origen se extrae del resultado de la consulta o los metadatos del archivo.
    * Para los orígenes de datos con un esquema flexible, como Azure Table/Cosmos DB, el esquema de origen se deduce del resultado de la consulta. Es posible sobrescribirlo configurando el elemento "structure" en el conjunto de datos.
    * Para el archivo de texto sin encabezado, los nombres de columna predeterminados se generan con el patrón "Prop_0", "Prop_1"... Se pueden sobrescribir configurando el elemento "structure" en el conjunto de datos.
    * Para el origen de Dynamics, tendrá que proporcionar la información de esquema en la sección "structure" del conjunto de datos.

2. Aplique la asignación de columnas explícita si se especifica.

3. Escritura de datos en el receptor

    * En los almacenes de datos con un esquema definido previamente, los datos se escriben en las columnas con el mismo nombre.
    * En los almacenes de datos sin esquema fijo y en los formatos de archivo, los nombres de columna o metadatos se generarán en función del esquema de origen.

### <a name="explicit-column-mapping"></a>Asignación de columnas explícita

Puede especificar **columnMappings** en la sección **typeProperties** de la actividad de copia para realizar una asignación de columna explícita. En este escenario, la sección "structure" es necesaria para los conjuntos de datos de entrada y salida. La asignación de columnas admite **la asignación de todas las columnas o un subconjunto de ellas del conjunto de datos de origen "structure" a todas las columnas del conjunto de datos receptor "structure"**. Las siguientes son las condiciones de error que tienen como resultado una excepción:

* El resultado de la consulta del almacén de datos de origen no tiene un nombre de columna que se especifique en la sección "structure" del conjunto de datos de entrada.
* El almacén de datos receptor (si está en el esquema predefinido) no tiene un nombre de columna que se especifique en la sección "structure" del conjunto de datos de salida.
* O bien menos columnas o más columnas en "structure" del conjunto de datos receptor de las que se especifican en la asignación.
* Asignación duplicada.

#### <a name="explicit-column-mapping-example"></a>Ejemplo de asignación de columnas explícita

En este ejemplo, la tabla de entrada tiene una estructura y apunta a una tabla en una base de datos SQL local.

```json
{
    "name": "SqlServerInput",
    "properties": {
        "structure":
         [
            { "name": "UserId"},
            { "name": "Name"},
            { "name": "Group"}
         ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "SqlServerLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTable"
        }
    }
}
```

En este ejemplo, la tabla de salida tiene una estructura y apunta a una tabla en una base de datos SQL de Azure.

```json
{
    "name": "AzureSqlOutput",
    "properties": {
        "structure":
        [
            { "name": "MyUserId"},
            { "name": "MyName" },
            { "name": "MyGroup"}
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SinkTable"
        }
    }
}
```

El siguiente fragmento JSON define una actividad de copia en una canalización. Las columnas del origen se asignan a columnas del receptor (**columnMappings**) usando la propiedad **Translator**.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [
        {
            "referenceName": "SqlServerInput",
            "type": "DatasetReference"
        }
    ],
    "outputs": [
        {
            "referenceName": "AzureSqlOutput",
            "type": "DatasetReference"
        }
    ],
    "typeProperties":    {
        "source": { "type": "SqlSource" },
        "sink": { "type": "SqlSink" },
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

**Flujo de asignación de columnas:**

![Flujo de asignación de columnas](./media/copy-activity-schema-and-type-mapping/column-mapping-sample.png)

## <a name="schema-mapping"></a>Asignación de esquemas

La asignación de esquemas se aplica al copiar datos entre tablas de datos jerárquicos y datos tabulares como, por ejemplo, al copiar de MongoDB o REST a un archivo de texto, o al copiar de SQL en la API de Azure Cosmos DB para MongoDB. Se admiten las siguientes propiedades en la sección `translator` de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del traductor de la actividad de copia debe establecerse en: **TabularTranslator** | SÍ |
| schemaMapping | Una colección de pares clave-valor, que representa la relación de la asignación del lado tabular al lado jerárquico.<br/>- **Clave:** el nombre de columna de los datos tabulares tal y como se define en la estructura del conjunto de datos.<br/>- **Valor:** la expresión de ruta de acceso JSON de cada campo para su extracción y asignación. Para los campos en el objeto raíz, comience por root $; para los campos dentro de la matriz elegida mediante la propiedad `collectionReference`, empiece desde el elemento de matriz.  | SÍ |
| collectionReference | Si desea iterar y extraer datos de los objetos **dentro de un campo de matriz** con el mismo patrón y convertir al modo por fila por objeto, especifique la ruta de acceso JSON de esa matriz para realizar la aplicación cruzada. Esta propiedad solo se admite si el origen son datos jerárquicos. | Sin  |

**Ejemplo: copia de MongoDB a SQL:**

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
    "name": "CopyFromMongoDBToSqlAzure",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "MongoDbV2Source"
        },
        "sink": {
            "type": "SqlSink"
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

Data Factory admite los siguientes tipos de datos provisionales: Puede especificar los siguientes valores cuando configure la información del tipo en la configuración de la [estructura del conjunto de datos](concepts-datasets-linked-services.md#dataset-structure):

* Byte[]
* boolean
* DateTime
* Datetimeoffset
* DECIMAL
* Doble
* Guid
* Int16
* Int32
* Int64
* Single
* string
* TimeSpan

### <a name="explicit-data-type-conversion"></a>Conversión de tipos de datos explícitos

Al copiar datos en almacenes de datos con esquema fijo (como SQL Server y Oracle), cuando el origen y el receptor tienen un tipo diferente en la misma columna, la conversión de tipos explícitos debe declararse en el lado del origen:

* Para el origen de archivo, por ejemplo, CSV o Avro, la conversión de tipos se declarará a través de la estructura de origen con la lista de columnas completa (nombre de columna en el lado de origen y tipo en el lado receptor).
* Para el origen relacional (como SQL/Oracle), la conversión de tipos debe realizarse mediante el envío de tipo explícito en la instrucción de la consulta.

## <a name="when-to-specify-dataset-structure"></a>Cuándo se debe especificar el conjunto de datos "structure"

En los escenarios siguientes, se requiere el elemento "structure" en el conjunto de datos:

* Aplicación de la [conversión de tipos de datos explícitos](#explicit-data-type-conversion) para los orígenes de archivo durante la copia (conjunto de datos de entrada)
* Aplicación de la [asignación de columnas explícitas](#explicit-column-mapping) durante la copia (en el conjunto de datos tanto de entrada como de salida)
* Copia desde el origen Dynamics 365/CRM (conjunto de datos de entrada)
* Copia a Cosmos DB como objeto anidado cuando el origen no es un archivo JSON (conjunto de datos de salida)

En los escenarios siguientes, se sugiere el elemento "structure" en el conjunto de datos:

* Copia desde el archivo de texto sin encabezado (conjunto de datos de entrada). Puede especificar los nombres de columna para que el archivo de texto se alinee con las columnas receptoras correspondientes, a fin de no tener que configurar la asignación de columnas explícita.
* Copia desde almacenes de datos con esquema flexible, como Azure Table/Cosmos DB (conjunto de datos de entrada), para garantizar que los datos esperados (columnas) se copien en lugar de permitir que la actividad de copia interfiera en el esquema en función de las filas superiores durante cada ejecución de la actividad.


## <a name="next-steps"></a>Pasos siguientes
Consulte los otros artículos de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Tolerancia a errores de la actividad de copia](copy-activity-fault-tolerance.md)
- [Rendimiento de la actividad de copia](copy-activity-performance.md)
