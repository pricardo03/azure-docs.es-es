---
title: Parámetros y encabezados comunes
description: Los parámetros y encabezados comunes a todas las operaciones que puede realizar relacionadas con los recursos de Key Vault.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 7ec8e1df71e6513b13e9c37174a3363471be01d9
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70879272"
---
# <a name="common-parameters-and-headers"></a>Parámetros y encabezados comunes

La siguiente información es común a todas las operaciones que puede realizar relacionadas con los recursos de Key Vault:

- Reemplace `{api-version}` por la versión de api del URI.
- Reemplace `{subscription-id}` por el identificador de suscripción del URI.
- Sustituya `{resource-group-name}` por el grupo de recursos. Para más información, consulte el artículo Uso de grupos de recursos para administrar los recursos de Azure.
- Reemplace `{vault-name}` por el nombre del almacén de claves del URI.
- Establezca el encabezado Content-Type en application/json.
- Establezca el encabezado Authorization en un token web de JSON que se obtiene de Azure Active Directory (AAD). Par más información, consulte [Solicitudes de autenticación de Azure Resource Manager](authentication-requests-and-responses.md).

## <a name="common-error-response"></a>Respuestas de errores habituales
El servicio usará códigos de estado HTTP para indicar el éxito o el error. Además, los errores contienen una respuesta en el formato siguiente:

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|Nombre del elemento | type | DESCRIPCIÓN |
|---|---|---|
| código | string | El tipo de error que se produjo.|
| message | string | Una descripción de lo que produjo el error. |



## <a name="see-also"></a>Otras referencias
 [Referencia de la API REST de Azure Key Vault](/rest/api/keyvault/)
