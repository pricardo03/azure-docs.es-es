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
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186498"
---
### <a name="authenticationresult-properties-in-msalnet"></a>Propiedades de AuthenticationResult en MSAL.NET

Los métodos para adquirir tokens devuelven `AuthenticationResult` (o, para los métodos asincrónicos, `Task<AuthenticationResult>`).

En MSAL.NET, `AuthenticationResult` expone:

- `AccessToken` para que la API web acceda a los recursos. Este parámetro es una cadena, normalmente una aserción de JWT codificada en base64, pero el cliente no debería mirar nunca dentro del token de acceso. No se garantiza que el formato permanezca estable y se pueda cifrar para el recurso. Los usuarios que escriben código según el contenido de un token de acceso del cliente constituyen una de las fuentes de errores y de interrupciones de la lógica de cliente más habituales. Vea también [Tokens de acceso](../articles/active-directory/develop/access-tokens.md).
- `IdToken` para el usuario (este parámetro es un JWT codificado). Vea [Tokens de identificador](../articles/active-directory/develop/id-tokens.md).
- `ExpiresOn` indica la hora a la que expira el token.
- `TenantId` indica el inquilino en el que se encontró el usuario. Para los usuarios invitados (escenarios de Azure AD B2B), el identificador de inquilino es el inquilino invitado, no el inquilino único.
Cuando se entrega el token para un usuario, `AuthenticationResult` también contiene información sobre dicho usuario. Para los flujos de cliente confidenciales en los que se solicitan tokens sin usuario (para la aplicación), esta información de usuario está vacía.
- Los `Scopes` para los que se emitió el token.
- El identificador único del usuario.

### <a name="iaccount"></a>IAccount

MSAL.NET define la noción de Cuenta (a través de la interfaz `IAccount`). Este cambio importante proporciona la semántica correcta: el hecho de que el mismo usuario pueda tener varias cuentas en diferentes directorios de Azure AD. También MSAL.NET proporciona mejor información en el caso de escenarios de invitado, cuando se proporciona información de la cuenta doméstica.
En el siguiente diagrama se muestra la estructura de la interfaz `IAccount`:

![imagen](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

La clase `AccountId` identifica una cuenta en un inquilino específico. Tiene las siguientes propiedades:

| Propiedad | DESCRIPCIÓN |
|----------|-------------|
| `TenantId` | Representación de una cadena en un GUID, que es el identificador del inquilino donde reside la cuenta. |
| `ObjectId` | Representación de una cadena en un GUID, que es el identificador del usuario a quien pertenece la cuenta en el inquilino. |
| `Identifier` | Identificador único de la cuenta. `Identifier` es la concatenación de `ObjectId` y `TenantId`, separados por una coma y no codificados en base64. |

La interfaz `IAccount` representa información sobre una sola cuenta. El mismo usuario puede estar presente en distintos inquilinos, es decir, un usuario puede tener varias cuentas. Sus miembros son:

| Propiedad | DESCRIPCIÓN |
|----------|-------------|
| `Username` | Una cadena que contiene el valor que se puede mostrar en formato UserPrincipalName (UPN), por ejemplo, john.doe@contoso.com. Esta cadena puede ser NULL, mientras que HomeAccountId y HomeAccountId.Identifier no serán NULL. Esta propiedad reemplaza la propiedad `DisplayableId` de `IUser` en versiones anteriores de MSAL.NET. |
| `Environment` | Una cadena que contiene el proveedor de identidades de dicha cuenta, por ejemplo, `login.microsoftonline.com`. Esta propiedad reemplaza la propiedad `IdentityProvider` de `IUser`, salvo que `IdentityProvider` también tenía información sobre el inquilino (además del entorno de nube), mientras que aquí el valor es solo el host. |
| `HomeAccountId` | AccountId de la cuenta de inicio para el usuario. Esta propiedad identifica unívocamente al usuario a través de los inquilinos de Azure AD. |

### <a name="using-the-token-to-call-a-protected-api"></a>Uso del token para llamar a una API protegida

Cuando MSAL haya devuelto `AuthenticationResult` (en `result`), deberá agregarlo al encabezado de autorización HTTP, antes de realizar la llamada para acceder a la API web protegida.

```CSharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```