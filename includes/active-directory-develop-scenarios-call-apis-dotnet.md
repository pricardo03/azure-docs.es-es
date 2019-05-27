---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 0196d39f5b131bc54e00412beb7fdf10b7352336
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121857"
---
### <a name="authenticationresult-properties-in-msalnet"></a>Propiedades de AuthenticationResult en MSAL.NET

Los métodos para adquirir tokens devuelven un `AuthenticationResult` (o, para los métodos asincrónicos, una `Task<AuthenticationResult>`.

En MSAL.NET, `AuthenticationResult` expone:

- `AccessToken` para la API Web para acceder a los recursos. Este parámetro es una cadena, normalmente un base64 codificado JWT pero nunca debe ser el cliente dentro del token de acceso. No se garantiza que el formato permanezca estable y se pueda cifrar para el recurso. Los usuarios que escriben código según el contenido de un token de acceso del cliente constituyen una de las fuentes de errores y de interrupciones de la lógica de cliente más habituales. Vea también [tokens de acceso](../articles/active-directory/develop/access-tokens.md)
- `IdToken` para el usuario (este parámetro es un JWT codificado). Consulte [los Tokens de identificador](../articles/active-directory/develop/id-tokens.md)
- `ExpiresOn` indica la fecha y hora cuando expira el token
- `TenantId` contiene al inquilino en el que se encontró el usuario. Para los usuarios invitados (escenarios de B2B de Azure AD), el identificador del inquilino es el inquilino de invitado, no el inquilino único.
Cuando se envía el token para un usuario, `AuthenticationResult` también contiene información sobre este usuario. Para los flujos de cliente confidencial donde se solicitan tokens sin usuario (para la aplicación), esta información de usuario es null.
- El `Scopes` para que se emitió el token.
- El identificador único para el usuario.

### <a name="iaccount"></a>IAccount

MSAL.NET define la noción de cuenta (a través de la `IAccount` interfaz). Este cambio importante proporciona la semántica correcta: el hecho de que el mismo usuario pueda tener varias cuentas en diferentes directorios de Azure AD. También MSAL.NET proporciona una mejor información en el caso de escenarios de invitado, tal como se proporciona información de la cuenta principal.
El siguiente diagrama muestra la estructura de la `IAccount` interfaz:

![image](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

La `AccountId` clase identifica una cuenta en un inquilino específico. Tiene las siguientes propiedades:

| Propiedad | DESCRIPCIÓN |
|----------|-------------|
| `TenantId` | Representación de cadena en un GUID, que es el identificador del inquilino donde reside la cuenta. |
| `ObjectId` | Representación de cadena en un GUID, que es el identificador del usuario que pertenece la cuenta en el inquilino. |
| `Identifier` | Identificador único para la cuenta. `Identifier` es la concatenación de `ObjectId` y `TenantId` separados por una coma y no codificados en base64. |

El `IAccount` interfaz representa información sobre una sola cuenta. El mismo usuario puede estar presente en distintos inquilinos, es decir, un usuario puede tener varias cuentas. Sus miembros son:

| Propiedad | DESCRIPCIÓN |
|----------|-------------|
| `Username` | Una cadena que contiene el valor que se pueden mostrar en formato UserPrincipalName (UPN), por ejemplo, john.doe@contoso.com. Esta cadena puede ser null, mientras que la HomeAccountId y HomeAccountId.Identifier no será nulo. Esta propiedad reemplaza la `DisplayableId` propiedad de `IUser` en versiones anteriores de MSAL.NET. |
| `Environment` | Una cadena que contiene el proveedor de identidades de dicha cuenta, por ejemplo, `login.microsoftonline.com`. Esta propiedad reemplaza la `IdentityProvider` propiedad de `IUser`, salvo que `IdentityProvider` tenía también información sobre el inquilino (además del entorno de nube), mientras que aquí el valor es solo el host. |
| `HomeAccountId` | Identificador de cuenta de la cuenta de inicio para el usuario. Esta propiedad identifica unívocamente al usuario a través de los inquilinos de Azure AD. |

### <a name="using-the-token-to-call-a-protected-api"></a>Mediante el token para llamar a una API protegida

Una vez el `AuthenticationResult` MSAL ha devuelto (en `result`), deberá agregar al encabezado de autorización HTTP, antes de realizar la llamada para obtener acceso a la API Web protegida.

```CSharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```