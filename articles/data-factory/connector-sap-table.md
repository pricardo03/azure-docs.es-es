---
title: Copiar datos de tabla de SAP con Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos de la tabla SAP en almacenes de datos receptores compatibles mediante el uso de una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: jingwang
ms.openlocfilehash: 4dee0e994c9e7be9677a8f1051481850990998e9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247175"
---
# <a name="copy-data-from-sap-table-using-azure-data-factory"></a>Copiar datos de tabla de SAP con Azure Data Factory

En este artículo se describe cómo usar la actividad de copia en Azure Data Factory para copiar datos de una tabla de SAP. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos de la tabla SAP a cualquier almacén de datos receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector de tabla SAP admite:

- Copiar datos de tabla de SAP en **SAP Business Suite con versión 7.01 o superior** (en una reciente SAP compatibilidad con paquete pila publicada después del año 2015) o **S/4HANA**.
- Copiar datos desde ambos **tabla transparente SAP** y **vista**.
- Copiar datos mediante **autenticación básica** o **SNC** (proteger las comunicaciones de red) si se configura SNC.
- Conectarse a **servidor de aplicaciones** o **Message Server**.

## <a name="prerequisites"></a>Requisitos previos

Para usar este conector de tabla de SAP, necesita:

- Configurar un entorno Integration Runtime autohospedado con la versión 3.17 o superior. Consulte el artículo sobre [Integration Runtime autohospedado](create-self-hosted-integration-runtime.md) para más información.

- Descargar el **[conector de SAP para .NET 3.0](https://support.sap.com/en/product/connectors/msnet.html) de 64 bits** del sitio web de SAP e instalarlo en la máquina del IR autohospedado. Al instalar, en la ventana de pasos de configuración opcionales, asegúrese de seleccionar el **instalar ensamblados en GAC** opción. 

    ![Instalación del conector de SAP para .NET](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Usuario SAP que se usa en el conector de tabla de SAP de factoría de datos debe tener los siguientes permisos: 

    - Autorización de RFC. 
    - Permisos para la actividad "Ejecutar" del objeto de autorización "S_SDSAUTH".

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector de tabla SAP.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Open Hub para SAP Business Warehouse:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **SapTable** | Sí |
| Servidor | Nombre del servidor en el que reside la instancia de SAP.<br/>Aplicable si desea conectarse a **SAP Application Server**. | Sin  |
| systemNumber | Número del sistema del sistema SAP.<br/>Aplicable si desea conectarse a **SAP Application Server**.<br/>Valor permitido: número decimal de dos dígitos que se representa en forma de cadena. | Sin  |
| messageServer | El nombre de host del servidor de mensajes de SAP.<br/>Aplicable si desea conectarse a **SAP Message Server**. | Sin  |
| messageServerService | El nombre del servicio o el puerto número del mensaje de servidor.<br/>Aplicable si desea conectarse a **SAP Message Server**. | Sin  |
| systemId | SystemID del sistema SAP que se encuentra la tabla.<br/>Aplicable si desea conectarse a **SAP Message Server**. | No |
| logonGroup | El grupo de inicio de sesión para el sistema SAP.<br/>Aplicable si desea conectarse a **SAP Message Server**. | Sin  |
| clientId | Id. de cliente del cliente en el sistema SAP.<br/>Valor permitido: número decimal de tres dígitos que se representa en forma de cadena. | Sí |
| language | Idioma que utiliza el sistema SAP. | No (el valor predeterminado es **EN**)|
| userName | Nombre del usuario que tiene acceso al servidor SAP. | Sí |
| password | Contraseña del usuario. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| sncMode | Indicador de activación de SNC para acceder al servidor SAP que se encuentra la tabla.<br/>Se aplica si desea usar SNC para conectarse al servidor SAP.<br/>Los valores permitidos son: **0** (desactivado, predeterminado) o **1** (activado). | Sin  |
| sncMyName | Nombre SNC del iniciador para acceder al servidor SAP que se encuentra la tabla.<br/>Se aplica cuando `sncMode` está activado. | No |
| sncPartnerName | Nombre SNC del asociado de comunicación para acceder al servidor SAP que se encuentra la tabla.<br/>Se aplica cuando `sncMode` está activado. | Sin  |
| sncLibraryPath | Biblioteca del producto de seguridad externas para acceder al servidor SAP donde se encuentra la tabla.<br/>Se aplica cuando `sncMode` está activado. | Sin  |
| sncQop | Calidad SNC de protección.<br/>Se aplica cuando `sncMode` está activado. <br/>Los valores permitidos son: **1** (autenticación), **2** (integridad), **3** (privacidad), **8** (predeterminado), **9** (máximo). | Sin  |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Tal y como se mencionó en los [requisitos previos](#prerequisites), se requiere un entorno Integration Runtime autohospedado. |Sí |

**Ejemplo 1: conectarse al servidor de aplicaciones de SAP**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo 2: conectar con el servidor de mensajes de SAP**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "messageServer": "<message server name>",
            "messageServerService": "<service name or port>",
            "systemId": "<system id>",
            "logonGroup": "<logon group>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo 3: conectar mediante SNC**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            },
            "sncMode": 1,
            "sncMyName": "snc myname",
            "sncPartnerName": "snc partner name",
            "sncLibraryPath": "snc library path",
            "sncQop": "8"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de la tabla SAP.

Para copiar datos desde y hacia el concentrador abierto de SAP BW, se admiten las siguientes propiedades.

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type debe establecerse en **SapTableResource**. | Sí |
| tableName | El nombre de la tabla para copiar datos de SAP. | Sí |

**Ejemplo:**

```json
{
    "name": "SAPTableDataset",
    "properties": {
        "type": "SapTableResource",
        "linkedServiceName": {
            "referenceName": "<SAP Table linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<SAP table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de propiedades admitidas por el origen de la tabla SAP.

### <a name="sap-table-as-source"></a>Tabla SAP como origen

Para copiar datos desde la tabla SAP, se admiten las siguientes propiedades.

| Propiedad                         | DESCRIPCIÓN                                                  | Obligatorio |
| :------------------------------- | :----------------------------------------------------------- | :------- |
| Tipo                             | La propiedad type debe establecerse en **SapTableSource**.       | Sí      |
| rowCount                         | Número de filas que se van a recuperar.                              | Sin        |
| rfcTableFields                   | Campos que se copiarán de la tabla SAP. Por ejemplo, `column0, column1`. | No       |
| rfcTableOptions                  | Opciones para filtrar las filas de tabla de SAP. Por ejemplo, `COLUMN0 EQ 'SOMEVALUE'`. | No       |
| customRfcReadTableFunctionModule | Módulo de función RFC personalizado que puede usarse para leer datos de la tabla SAP. | Sin        |
| partitionOption                  | El mecanismo de partición para leer de la tabla SAP. Las opciones admitidas incluyen: <br/>- **Ninguno**<br/>- **PartitionOnInt** (entero normal o valores enteros con ceros a la izquierda, como 0000012345 de relleno)<br/>- **PartitionOnCalendarYear** (4 dígitos en formato "YYYY")<br/>- **PartitionOnCalendarMonth** (6 dígitos en formato "AAAAMM")<br/>- **PartitionOnCalendarDate** (8 dígitos en formato "AAAAMMDD") | Sin        |
| partitionColumnName              | El nombre de la columna para particionar los datos. | Sin        |
| partitionUpperBound              | El valor máximo de la columna especificada en `partitionColumnName` que se usará para crear particiones de continuar. | Sin        |
| partitionLowerBound              | El valor mínimo de la columna especificada en `partitionColumnName` que se usará para crear particiones de continuar. | No       |
| maxPartitionsNumber              | El número máximo de particiones para dividir los datos. | Sin        |

>[!TIP]
>- Si la tabla SAP tiene gran volumen de datos, como varios miles de millones de filas, use `partitionOption` y `partitionSetting` para dividir los datos en particiones pequeñas, en cuyo caso los datos se leen las particiones y cada partición de datos se recupera desde el servidor SAP a través de una única Llamada RFC.<br/>
>- Teniendo `partitionOption` como `partitionOnInt` como por ejemplo, se calcula el número de filas de cada partición (número total de filas comprendidas entre *partitionUpperBound* y *partitionLowerBound*) /*maxPartitionsNumber*.<br/>
>- Si desea ejecutar más particiones en paralelo para acelerar la copia, se recomienda encarecidamente realizar `maxPartitionsNumber` como un múltiplo del valor de `parallelCopies` (Obtenga más información en [copia en paralelo](copy-activity-performance.md#parallel-copy)).

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromSAPTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP Table input dataset name>",
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
                "type": "SapTableSource",
                "partitionOption": "PartitionOnInt",
                "partitionSettings": {
                     "partitionColumnName": "<partition column name>",
                     "partitionUpperBound": "2000",
                     "partitionLowerBound": "1",
                     "maxPartitionsNumber": 500
                 }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-table"></a>Asignación de tipos de datos para la tabla SAP

Al copiar datos desde la tabla SAP, se usan las siguientes asignaciones de tipos de datos de la tabla SAP para tipos de datos provisionales de Azure Data Factory. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipo ABAP para SAP | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| C (cadena) | String |
| Me (entero) | Int32 |
| F (flotante) | Double |
| D (fecha) | string |
| T (hora) | string |
| P (BCD empaquetado, moneda, decimal, cantidad) | Decimal |
| N (numérico) | String |
| X (binario y sin procesar) | string |

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
