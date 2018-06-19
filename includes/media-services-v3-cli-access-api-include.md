---
title: archivo de inclusión
description: archivo de inclusión
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/29/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 7454e96a2a05bf89a0455674a4f144534c374c71
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34667884"
---
## <a name="access-the-media-services-api"></a>Acceso a la API de Media Services

Debe usar la autenticación de la entidad de servicio de Azure AD para conectarse a las API de Azure Media Services. El siguiente comando crea una aplicación de Azure AD y asocia una entidad de servicio a la cuenta. Debe usar los valores devueltos para configurar la aplicación.

Antes de ejecutar el script, puede reemplazar `amsaccount` y `amsResourceGroup` por los nombres que eligió al crear estos recursos. `amsaccount` es el nombre de la cuenta de Azure Media Services a la que asociar la entidad de servicio.

El siguiente comando devuelve una salida `json`:

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Este comando genera una respuesta similar a esta:

```json
{
  "AadClientId": "00000000-4cdd-418a-8a72-0755ace03de5",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-02f5-4bf2-9057-1c4f7baff155",
  "AadTenantId": "00000000-86f1-41af-91ab-2d7cd011db47",
  "AccountName": "amsaccount22",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup2",
  "SubscriptionId": "00000000-6753-4ca2-b1ae-193798e2c9d8"
}
```

Si desea obtener un `xml` en la respuesta, use el siguiente comando:

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
