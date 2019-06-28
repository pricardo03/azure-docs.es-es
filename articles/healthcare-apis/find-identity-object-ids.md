---
title: 'Encontrar identificadores de objeto de identidad para la autenticación: Azure API for FHIR'
description: En este artículo se explica cómo buscar los identificadores de objetos de identidad necesarios para configurar la autenticación de Azure API for FHIR
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: b120b21a89a7105a25ba610402da99f9dce4b7e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033648"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>Encontrar identificadores de objeto de identidad para la autenticación de autenticación

En este artículo, obtendrá información sobre cómo buscar los identificadores de objeto de identidad necesarios para configurar la lista de identificadores de objeto de identidad permitidos para Azure API for FHIR.

Azure API for FHIR totalmente administrada para el &reg;servicio FHIR está configurada para permitir el acceso a solo una lista predefinida de identificadores de objeto de identidad. Cuando una aplicación o un usuario intente acceder a la API de FHIR, se debe presentar un token de portador. Este token de portador tendrán ciertas notificaciones (campos). Para conceder acceso a la API de FHIR, el token debe contener el emisor correcto (`iss`), audiencia (`aud`) y un identificador de objeto (`oid`) de una lista de identificadores de objetos permitidos. Un identificador de objeto de identidad es el identificador de objeto de un usuario o una entidad de servicio en Azure Active Directory.

## <a name="configure-list-of-allowed-object-ids"></a>Configuración de la lista de identificadores de objetos permitidos

Cuando crea una nueva instancia de Azure API for FHIR, puede configurar una lista de identificadores de objetos permitidos:

![Configuración de identificadores de objeto permitidos](media/quickstart-paas-portal/configure-allowed-oids.png)

Estos identificadores de objeto pueden serlo para usuarios específicos o para entidades de servicio en Azure Active Directory.

## <a name="find-user-object-id-using-powershell"></a>Búsqueda de identificadores de objeto de usuario mediante PowerShell

Si tiene un usuario con el nombre de usuario `myuser@consoso.com`, puede buscar el `ObjectId` de los usuarios con el siguiente comando de PowerShell:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

O bien, puede usar la CLI de Azure:

```azurecli-interactive
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

## <a name="find-service-principal-object-id-using-powershell"></a>Búsqueda del identificador de objeto de entidad de servicio con PowerShell

Supongamos que ha registrado una [aplicación cliente de servicio](register-service-azure-ad-client-app.md) y desea permitir que este cliente de servicio tenga acceso a Azure API for FHIR, puede encontrar el identificador de objeto para la entidad de servicio de cliente con el siguiente comando de PowerShell:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

donde `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` es el identificador de la aplicación cliente de servicio. Como alternativa, puede usar el `DisplayName` del cliente de servicio:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

Si usa la CLI de Azure, puede usar:

```azurecli-interactive
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX | jq -r .objectId
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo buscar los identificadores de objeto de identidad necesarios para configurar las identidades que pueden tener acceso a una instancia de Azure API for FHIR. A continuación, implemente una instancia de Azure API for FHIR totalmente administrada:
 
>[!div class="nextstepaction"]
>[Implementación de FHIR Server de código abierto](fhir-paas-portal-quickstart.md)