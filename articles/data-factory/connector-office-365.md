---
title: Copiar datos de Office 365 con Azure Data Factory (versión preliminar) | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde Office 365 a almacenes de datos receptores compatibles mediante una actividad de copia de una canalización de Azure Data Factory.
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
ms.date: 10/22/2018
ms.author: jingwang
ms.openlocfilehash: f76c1676e21e1abdc3f23e2e2c4a7f6f721fefdb
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53386577"
---
# <a name="copy-data-from-office-365-into-azure-using-azure-data-factory-preview"></a>Copiar datos de Office 365 en Azure con Azure Data Factory (versión preliminar) 

Azure Data Factory permite incluir los datos importantes de la organización en el inquilino de Office 365 en Azure de una manera escalable, además de crear aplicaciones de análisis y extraer información en función de estos importantes recursos de datos. La integración con Privileged Access Management proporciona control de acceso seguro para los datos protegidos valiosos en Office 365.  Para más información sobre la conexión de datos de Microsoft Graph, consulte [este vínculo](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki).

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos de Office 365. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Por ahora, en una única actividad de copia, solo puede **copiar datos de Office 365 en [Azure Blob Storage](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) y [Azure Data Lake Storage Gen2 (versión preliminar)](connector-azure-data-lake-storage.md) en formato JSON** (tipo setOfObjects). Si desea cargar Office 365 en otros tipos de almacenes de datos o en otros formatos, puede encadenar la primera actividad de copia con una actividad de copia posterior para cargar más datos en cualquiera de los [almacenes de destino de ADF admitidos](copy-activity-overview.md#supported-data-stores-and-formats); consulte la columna "Se admite como receptor" de la tabla "Almacenes de datos y formatos que se admiten".

>[!IMPORTANT]
>- La suscripción de Azure que contiene la factoría de datos y el almacén de datos del receptor debe estar en el mismo inquilino de Azure Active Directory (Azure AD) que Office 365.
>- Asegúrese de que la región de Azure Integration Runtime utilizada para la actividad de copia y el destino están en la misma región donde se encuentra el buzón de los usuarios del inquilino de Office 365. Haga clic [aquí](concepts-integration-runtime.md#integration-runtime-location) para obtener información sobre cómo se determina la ubicación de Azure IR. Consulte la [tabla aquí](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Capabilities#data-regions) para obtener la lista de las regiones admitidas de Office y las regiones de Azure correspondientes.
>-  Si va a cargar los datos de Office 365 en **Azure Blob Storage** como destino, asegúrese de que utiliza la **[autenticación de entidad de servicio](connector-azure-blob-storage.md#service-principal-authentication)** al definir el servicio vinculado a Azure Blob Storage y de que no usa las autenticaciones [clave de cuenta](connector-azure-blob-storage.md#account-key-authentication), [firma de acceso compartido](connector-azure-blob-storage.md#shared-access-signature-authentication) o [identidades administradas para los recursos de Azure](connector-azure-blob-storage.md#managed-identity).
>-  Si va a cargar los datos de Office 365 en **Azure Data Lake Storage Gen1** como destino, asegúrese de que utiliza la [**autenticación de entidad de servicio**](connector-azure-data-lake-store.md#use-service-principal-authentication) al definir el servicio vinculado a Azure Data Lake Storage Gen1 y de que no usa las [identidades administradas para la autenticación de los recursos de Azure](connector-azure-data-lake-store.md#managed-identity).

## <a name="prerequisites"></a>Requisitos previos

Para copiar datos de Office 365 en Azure, es preciso completar los pasos de requisitos previos siguientes:

- El administrador de inquilinos de Office 365 debe completar las acciones de incorporación como se describe [aquí](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/On-boarding).
- Cree y configure una aplicación web de Azure AD en Azure Active Directory.  Para obtener instrucciones, vea cómo [crear una aplicación de Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application).
- Anote los siguientes valores; los usará para definir el servicio vinculado para Office 365:
    - Identificador de inquilino.  Para obtener instrucciones, vea [Obtención del identificador de inquilino](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-id).
    - Identificador de aplicación y clave de aplicación.  Para obtener instrucciones, vea [Obtención del id. y la clave de autenticación de la aplicación](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key).
- Agregue la identidad del usuario que va a realizar la solicitud de acceso a los datos como propietario de la aplicación web de Azure AD (aplicación web de Azure AD > Configuración > Propietarios > Agregar propietario).

## <a name="approving-new-data-access-requests"></a>Aprobar nuevas solicitudes de acceso a los datos

Si es la primera vez que solicita datos para este contexto (una combinación de los datos de tabla a los que se accede, la cuenta de destino en la que se cargan los datos y la identidad de usuario que realiza la solicitud de acceso a los datos), verá el estado de la actividad de copia como "En curso", y solo al hacer clic en el [vínculo "Detalles" en Acciones](copy-activity-overview.md#monitoring), verá el estado como "RequestingConsent".  Un miembro del grupo de aprobadores de acceso a datos debe aprobar la solicitud en Privileged Access Management antes de pasar a la extracción de datos.

Consulte [aquí](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Approving-data-access-requests) cómo el aprobador puede aprobar la solicitud de acceso a los datos y haga clic [aquí](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#integration-with-privileged-access-management) para obtener una explicación sobre la integración total con Privileged Access Management, incluida la forma de configurar el grupo de aprobadores de acceso a los datos.

## <a name="policy-validation"></a>Validación de directivas

Si ADF se crea como parte de una aplicación administrada y las asignaciones de directivas de Azure se realizan en los recursos dentro del grupo de recursos de administración, entonces, para cada ejecución de la actividad de copia, ADF comprobará para asegurarse de que se aplican las asignaciones de directivas. Consulte [aquí](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Capabilities#policies) para obtener una lista de las directivas admitidas.

## <a name="getting-started"></a>Introducción

>[!TIP]
>Para ver un tutorial sobre el uso del conector de Office 365, consulte el artículo [Carga de datos de Office 365](load-office-365-data.md).

Puede crear una canalización con la actividad de copia mediante una de las siguientes herramientas o SDK. Seleccione un vínculo para acceder a un tutorial con instrucciones detalladas sobre cómo crear una canalización con una actividad de copia. 

- [Azure Portal](quickstart-create-data-factory-portal.md)
- [SDK de .NET](quickstart-create-data-factory-dot-net.md)
- [SDK de Python](quickstart-create-data-factory-python.md)
- [Azure PowerShell](quickstart-create-data-factory-powershell.md)
- [API DE REST](quickstart-create-data-factory-rest-api.md)
- [Plantilla de Azure Resource Manager](quickstart-create-data-factory-resource-manager-template.md) 

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector de Office 365.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Office 365:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type debe establecerse en: **Office365** | SÍ |
| office365TenantId | Id. de inquilino de Azure al que pertenece la cuenta de Office 365. | SÍ |
| servicePrincipalTenantId | Especifique la información del inquilino en el que reside la aplicación web de Azure AD. | SÍ |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | SÍ |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory. | SÍ |
| connectVia | El entorno de ejecución de integración que se usará para conectarse al almacén de datos.  Si no se especifica, se usará Azure Integration Runtime. | Sin  |

>[!NOTE]
> La diferencia entre **office365TenantId** y **servicePrincipalTenantId** y el valor correspondiente que se debe proporcionar:
>- Si es un desarrollador empresarial que va a desarrollar una aplicación con los datos de Office 365 para usarla en su organización, debe proporcionar el mismo identificador de inquilino para ambas propiedades, que es el identificador de inquilino de AAD de su organización.
>- Si es un ISV que va a desarrollar una aplicación para los clientes, office365TenantId será el identificador de inquilino de AAD (instalador de la aplicación) del cliente y servicePrincipalTenantId será el identificador de inquilino de AAD de la empresa.

**Ejemplo:**

```json
{
    "name": "Office365LinkedService",
    "properties": {
        "type": "Office365",
        "typeProperties": {
            "office365TenantId": "<Office 365 tenant id>",
            "servicePrincipalTenantId": "<AAD app service principal tenant id>",
            "servicePrincipalId": "<AAD app service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<AAD app service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Office 365.

Para copiar datos de Office 365, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del conjunto de datos debe establecerse en: **Office365Table** | SÍ |
| tableName | Nombre del conjunto de datos para extraer de Office 365. Haga clic [aquí](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#datasets) para obtener la lista de conjuntos de datos de Office 365 disponibles para la extracción. | SÍ |
| predicate | Una expresión de predicado que se puede usar para filtrar las filas específicas para extraer de Office 365.  Haga clic [aquí](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#filters) para averiguar qué columnas se pueden usar para el filtrado de predicados de cada tabla y el formato de expresión de filtro. | Sin <br>(Si no se proporciona ningún predicado, el valor predeterminado es extraer datos durante los treinta últimos días). |

**Ejemplo**

```json
{
    "name": "Office365Table1",
    "properties": {
        "type": "Office365Table",
        "linkedServiceName": {
            "referenceName": "<Office 365 linked service name>",
            "type": "LinkedServiceReference"
        },
        "structure": [
            {
                "name": "ReceivedDateTime",
                "type": "datetime"
            },
            {
                "name": "SentDateTime",
                "type": "datetime"
            },
            {
                "name": "HasAttachments",
                "type": "boolean"
            },
            {
                "name": "InternetMessageId",
                "type": "string"
            },
            {
                "name": "Subject",
                "type": "string"
            },
            {
                "name": "Importance",
                "type": "string"
            },
            {
                "name": "ParentFolderId",
                "type": "string"
            },
            {
                "name": "Sender",
                "type": "string"
            },
            {
                "name": "From",
                "type": "string"
            },
            {
                "name": "ToRecipients",
                "type": "string"
            },
            {
                "name": "CcRecipients",
                "type": "string"
            },
            {
                "name": "BccRecipients",
                "type": "string"
            },
            {
                "name": "ReplyTo",
                "type": "string"
            },
            {
                "name": "ConversationId",
                "type": "string"
            },
            {
                "name": "UniqueBody",
                "type": "string"
            },
            {
                "name": "IsDeliveryReceiptRequested",
                "type": "boolean"
            },
            {
                "name": "IsReadReceiptRequested",
                "type": "boolean"
            },
            {
                "name": "IsRead",
                "type": "boolean"
            },
            {
                "name": "IsDraft",
                "type": "boolean"
            },
            {
                "name": "WebLink",
                "type": "string"
            },
            {
                "name": "CreatedDateTime",
                "type": "datetime"
            },
            {
                "name": "LastModifiedDateTime",
                "type": "datetime"
            },
            {
                "name": "ChangeKey",
                "type": "string"
            },
            {
                "name": "Categories",
                "type": "string"
            },
            {
                "name": "Id",
                "type": "string"
            },
            {
                "name": "Attachments",
                "type": "string"
            }
        ],
        "typeProperties": {
            "tableName": "BasicDataSet_v0.Contact_v0",
            "predicate": "CreatedDateTime >= 2018-07-11T16:00:00.000Z AND CreatedDateTime <= 2018-07-18T16:00:00.000Z"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de Office 365.

### <a name="office-365-as-source"></a>Office 365 como origen

Para copiar datos desde Office 365, establezca el tipo de origen de la actividad de copia en **Office365Source**. No se admiten propiedades adicionales en la sección **origen** de la actividad de copia.

**Ejemplo:**

```json
"activities": [
    {
        "name": "CopyFromO365ToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Office 365 input dataset name>",
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
                "type": "Office365Source"
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
