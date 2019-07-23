---
title: 'API web que llama a API web (adquisición de un token para la aplicación): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que llame a API web (adquisición de un token para la aplicación).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 653db995000308bb3ef78a9183696cd9d8ed1056
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074806"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>API web que llama a las API web: adquisición de un token para la aplicación

Ahora que ha compilado su objeto de aplicación cliente, lo usará para adquirir un token que luego usará para llamar a las API web. En ASP.NET o ASP.NET Core, las llamadas a una API web se hacen en el controlador. Se trata de:

- Obtener un token para la API web mediante la caché de tokens. Para ello, llama a `AcquireTokenSilent`.
- Llamar a la API protegida con el token de acceso.

## <a name="aspnet-core"></a>ASP.NET Core

Los métodos de controlador están protegidos por un atributo `[Authorize]` que obliga a los usuarios a autenticarse para usar la aplicación web. Este es el código que llama a Microsoft Graph

```CSharp
[Authorize]
public class HomeController : Controller
{
 ...
}
```

Este es un código simplificado de la acción de HomeController, que obtiene un token para llamar a Microsoft Graph.

```CSharp
public async Task<IActionResult> Profile()
{
 var application = BuildConfidentialClientApplication(HttpContext, HttpContext.User);
 string accountIdentifier = claimsPrincipal.GetMsalAccountId();
 string loginHint = claimsPrincipal.GetLoginHint();

 // Get the account
 IAccount account = await application.GetAccountAsync(accountIdentifier);

 // Special case for guest users as the Guest iod / tenant id are not surfaced.
 if (account == null)
 {
  var accounts = await application.GetAccountsAsync();
  account = accounts.FirstOrDefault(a => a.Username == loginHint);
 }

 AuthenticationResult result;
 result = await application.AcquireTokenSilent(new []{"user.read"}, account)
                            .ExecuteAsync();
 var accessToken = result.AccessToken;
 ...
 // use the access token to call a web API
}
```

Si quiere conocer en detalle el total del código necesario para este escenario, consulte el paso de [2-1-aplicación web llama a Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph) de la fase 2 del tutorial [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial).

Hay varias complejidades adicionales, como:

- Implementar una caché de tokens para la aplicación web (el tutorial presenta varias implementaciones)
- Quitar la cuenta de la memoria caché cuando el usuario cierra sesión
- Llamar a varias API, incluido el consentimiento incremental

## <a name="aspnet"></a>ASP.NET

Las cosas son similares en ASP.NET:

- Una acción de controlador protegida por un atributo [Authorize] extraerá el identificador de inquilino y el identificador de usuario del miembro `ClaimsPrincipal` del controlador (ASP.NET usa `HttpContext.User`).
- Desde allí se compila un MSAL.NET `IConfidentialClientApplication`.
- TI llama al método `AcquireTokenSilent` de la aplicación cliente confidencial. 

El código es similar al código que se muestra para ASP.NET Core.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamada a una API de web](scenario-web-app-call-api-call-api.md)
