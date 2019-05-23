---
title: Almacenamiento de credenciales en Azure Key Vault | Microsoft Docs
description: Aprenda a almacenar credenciales para almacenes de datos usados en una instancia de Azure Key Vault que Azure Data Factory pueda recuperar automáticamente en tiempo de ejecución.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: jingwang
ms.openlocfilehash: a7d440509e2b823400cde83c1ac2ec054c37eb74
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60311920"
---
# <a name="store-credential-in-azure-key-vault"></a>Almacenamiento de credenciales en Azure Key Vault

Puede almacenar las credenciales de los almacenes de datos y los procesos en una instancia de [Azure Key Vault](../key-vault/key-vault-whatis.md). Azure Data Factory las recupera al ejecutar una actividad que usa el almacén de datos o el proceso.

Actualmente, todos los tipos de actividad, excepto la actividad personalizada, admiten esta característica. En cuanto a la configuración de conectores concretos, revise la sección "Propiedades del servicio vinculado" de [cada tema de conector](copy-activity-overview.md#supported-data-stores-and-formats) para obtener más información.

## <a name="prerequisites"></a>Requisitos previos

Esta característica se basa en la identidad de data factory administra. Obtenga información sobre cómo funciona [la identidad de la factoría de datos administrada](data-factory-service-identity.md) y asegúrese de que la factoría de datos tiene una asociada.

## <a name="steps"></a>Pasos

Para hacer referencia a una credencial almacenada en Azure Key Vault, deberá seguir estos pasos:

1. **Recuperar la identidad de data factory administra** copiando el valor "servicio de identidad del identificador de aplicación" generado junto con la factoría. Si usa la interfaz de usuario de creación de ADF, se mostrará el identificador de aplicación de identidad administrada en la ventana de creación del servicio vinculado de Azure Key Vault; También se puede recuperar desde Azure portal, consulte [factoría de datos de recuperar la identidad administrada](data-factory-service-identity.md#retrieve-managed-identity).
2. **Conceda acceso de identidad administrada para Azure Key Vault.** En el almacén de claves de-acceso -> directivas > Agregar -> nuevo, busque administra este Id. de aplicación de identidad para conceder **obtener** permiso en la lista desplegable de permisos de secretos. De esta forma, la factoría designada puede acceder al secreto del almacén de claves.
3. **Creación de un servicio vinculado que apunte a Azure Key Vault.** Consulte [Servicio vinculado de Azure Key Vault](#azure-key-vault-linked-service).
4. **Creación de un servicio vinculado a un almacén de datos en el que se hace referencia al secreto correspondiente guardado en el almacén de claves.** Consulte [Secreto de referencia almacenado en el almacén de claves](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Servicio vinculado de Azure Key Vault

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Key Vault:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **AzureKeyVault**. | Sí |
| baseUrl | Especifique la dirección URL de Azure Key Vault. | Sí |

**Uso de la IU de creación:**

Haga clic en **Conexiones** -> **Servicios vinculados** -> **+ Nuevo** -> busque "Azure Key Vault":

![Búsqueda de AKV](media/store-credentials-in-key-vault/search-akv.png)

Seleccione el almacén de Azure Key Vault aprovisionado en el que se almacenan las credenciales. Puede hacer una **conexión de prueba** para asegurarse de que su conexión de AKV es válida. 

![Configuración de AKV](media/store-credentials-in-key-vault/configure-akv.png)

**Ejemplo JSON:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-secret-stored-in-key-vault"></a>Secreto de referencia almacenado en el almacén de claves

Al configurar un campo en un servicio vinculado que hace referencia a un secreto del almacén de claves, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del campo debe establecerse en: **AzureKeyVaultSecret**. | Sí |
| secretName | El nombre del secreto en Azure Key Vault. | Sí |
| secretVersion | La versión del secreto en Azure Key Vault.<br/>Si no se especifica, siempre se usa la versión más reciente del secreto.<br/>Si se especifica, se usa la versión dada.| Sin  |
| store | Hace referencia a un servicio vinculado de Azure Key Vault que se usa para almacenar las credenciales. | Sí |

**Uso de la IU de creación:**

Seleccione **Azure Key Vault** para campos secretos al crear la conexión al almacén de datos/proceso. Seleccione el servicio vinculado de Azure Key Vault aprovisionado y proporcione el **nombre del secreto**. También puede proporcionar una versión del secreto. 

>[!TIP]
>Para los conectores con la cadena de conexión en el servicio vinculado, como SQL Server, almacenamiento de blobs, etc., puede elegir almacenar solo el campo secreto, por ejemplo, la contraseña en Azure Key VAULT, o para almacenar la cadena de conexión completa en Azure Key VAULT. Puede encontrar ambas opciones en la interfaz de usuario.

![Configuración de secreto de AKV](media/store-credentials-in-key-vault/configure-akv-secret.png)

**Ejemplo de JSON: (consulte la sección "Contraseña")**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
