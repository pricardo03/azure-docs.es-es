---
title: Copia de datos con una instancia de Azure SQL Database como origen o destino mediante Azure Data Factory | Microsoft Docs
description: Aprenda a copiar datos desde cualquier almacén de datos de origen compatible a Azure SQL Database, o bien desde SQL Database a cualquier almacén de datos receptor compatible mediante Data Factory.
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
ms.date: 05/05/2018
ms.author: jingwang
ms.openlocfilehash: e5ecd3ab5133150368be935d8208a3e93a713df3
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435835"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Copia de datos con una instancia de Azure SQL Database como origen o destino mediante Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you use:"]
> * [Versión 1](v1/data-factory-azure-sql-connector.md)
> * [Versión actual](connector-azure-sql-database.md)

En este artículo se explica el uso de la actividad de copia en Azure Data Factory para copiar datos desde Azure SQL Database y en este servicio. Se basa en el artículo [Actividad de copia en Azure Data Factory](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos de Azure SQL Database a cualquier almacén de datos receptor compatible, y viceversa. También puede copiar datos desde cualquier almacén de datos de origen compatible a Azure SQL Database. En la tabla de [formatos y almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) puede ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector de Azure SQL Database admite estas funciones:

- Copiar datos mediante la autenticación SQL y la autenticación de tokens de aplicaciones de Azure Active Directory (Azure AD) con una entidad de servicio o Managed Service Identity (MSI).
- Como origen, la recuperación de datos mediante una consulta SQL o un procedimiento almacenado.
- Como receptor, la anexión de datos a una tabla de destino o la invocación de un procedimiento almacenado con lógica personalizada durante la copia.

> [!IMPORTANT]
> Si copia los datos mediante Azure Data Factory Integration Runtime, configure un [firewall de Azure SQL Server ](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) para que los servicios de Azure puedan acceder al servidor.
> Si copia los datos mediante un IR autohospedado, configure el firewall del servidor de Azure SQL para permitir el intervalo IP apropiado. Dicho intervalo incluye la dirección IP del equipo que se utiliza para conectarse a Azure SQL Database.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporciona información acerca de las propiedades que se usan para definir entidades de Data Factory específicas de un conector de Azure SQL Database.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Estas propiedades son compatibles con un servicio vinculado de Azure SQL Database:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** debe establecerse en **AzureSqlDatabase**. | SÍ |
| connectionString | Especifique la información necesaria para conectarse a la instancia de Azure SQL Database para la propiedad **connectionString**. Marque este campo como [SecureString](store-credentials-in-key-vault.md) para almacenarlo de forma segura en Data Factory, o bien **para hacer referencia a un secreto almacenado en Azure Key Vault**. | SÍ |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio. |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como [SecureString](store-credentials-in-key-vault.md) para almacenarlo de forma segura en Data Factory, o bien **para hacer referencia a un secreto almacenado en Azure Key Vault**. | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio. |
| tenant | Especifique la información del inquilino (nombre de dominio o identificador de inquilino) en el que reside la aplicación. Para recuperarla, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio. |
| connectVia | El [entorno de ejecución de integración](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Si el almacén de datos se encuentra en una red privada, se puede usar Azure Integration Runtime o un IR autohospedado. Si no se especifica, se usará Azure Integration Runtime. | Sin  |

Para ver los distintos tipos de autenticación, consulte las secciones siguientes acerca de requisitos previos y ejemplos de JSON, respectivamente:

- [Autenticación de SQL](#sql-authentication)
- [Autenticación de tokens de aplicaciones de Azure AD: entidad de servicio](#service-principal-authentication)
- [Autenticación de tokens de aplicaciones de Azure AD: Managed Service Identity](#managed-service-identity-authentication)

### <a name="sql-authentication"></a>Autenticación de SQL

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Ejemplo de servicio vinculado que usa la autenticación de SQL

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Autenticación de entidad de servicio

Para usar la autenticación de token de aplicación de Azure AD basada en la entidad de servicio, realice estos pasos:

1. **[Cree una aplicación de Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)** desde Azure Portal. Anote el nombre de la aplicación y los siguientes valores, que definen el servicio vinculado:

    - Identificador de aplicación
    - Clave de la aplicación
    - Id. de inquilino

1. **[Aprovisione un administrador de Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** para el servidor Azure SQL Server desde Azure Portal, en caso de que aún no lo haya hecho. El administrador de Azure AD debe ser un usuario de Azure AD o un grupo de Azure AD, pero no puede ser una entidad de servicio. Este paso se realiza con el fin de que, en el siguiente paso, pueda usar una identidad de Azure AD para crear un usuario de base de datos independiente para la entidad de servicio.

1. **[Cree usuarios de bases de datos independientes](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** para la entidad de servicio. Conéctese a la base de datos de la que desea copiar datos (o a la que desea copiarlos) mediante alguna herramienta como SSMS, con una identidad de Azure AD que tenga al menos permiso para modificar cualquier usuario. Ejecute el siguiente T-SQL: 
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

1. **Conceda a la entidad de servicio los permisos necesarios**, tal como lo haría normalmente para los usuarios de SQL, u otros usuarios. Ejecute el código siguiente:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

1. En Azure Data Factory, **configure un servicio vinculado de Azure SQL Database**.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Ejemplo de servicio vinculado que usa la autenticación de entidad de servicio

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-service-identity-authentication"></a>Autenticación de Managed Service Identity

Una factoría de datos se puede asociarse con una instancia de [Managed Service Identity](data-factory-service-identity.md) que representa esa factoría de datos concreta. Esta identidad de servicio se puede usar para Azure SQL Database. Con esta identidad la fábrica designada y puede acceder y copiar datos de su base de datos o en ella.

Para usar la autenticación de token de aplicación de Azure AD basada en MSI, siga estos pasos:

1. **Cree a grupo en Azure AD.** Convierta al MSI de la fábrica en miembro del grupo.

    a. Busque la identidad de servicio de la factoría de datos en Azure Portal. Vaya a las **propiedades** de la factoría de datos. Copie el valor de Id. de la identidad de servicio.

    b. Instalación del módulo de [PowerShell de Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2). Inicie sesión mediante el comando `Connect-AzureAD`. Ejecute los comandos siguientes para crear un grupo y agregue el MSI de la factoría de datos como miembro.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

1. **[Aprovisione un administrador de Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** para el servidor Azure SQL Server desde Azure Portal, en caso de que aún no lo haya hecho. El administrador de Azure AD puede ser un usuario de Azure AD o un grupo de Azure AD. Si concede al grupo con MSI el rol de administrador, omita los pasos 3 y 4. El administrador tendrá acceso total a la base de datos.

1. **[Cree usuarios de bases de datos independientes](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** para el grupo de Azure AD. Conéctese a la base de datos de la que desea copiar datos (o a la que desea copiarlos) mediante alguna herramienta como SSMS, con una identidad de Azure AD que tenga al menos permiso para modificar cualquier usuario. Ejecute el siguiente T-SQL: 
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

1. **Conceda al grupo de Azure AD los permisos necesarios**, tal como lo haría normalmente para los usuarios de SQL y otros usuarios. Por ejemplo, ejecute el siguiente código:

    ```sql
    EXEC sp_addrolemember [role name], [your AAD group name];
    ```

1. En Azure Data Factory, **configure un servicio vinculado de Azure SQL Database**.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>Ejemplo de servicio vinculado que usa la autenticación de MSI

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Azure SQL Database.

Para copiar datos con una instancia de Azure SQL Database como origen o destino, establezca la propiedad **type** del conjunto de datos en **AzureSqlTable**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del conjunto de datos debe establecerse en **AzureSqlTable**. | SÍ |
| tableName | Nombre de la tabla o vista de la instancia de Azure SQL Database a la que hace referencia el servicio vinculado. | SÍ |

#### <a name="dataset-properties-example"></a>Ejemplo de propiedades de un conjunto de datos

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que el origen y el receptor de Azure SQL Database admiten.

### <a name="azure-sql-database-as-the-source"></a>Azure SQL Database como origen

Para copiar datos desde Azure SQL Database (o en dicho servicio), establezca la propiedad **type** del origen de la actividad de copia en **SqlSource**. La sección **source** de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del origen de la actividad de copia debe establecerse en **SqlSource**. | SÍ |
| SqlReaderQuery | Use la consulta SQL personalizada para leer los datos. Ejemplo: `select * from MyTable`. | Sin  |
| sqlReaderStoredProcedureName | Nombre del procedimiento almacenado que lee datos de la tabla de origen. La última instrucción SQL debe ser una instrucción SELECT del procedimiento almacenado. | Sin  |
| storedProcedureParameters | Parámetros del procedimiento almacenado.<br/>Los valores permitidos son pares de nombre o valor. Los nombres y las mayúsculas y minúsculas de los parámetros deben coincidir con las mismas características de los parámetros de procedimiento almacenado. | Sin  |

### <a name="points-to-note"></a>Puntos que se deben tener en cuenta

- Si se especifica **sqlReaderQuery** en **SqlSource**, la actividad de copia ejecuta la consulta en el origen de Azure SQL Database para obtener los datos. O bien se puede especificar un procedimiento almacenado. Especifique **sqlReaderStoredProcedureName** y **storedProcedureParameters** si el procedimiento almacenado usa parámetros.
- Si no se especifican **sqlReaderQuery** ni **sqlReaderStoredProcedureName**, se usan las columnas que se definen en la sección **structure** del JSON del conjunto de datos para construir una consulta. `select column1, column2 from mytable` se ejecuta en Azure SQL Database. Si la definición del conjunto de datos no tiene la sección **structure**, se seleccionan todas las columnas de la tabla.
- Cuando use **sqlReaderStoredProcedureName**, necesitará especificar un valor para la propiedad ficticia **tableName** del JSON del conjunto de datos.

#### <a name="sql-query-example"></a>Ejemplo de consulta SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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

#### <a name="stored-procedure-example"></a>Ejemplo de procedimiento almacenado

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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

### <a name="stored-procedure-definition"></a>Definición del procedimiento almacenado

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

### <a name="azure-sql-database-as-the-sink"></a>Azure SQL Database como receptor

Para copiar los datos a Azure SQL Database, establezca la propiedad **type** del receptor de la actividad de copia en **SqlSink**. La sección **sink** de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del receptor de la actividad de copia debe establecerse en **SqlSink**. | SÍ |
| writeBatchSize | Inserta datos en la tabla SQL cuando el tamaño del búfer alcanza el valor de **writeBatchSize**.<br/> El valor que se permite es un **entero** (número de filas). | No. El valor predeterminado es 10 000. |
| writeBatchTimeout | Tiempo que se concede a la operación de inserción por lotes para que finalice antes de que se agote el tiempo de espera.<br/> El valor permitido es **intervalo de tiempo**. Ejemplo: "00:30:00" (30 minutos). | Sin  |
| preCopyScript | Especifique una consulta SQL para que la actividad de copia se ejecute antes de escribir datos en Azure SQL Database. Solo se invoca una vez por cada copia que se ejecute. Esta propiedad se usa para limpiar los datos cargados previamente. | Sin  |
| sqlWriterStoredProcedureName | El nombre del procedimiento almacenado que define cómo se aplican los datos de origen en una tabla de destino. Un ejemplo es realizar operaciones upsert o transformaciones mediante su propia lógica de negocios. <br/><br/>Este procedimiento almacenado se **invoca por lote**. En las operaciones que se ejecutan solo una vez y no tienen nada que ver con los datos de origen, use la propiedad `preCopyScript`. Algunas operaciones de ejemplo son eliminar y truncar. | Sin  |
| storedProcedureParameters |Parámetros del procedimiento almacenado.<br/>Los valores permitidos son pares de nombre y valor. Los nombres y las mayúsculas y minúsculas de los parámetros deben coincidir con las mismas características de los parámetros de procedimiento almacenado. | Sin  |
| sqlWriterTableType | Especifique el nombre del tipo de tabla que se usará en el procedimiento almacenado anterior. La actividad de copia permite que se usen los datos que se mueven, ya que los coloca en una tabla temporal con este tipo de tabla. El código de procedimiento almacenado puede combinar los datos copiados con datos existentes. | Sin  |

> [!TIP]
> Cuando se copian datos en Azure SQL Database, la actividad de copia los anexa de forma predeterminada a la tabla del receptor. Para llevar a cabo una operación upsert o una lógica de negocios adicional, use el procedimiento almacenado de **SqlSink**. Para más información, consulte [Invocación del procedimiento almacenado desde el receptor de SQL](#invoking-stored-procedure-for-sql-sink).

#### <a name="append-data-example"></a>Anexión de datos de ejemplo

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
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

#### <a name="invoke-a-stored-procedure-during-copy-for-upsert-example"></a>Invocación de un procedimiento almacenado durante la copia para la operación upsert de ejemplo

Para más información, consulte [Invocación del procedimiento almacenado desde el receptor de SQL](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
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

En esta sección se muestra cómo copiar datos de una tabla de origen sin una columna de identidad en una tabla de destino con una columna de identidad.

#### <a name="source-table"></a>Tabla de origen

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

#### <a name="destination-table"></a>Tabla de destino

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

> [!NOTE]
> La tabla de destino tiene una columna de identidad.

#### <a name="source-dataset-json-definition"></a>Definición de JSON del conjunto de datos de origen

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " AzureSqlTable",
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

#### <a name="destination-dataset-json-definition"></a>Definición de JSON del conjunto de datos de destino

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
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

> [!NOTE]
> Las tablas de origen y de destino tienen esquemas diferentes. 

El destino tiene una columna adicional con una identidad. En este escenario, debe especificar la propiedad **structure** de la definición del conjunto de datos de destino, que no incluye la columna de identidad.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Invocación del procedimiento almacenado desde el receptor de SQL

Al copiar datos en Azure SQL Database, también se puede configurar e invocar un procedimiento almacenado especificado por el usuario con parámetros adicionales.

Cuando los mecanismos de copia integrados no prestan el servicio, se puede usar un procedimiento almacenado. Normalmente se utiliza cuando se debe realizar una operación de actualización más inserción o un procedimiento adicional antes de la inserción final de datos de origen en la tabla de destino. Algunos ejemplos de procesamiento adicional son las columnas de combinación, la búsqueda de valores adicionales y la inserción en más de una tabla.

En el ejemplo siguiente se muestra cómo usar un procedimiento almacenado para realizar una operación upsert en una tabla de Azure SQL Database. Supongamos que los datos de entrada y la tabla **Marketing** del receptor tienen tres columnas: **ProfileID**, **State** y **Category**. Realice una operación UPSERT en función de la columna **ProfileID** y aplíquela solo a una categoría concreta.

#### <a name="output-dataset"></a>Conjunto de datos de salida

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Defina la sección **SqlSink** de la actividad de copia:

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

En la base de datos, defina el procedimiento almacenado con el mismo nombre que **SqlWriterStoredProcedureName**. Dicho procedimiento administra los datos de entrada del origen especificado y los combina en la tabla de salida. El nombre del parámetro del procedimiento almacenado debe ser el mismo que el valor de **tableName** definido en el conjunto de datos.

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
      VALUES (source.ProfileID, source.State, source.Category)
END
```

En la base de datos, defina el tipo de tabla con el mismo nombre que **sqlWriterTableType**. El esquema del tipo de tabla debe ser el mismo que el que devuelven los datos de entrada.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL,
)
```

La característica de procedimiento almacenado aprovecha los [parámetros con valores de tabla](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>Asignación de tipos de Azure SQL Database

Al copiar datos con Azure SQL Database como origen o destino, se utilizan las siguientes asignaciones de tipos de datos de Azure SQL Database en los tipos de datos provisionales de Azure Data Factory. Para obtener información acerca de la forma en que la actividad de copia asigna el esquema de origen y el tipo de datos al receptor, consulte el artículo acerca de [asignaciones de esquemas y tipos de datos](copy-activity-schema-and-type-mapping.md).

| Tipo de datos de Azure SQL Database | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |boolean |
| char |String, Char[] |
| fecha |Datetime |
| DateTime |Datetime |
| datetime2 |Datetime |
| Datetimeoffset |DateTimeOffset |
| DECIMAL |DECIMAL |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Doble |
| imagen |Byte[] |
| int |Int32 |
| money |DECIMAL |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |DECIMAL |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |Datetime |
| smallint |Int16 |
| smallmoney |DECIMAL |
| sql_variant |Object * |
| text |String, Char[] |
| Twitter en tiempo |timespan |
| timestamp |Byte[] |
| tinyint |Byte |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |xml |

## <a name="next-steps"></a>Pasos siguientes
Consulte los [formatos y almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver una lista de los almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
