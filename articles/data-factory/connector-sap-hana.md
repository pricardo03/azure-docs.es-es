---
title: Copia de datos desde SAP HANA
description: Obtenga información sobre cómo copiar datos desde SAP HANA en almacenes de datos receptores compatibles a través de una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/02/2019
ms.openlocfilehash: 97c277eadbd1b425c50b10d15172c13e17e20eb3
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926200"
---
# <a name="copy-data-from-sap-hana-using-azure-data-factory"></a>Copia de datos de SAP HANA mediante Azure Data Factory
> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
> * [Versión 1](v1/data-factory-sap-hana-connector.md)
> * [Versión actual](connector-sap-hana.md)

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos desde una base de datos SAP HANA. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

>[!TIP]
>Para obtener información sobre la compatibilidad general de ADF con el escenario de integración de datos de SAP, consulte el [informe técnico sobre la integración de datos de SAP con Azure Data Factory](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) para ver una introducción, comparación y guía detalladas.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector SAP HANA es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos desde una base de datos SAP HANA a cualquier almacén de datos receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector SAP HANA admite las siguientes funcionalidades:

- Copiar datos de cualquier versión de la base de datos SAP HANA.
- Copiar de datos de **modelos de información de HANA** (como las vistas de análisis y cálculo) y las **tablas de fila o columna**.
- La copia de datos con autenticación **básica** o de **Windows**.

> [!TIP]
> Para copiar datos **en** un almacén de datos SAP HANA, use un conector ODBC genérico. Consulte el artículo sobre el [receptor de SAP HANA](connector-odbc.md#sap-hana-sink) con detalles. Tenga en cuenta que los servicios vinculados para el conector SAP HANA y el conector ODBC tienen tipos distintos y, por tanto, no se pueden reutilizar.

## <a name="prerequisites"></a>Requisitos previos

Para usar este conector SAP HANA, necesitará lo siguiente:

- Configurar un entorno Integration Runtime autohospedado. Consulte el artículo sobre [Integration Runtime autohospedado](create-self-hosted-integration-runtime.md) para más información.
- Instale el controlador ODBC de SAP HANA en la máquina de Integration Runtime. Puede descargar el controlador ODBC de SAP HANA desde el [centro de descarga de software de SAP](https://support.sap.com/swdc). Busque con la palabra clave **SAP HANA CLIENT for Windows** (Cliente SAP HANA para Windows).

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector SAP HANA.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado SAP HANA:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **SapHana** | Sí |
| connectionString | Especifique la información necesaria para conectarse a SAP HANA mediante la **autenticación básica** o la **autenticación de Windows**. Consulte los ejemplos siguientes.<br>En la cadena de conexión, el servidor o el puerto es obligatorio (el puerto predeterminado es 30015), y el nombre de usuario y la contraseña son obligatorios cuando se usa la autenticación básica. Para obtener configuraciones avanzadas adicionales, consulte [Propiedades de conexión ODBC de SAP HANA](<https://help.sap.com/viewer/0eec0d68141541d1b07893a39944924e/2.0.02/en-US/7cab593774474f2f8db335710b2f5c50.html>)<br/>También puede colocar la contraseña en Azure Key Vault y extraer la configuración de contraseña de la cadena de conexión. Consulte el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) para obtener información detallada. | Sí |
| userName | Especifique el nombre de usuario cuando use la autenticación de Windows. Ejemplo: `user@domain.com` | Sin |
| password | Especifique la contraseña para la cuenta de usuario. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sin |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Tal y como se mencionó en los [requisitos previos](#prerequisites), se requiere un entorno Integration Runtime autohospedado. |Sí |

**Ejemplo: use la autenticación básica**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "connectionString": "SERVERNODE=<server>:<port (optional)>;UID=<userName>;PWD=<Password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo: use la autenticación de Windows**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "connectionString": "SERVERNODE=<server>:<port (optional)>;",
            "userName": "<username>", 
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

Si usaba el servicio SAP HANA vinculado a la siguiente carga, todavía se admite tal cual, aunque se aconseja usar la nueva en el futuro.

**Ejemplo:**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server>:<port (optional)>",
            "authenticationType": "Basic",
            "userName": "<username>",
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de SAP HANA.

Para copiar datos de una tabla de SAP HANA, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **SapHanaTable** | Sí |
| schema | Nombre del esquema de la base de datos de SAP HANA. | No (si se especifica "query" en el origen de la actividad) |
| table | Nombre de la tabla de la base de datos de SAP HANA. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo:**

```json
{
    "name": "SAPHANADataset",
    "properties": {
        "type": "SapHanaTable",
        "typeProperties": {
            "schema": "<schema name>",
            "table": "<table name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP HANA linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Si estaba usando un conjunto de datos de tipo `RelationalTable`, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de SAP HANA.

### <a name="sap-hana-as-source"></a>SAP HANA como origen

Para copiar datos desde SAP HANA, en la sección **source** de la actividad de copia se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **SapHanaSource** | Sí |
| query | Especifica la consulta SQL para leer datos de la instancia de SAP HANA. | Sí |
| packetSize | Especifica el tamaño del paquete de red (en kilobytes) para dividir los datos en varios bloques. Si tiene una gran cantidad de datos para copiar, si aumenta el tamaño de los paquetes aumentará a su vez la velocidad de lectura de SAP HANA en la mayoría de los casos. Se recomienda realizar pruebas de rendimiento al ajustar el tamaño de los paquetes. | No.<br>El valor predeterminado es 2048 (2 MB). |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromSAPHANA",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP HANA input dataset name>",
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
                "type": "SapHanaSource",
                "query": "<SQL query for SAP HANA>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Si estaba usando un origen de copia de tipo `RelationalSource`, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

## <a name="data-type-mapping-for-sap-hana"></a>Asignación de tipos de datos para SAP HANA

Al copiar datos desde SAP HANA, se usan las siguientes asignaciones de tipos de datos de SAP HANA en los tipos de datos provisionales de Azure Data Factory. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipo de datos de SAP HANA | Tipo de datos provisionales de Data Factory |
| ------------------ | ------------------------------ |
| ALPHANUM           | Cadena                         |
| BIGINT             | Int64                          |
| BINARY             | Byte[]                         |
| BINTEXT            | Cadena                         |
| BLOB               | Byte[]                         |
| BOOL               | Byte                           |
| CLOB               | Cadena                         |
| DATE               | DateTime                       |
| DECIMAL            | Decimal                        |
| DOUBLE             | DOUBLE                         |
| FLOAT              | Double                         |
| INTEGER            | Int32                          |
| NCLOB              | Cadena                         |
| NVARCHAR           | Cadena                         |
| REAL               | Single                         |
| SECONDDATE         | DateTime                       |
| SHORTTEXT          | Cadena                         |
| SMALLDECIMAL       | Decimal                        |
| SMALLINT           | Int16                          |
| STGEOMETRYTYPE     | Byte[]                         |
| STPOINTTYPE        | Byte[]                         |
| TEXT               | Cadena                         |
| TIME               | TimeSpan                       |
| TINYINT            | Byte                           |
| VARCHAR            | Cadena                         |
| TIMESTAMP          | DateTime                       |
| VARBINARY          | Byte[]                         |

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
