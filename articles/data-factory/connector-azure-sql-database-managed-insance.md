---
title: Copia de datos hacia y desde Instancia administrada de Azure SQL Database mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo mover datos hacia y desde Instancia administrada de Azure SQL Database mediante Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: jingwang
ms.openlocfilehash: 3f29db5786c682188b4eadec12275df46ae3b547
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153336"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Copia de datos hacia y desde Instancia administrada de Azure SQL Database mediante Azure Data Factory

En este artículo se describe el uso de la actividad de copia de Azure Data Factory para copiar datos hacia y desde Instancia administrada de Azure SQL Database. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) en el que se describe información general de esta actividad.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde Instancia administrada de Azure SQL Database a cualquier almacén de datos receptor compatible. También puede copiar datos desde cualquier almacén de datos de origen compatible a la instancia administrada. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores.

En concreto, este conector de Instancia administrada de Azure SQL Database admite lo siguiente:

- La copia de datos con autenticación de SQL o Windows.
- Como origen, la recuperación de datos mediante una consulta SQL o un procedimiento almacenado.
- Como receptor, anexar datos a una tabla de destino o invocar un procedimiento almacenado con lógica personalizada durante la copia.

La característica [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) de SQL Server ya no se admite. 

## <a name="prerequisites"></a>Requisitos previos

Para usar los datos de copia de una Instancia administrada de Azure SQL Database que se encuentra en una red virtual, debe configurar un entorno de ejecución de integración autohospedado que pueda acceder a la base de datos. Para más información, vea [Creación y configuración de un entorno de ejecución de integración autohospedado](create-self-hosted-integration-runtime.md).

Si aprovisiona el entorno de ejecución de integración autohospedado en la misma red virtual que la instancia administrada, asegúrese de que la máquina del entorno de ejecución de integración está en una subred distinta a la de la instancia administrada. Si aprovisiona el entorno de ejecución de integración autohospedado en una red virtual distinta a la de la instancia administrada, puede usar un emparejamiento de redes virtuales o una conexión de red virtual a red virtual. Para más información, vea [Conexión de la aplicación a Instancia administrada de Azure SQL Database](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector Instancia administrada de Azure SQL Database.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades se admiten en el servicio vinculado de Instancia administrada de Azure SQL Database:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type se debe establecer en: **SqlServer**. | Sí. |
| connectionString |Esta propiedad especifica la información de connectionString necesaria para conectarse a la instancia administrada mediante autenticación de SQL o autenticación de Windows. Para más información, vea los ejemplos siguientes. <br/>Marque este campo como SecureString para almacenarlo de forma segura en Data Factory. También puede colocar la contraseña en Azure Key Vault y, en el caso de autenticación de SQL, extraer la configuración `password` de la cadena de conexión. Vea el ejemplo de JSON debajo de la tabla y el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) con información detallada. |Sí. |
| userName |Esta propiedad especifica un nombre de usuario si usa la autenticación de Windows. Un ejemplo es **domainname\\username**. | No. |
| password |Esta propiedad especifica la contraseña de la cuenta de usuario que se especificó para el nombre de usuario. Seleccione **SecureString** para almacenar la información de connectionString de forma segura en Data Factory o para [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | No. |
| connectVia | Este [entorno de ejecución de integración](concepts-integration-runtime.md) se usa para conectarse al almacén de datos. Aprovisione el entorno de ejecución de integración autohospedado en la misma red virtual que la instancia administrada. |Sí. |

>[!TIP]
>Es posible que vea el código de error "UserErrorFailedToConnectToSqlServer" con un mensaje como "Se ha alcanzado el límite de la sesión para la base de datos establecido en XXX". Si se produce este error, agregue `Pooling=false` a la cadena de conexión y vuelva a intentarlo.

**Ejemplo 1: Uso de la autenticación de SQL**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo 2: Uso de la autenticación de SQL con contraseña en Azure Key Vault**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
            },
            "password": { 
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

**Ejemplo 3: Uso de la autenticación de Windows**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;"
            },
            "userName": "<domain\\username>",
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

Si desea obtener una lista completa de secciones y propiedades disponibles para definir conjuntos de datos, vea el artículo sobre conjuntos de datos. En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Instancia administrada de Azure SQL Database.

Para copiar datos hacia y desde Azure SQL Database Managed Instance, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos se debe establecer en **SqlServerTable**. | Sí. |
| tableName |Esta propiedad es el nombre de la tabla o vista de la instancia de base de datos a la que hace referencia el servicio vinculado. | No para el origen. Sí para el receptor. |

**Ejemplo**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, vea el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades admitidas en el origen y el receptor de Instancia administrada de Azure SQL Database.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Instancia administrada de Azure SQL Database como un origen

Si va a copiar datos desde Instancia administrada de Azure SQL Database, establezca el tipo de origen de la actividad de copia en **SqlSource**. En la sección source de la actividad de copia se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en **SqlSource**. | Sí. |
| sqlReaderQuery |Esta propiedad usa la consulta SQL personalizada para leer los datos. Un ejemplo es `select * from MyTable`. | No. |
| sqlReaderStoredProcedureName |Esta propiedad es el nombre del procedimiento almacenado que lee datos de la tabla de origen. La última instrucción SQL debe ser una instrucción SELECT del procedimiento almacenado. | No. |
| storedProcedureParameters |Estos parámetros son para el procedimiento almacenado.<br/>Los valores permitidos son pares de nombre o valor. Los nombres y las mayúsculas y minúsculas de los parámetros deben coincidir con las mismas características de los parámetros de procedimiento almacenado. | No. |

Tenga en cuenta los siguientes puntos:

- Si se especifica **sqlReaderQuery** para **SqlSource**, la actividad de copia ejecuta esta consulta en el origen de Instancia administrada para obtener los datos. También puede indicar un procedimiento almacenado mediante la definición de **sqlReaderStoredProcedureName** y **storedProcedureParameters** si el procedimiento almacenado adopta parámetros.
- Si no se especifican las propiedades **sqlReaderQuery** o **sqlReaderStoredProcedureName**, se usan las columnas que se definen en la sección “structure” del JSON del conjunto de datos para construir una consulta. La consulta `select column1, column2 from mytable` se ejecuta en la instancia administrada. Si la definición del conjunto de datos no tiene la sección "structure", se seleccionan todas las columnas de la tabla.

**Ejemplo: Uso de una consulta SQL**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Ejemplo: Uso de un procedimiento almacenado**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Definición del procedimiento almacenado**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Instancia administrada de Azure SQL Database como un receptor

Para copiar datos en Instancia administrada de Azure SQL Database, establezca el tipo de receptor de la actividad de copia en **SqlSink**. La sección sink de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del receptor de la actividad de copia debe establecerse en **SqlSink**. | Sí. |
| writeBatchSize |Número de filas que se inserta en la tabla SQL **por lote**.<br/>Los valores permitidos son enteros para el número de filas. De forma predeterminada, Data Factory determinar dinámicamente el tamaño del lote adecuado en función del tamaño de fila.  |Sin  |
| writeBatchTimeout |Esta propiedad especifica el tiempo de espera para que la operación de inserción por lotes se complete antes de que se agote el tiempo de espera.<br/>Los valores permitidos son para el intervalo de tiempo. Un ejemplo es "00:30:00", que es 30 minutos. | No. |
| preCopyScript |Esta propiedad especifica una consulta SQL para que la actividad de copia se ejecute antes de escribir datos en la instancia administrada. Solo se invoca una vez por cada copia que se ejecuta. Puede usar esta propiedad para limpiar los datos cargados previamente. | No. |
| sqlWriterStoredProcedureName |Este nombre es para el procedimiento almacenado que define cómo se aplican los datos de origen en una tabla de destino. Los ejemplos de procedimientos son realizar operaciones upsert o transformaciones mediante su propia lógica de negocios. <br/><br/>Este procedimiento almacenado se *invoca por lote*. Para realizar una operación que solo se ejecuta una vez y que no tiene nada que ver con los datos de origen, como por ejemplo, eliminar o truncar, use la propiedad `preCopyScript`. | No. |
| storedProcedureParameters |Estos parámetros se usan para el procedimiento almacenado.<br/>Los valores permitidos son pares de nombre o valor. Los nombres y las mayúsculas y minúsculas de los parámetros deben coincidir con las mismas características de los parámetros de procedimiento almacenado. | No. |
| sqlWriterTableType |Esta propiedad especifica el nombre de un tipo de tabla que se usará en el procedimiento almacenado. La actividad de copia dispone que los datos que se mueven estén disponibles en una tabla temporal con este tipo de tabla. El código de procedimiento almacenado puede combinar los datos copiados con datos existentes. | No. |

> [!TIP]
> Cuando se copian datos en Instancia administrada de Azure SQL Database, la actividad de copia anexa datos a la tabla receptora de forma predeterminada. Para llevar a cabo una operación upsert o una lógica de negocios adicional, use el procedimiento almacenado de SqlSink. Para más información, vea [Invocación del procedimiento almacenado desde el receptor de SQL](#invoke-a-stored-procedure-from-a-sql-sink).

**Ejemplo 1: Anexión de datos**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Ejemplo 2: Invocación de un procedimiento almacenado durante la copia para la operación upsert**

Para más información, vea [Invocación del procedimiento almacenado desde el receptor de SQL ](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Columnas de identidad en la base de datos de destino

En el ejemplo siguiente se copian datos de una tabla de origen sin una columna de identidad en una tabla de destino con una columna de identidad.

**Tabla de origen**

```sql
create table dbo.SourceTbl
(
    name varchar(100),
    age int
)
```

**Tabla de destino**

```sql
create table dbo.TargetTbl
(
    identifier int identity(1,1),
    name varchar(100),
    age int
)
```

Observe que la tabla de destino tiene una columna de identidad.

**Definición de JSON del conjunto de datos de origen**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

**Definición de JSON del conjunto de datos de destino**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

Tenga en cuenta que las tablas de origen y de destino tienen esquemas diferentes. La tabla de destino tiene una columna de identidad. En este escenario, especifique la propiedad “structure” de la definición del conjunto de datos de destino, que no incluye la columna de identidad.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Invocación del procedimiento almacenado desde el receptor de SQL

Al copiar datos en Instancia administrada de Azure SQL Database, se puede configurar e invocar un procedimiento almacenado con parámetros adicionales especificados.

Cuando los mecanismos de copia integrados no prestan el servicio, se puede usar un procedimiento almacenado. Normalmente se utiliza cuando se debe realizar una operación upsert (actualización más inserción) o un procesamiento adicional antes de la inserción final de los datos de origen en la tabla de destino. El procesamiento adicional puede incluir tareas como la combinación de columnas, la búsqueda de valores adicionales y la inserción en varias tablas.

En el ejemplo siguiente se muestra cómo usar un procedimiento almacenado para realizar una operación UPSERT en una tabla en la base de datos SQL Server. Supongamos que los datos de entrada y la tabla **Marketing** del receptor tienen tres columnas: **ProfileID**, **State** y **Category**. Realice una operación UPSERT en función de la columna **ProfileID** y aplíquela solo a una categoría concreta.

**Conjunto de datos de salida:** "tableName" debe ser el mismo nombre de parámetro de tipo de tabla en el procedimiento almacenado (vea a continuación de la secuencia de comandos de procedimiento almacenado).

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definir la **receptor SQL** sección actividad de copia como se indica a continuación.

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

En la base de datos, defina el procedimiento almacenado con el mismo nombre que **SqlWriterStoredProcedureName**. Dicho procedimiento administra los datos de entrada del origen especificado y los combina en la tabla de salida. El nombre del parámetro del tipo de tabla en el procedimiento almacenado debe ser el mismo que el de **tableName** definido en el conjunto de datos.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category);
END
```

En la base de datos, defina el tipo de tabla con el mismo nombre que sqlWriterTableType. El esquema del tipo de tabla es el mismo que el que devuelven los datos de entrada.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

La característica de procedimiento almacenado aprovecha los [parámetros con valores de tabla](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Asignación de tipos de datos para Instancia administrada de Azure SQL Database

Al copiar datos desde y hacia Instancia administrada de Azure SQL Database, se usan las siguientes asignaciones de los tipos de datos de dicha instancia a los tipos de datos provisionales de Azure Data Factory. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para más información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipo de datos de Instancia administrada de Azure SQL Database | Tipo de datos provisionales de Azure Data Factory |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |Datetime |
| Datetime |Datetime |
| datetime2 |Datetime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |Datetime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
|  timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |Xml |

>[!NOTE]
> Para los tipos de datos que se asignan al tipo decimal provisional, Azure Data Factory actualmente admite una precisión de hasta 28. Si tiene datos que requieren una precisión mayor que 28, considere la posibilidad de convertir a una cadena en una consulta SQL.

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
