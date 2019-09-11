---
title: Copia de datos de Google BigQuery con Azure Data Factory | Microsoft Docs
description: Aprenda cómo copiar datos de Google BigQuery en almacenes de datos receptores compatibles mediante una actividad de copia en una canalización de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: jingwang
ms.openlocfilehash: 61c10055a7f85f849fc366211eb41382c4c3039b
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70275195"
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory"></a>Copia de datos de Google BigQuery con Azure Data Factory

En este artículo se describe el uso de la actividad de copia de Azure Data Factory para copiar datos de Google BigQuery. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que presenta información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos de Google BigQuery en cualquier almacén de datos de receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

Data Factory proporciona un controlador integrado para permitir la conectividad. Por lo tanto, no es necesario instalar manualmente uno para usar este conector.

>[!NOTE]
>Este conector de Google BigQuery se basa en las API de BigQuery. Tenga en cuenta que BigQuery limita la velocidad máxima de las solicitudes entrantes e impone cuotas adecuadas según cada proyecto; consulte las [cuotas y límites de las solicitudes de API](https://cloud.google.com/bigquery/quotas#api_requests) para más información. Asegúrese de no desencadenar demasiadas solicitudes simultáneas a la cuenta.

## <a name="get-started"></a>Primeros pasos

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector Google BigQuery.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Google BigQuery:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type debe establecerse en **GoogleBigQuery**. | Sí |
| proyecto | Identificador del proyecto predeterminado de BigQuery para el que se realizarán consultas.  | Sí |
| additionalProjects | Lista separada por comas de identificadores de proyectos públicos de BigQuery para su acceso.  | Sin |
| requestGoogleDriveScope | Si desea solicitar acceso a Google Drive. Al permitir el acceso a Google Drive, se habilita la compatibilidad para las tablas federadas que combinan datos de BigQuery con datos de Google Drive. El valor predeterminado es **false**.  | Sin |
| authenticationType | Mecanismo de autenticación OAuth 2.0 que se usa para autenticar. ServiceAuthentication solo puede usarse en Integration Runtime autohospedado. <br/>Los valores permitidos son: **UserAuthentication** y **ServiceAuthentication**. Consulte en las secciones después de esta tabla más propiedades y ejemplos de JSON para esos tipos de autenticación respectivamente. | Sí |

### <a name="using-user-authentication"></a>Uso de la autenticación de usuarios

Establezca la propiedad "authenticationType" en **UserAuthentication** y especifique las siguientes propiedades junto con las propiedades genéricas descritas en la sección anterior:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| clientId | Identificador de la aplicación usada para generar el token de actualización. | Sin |
| clientSecret | Secreto de la aplicación usado para generar el token de actualización. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sin |
| refreshToken | El token de actualización obtenido de Google usado para autorizar el acceso a BigQuery. Aprenda cómo obtener uno en [Obtaining OAuth 2.0 access tokens](https://developers.google.com/identity/protocols/OAuth2WebServer#obtainingaccesstokens) (Obtención de tokens de acceso de OAuth 2.0) y este [blog de la comunidad](https://jpd.ms/getting-your-bigquery-refresh-token-for-azure-datafactory-f884ff815a59). Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sin |

**Ejemplo:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project ID>",
            "additionalProjects" : "<additional project IDs>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "clientId": "<id of the application used to generate the refresh token>",
            "clientSecret": {
                "type": "SecureString",
                "value":"<secret of the application used to generate the refresh token>"
            },
            "refreshToken": {
                "type": "SecureString",
                "value": "<refresh token>"
            }
        }
    }
}
```

### <a name="using-service-authentication"></a>Uso de la autenticación de servicio

Establezca la propiedad "authenticationType" en **ServiceAuthentication** y especifique las siguientes propiedades junto con las propiedades genéricas descritas en la sección anterior. Este tipo de autenticación solo puede usarse en Integration Runtime autohospedado.

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| email | El identificador de correo electrónico de la cuenta de servicio que se usa para ServiceAuthentication. Solo se puede usar en Integration Runtime autohospedado.  | Sin |
| keyFilePath | La ruta de acceso completa al archivo de clave. p12 que se usa para autenticar la dirección de correo electrónico de la cuenta de servicio. | Sin |
| trustedCertPath | La ruta de acceso completa del archivo .pem que contiene certificados de entidad de certificación de confianza usados para comprobar el servidor cuando se conecta a través de SSL. Esta propiedad solo puede establecerse cuando se usa SSL en Integration Runtime autohospedado. El valor predeterminado es el archivo cacerts.pem instalado con Integration Runtime.  | Sin |
| useSystemTrustStore | Especifica si se usa un certificado de entidad de certificación del almacén de confianza del sistema o de un archivo .pem especificado. El valor predeterminado es **false**.  | Sin |

**Ejemplo:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project id>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "ServiceAuthentication",
            "email": "<email>",
            "keyFilePath": "<.p12 key path on the IR machine>"
        },
        "connectVia": {
            "referenceName": "<name of Self-hosted Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades compatibles con el conjunto de datos de Google BigQuery.

Para copiar datos de Google BigQuery, establezca la propiedad type del conjunto de datos en **GoogleBigQueryObject**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **GoogleBigQueryObject** | Sí |
| dataset | Nombre del conjunto de datos de Google BigQuery. |No (si se especifica "query" en el origen de la actividad)  |
| table | Nombre de la tabla. |No (si se especifica "query" en el origen de la actividad)  |
| tableName | Nombre de la tabla. Esta propiedad permite la compatibilidad con versiones anteriores. Para la carga de trabajo nueva, use `dataset` y `table`. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el tipo de origen de Google BigQuery.

### <a name="googlebigquerysource-as-a-source-type"></a>GoogleBigQuerySource como tipo de origen

Para copiar datos de Google BigQuery, establezca el tipo de origen de la actividad de copia en **GoogleBigQuerySource**. En la sección **source** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del origen de la actividad de copia debe establecerse en **GoogleBigQuerySource**. | Sí |
| query | Use la consulta SQL personalizada para leer los datos. Un ejemplo es `"SELECT * FROM MyTable"`. | No (si se especifica "tableName" en el conjunto de datos) |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
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
                "type": "GoogleBigQuerySource",
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
Para ver la lista de almacenes de datos que la actividad de copia de Data Factory admite como orígenes y receptores consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
