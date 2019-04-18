---
title: 'Conectarse a la API de Azure Media Services v3: Python'
description: Obtenga información sobre cómo conectarse a la API de Media Services v3 con Python.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: 971e36b600a2c6be516e39ce84ca5780a2f23bbd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59700218"
---
# <a name="connect-to-media-services-v3-api---python"></a>Conectarse a la API de Media Services v3 - Python

Este artículo muestra cómo conectar con el SDK de Python de Azure Media Services v3 con el inicio de sesión entidad de servicio en el método.

## <a name="prerequisites"></a>Requisitos previos

- Descargar Python desde [python.org](https://www.python.org/downloads/)
- Asegúrese de establecer el `PATH` variable de entorno
- [Cree una cuenta de Media Services](create-account-cli-how-to.md). Asegúrese de recordar el nombre del grupo de recursos y el nombre de cuenta de Media Services.
- Siga los pasos descritos en la [acceder a las API](access-api-cli-how-to.md) tema. Registre el identificador de suscripción, identificador de la aplicación (Id. de cliente), la clave de autenticación (secreto) y el identificador de inquilino que necesitará en el paso posterior.

## <a name="install-the-modules"></a>Instalación de los módulos

Para trabajar con Azure Media Services mediante Python, debe instalar estos módulos.

* El `azure-mgmt-resource` módulo, que incluye los módulos de Azure para Active Directory.
* El `azure-mgmt-media` módulo, que incluye las entidades de Media Services.

Abra una herramienta de línea de comandos y use los siguientes comandos para instalar los módulos.

```
pip3 install azure-mgmt-resource
pip3 install azure-mgmt-media==1.1.1
```

## <a name="connect-to-the-python-client"></a>Conectar con el cliente de Python

1. Cree un archivo con un `.py` extensión
1. Abra el archivo en su editor favorito
1. Agregue el código siguiente al archivo. El código importa los módulos necesarios y crea el objeto de credenciales de Active Directory que necesita para conectarse a Media Services.

      Establezca los valores de las variables en los valores que se obtuvo al [acceder a las API](access-api-cli-how-to.md)

      ```
      import adal
      from msrestazure.azure_active_directory import AdalAuthentication
      from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
      from azure.mgmt.media import AzureMediaServices
      from azure.mgmt.media.models import MediaService

      RESOURCE = 'https://management.core.windows.net/'
      # Tenant ID for your Azure Subscription
      TENANT_ID = '00000000-0000-0000-000000000000'
      # Your Service Principal App ID
      CLIENT = '00000000-0000-0000-000000000000'
      # Your Service Principal Password
      KEY = '00000000-0000-0000-000000000000'
      # Your Azure Subscription ID
      SUBSCRIPTION_ID = '00000000-0000-0000-000000000000'
      # Your Azure Media Service (AMS) Account Name
      ACCOUNT_NAME = 'amsv3account'
      # Your Resource Group Name
      RESOUCE_GROUP_NAME = 'AMSv3ResourceGroup'

      LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
      RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

      context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
      credentials = AdalAuthentication(
          context.acquire_token_with_client_credentials,
          RESOURCE,
          CLIENT,
          KEY
      )

      # The AMS Client
      # You can now use this object to perform different operations to your AMS account.
      client = AzureMediaServices(credentials, SUBSCRIPTION_ID)

      print("signed in")

      # Now that you are authenticated, you can manipulate the entities.
      # For example, list assets in your AMS account
      print (client.assets.list(RESOUCE_GROUP_NAME, ACCOUNT_NAME).get(0))
      ```

1. Ejecute el archivo

## <a name="next-steps"></a>Pasos siguientes

- Use el [SDK de Python](https://aka.ms/ams-v3-python-sdk).
- Revise la documentación de [referencia de Python](https://aka.ms/ams-v3-python-ref) de Media Services.
