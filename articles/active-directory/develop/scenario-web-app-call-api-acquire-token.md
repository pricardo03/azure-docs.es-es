---
title: 'Aplicación Web que llama a web API (adquirir un token para la aplicación): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación Web que llama a web API (adquirir un token para la aplicación)
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
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074806"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>Aplicación Web que llama a web API: adquirir un token para la aplicación

Ahora que ha creado el objeto de aplicación de cliente, se usará para adquirir un token que va a usar, a continuación, llamar a una API web. En ASP.NET o ASP.NET Core, al llamar a una web que API, a continuación, se realiza en el controlador. Se trata de:

- Obtener un token para la API web mediante la caché de tokens. Para ello, llame a `AcquireTokenSilent`
- Una llamada a la API protegida con el token de acceso

## <a name="aspnet-core"></a>ASP.NET Core

Los métodos de controlador están protegidos por un `[Authorize]` atributo que obliga a los usuarios se autentiquen para usar la aplicación Web. Este es el código que llama a Microsoft Graph

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

Si desea conocer detalladamente el total del código necesario para este escenario, consulte la fase 2 [2-1-Web App llama a Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph) paso de la [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) tutorial

Hay numerosas complejidades adicionales, como:

- implementación de una caché de tokens para la aplicación Web (tutorial presentan varias implementaciones)
- Elimina la cuenta de la memoria caché cuando el usuario inicia horizontal
- Llamar a varias API, incluyendo el consentimiento incremental

## <a name="aspnet"></a>ASP.NET

Las cosas son similares en ASP.NET:

- Una acción de controlador protegida por un atributo [Authorize], extraerá el inquilino de identificador y el identificador de usuario de la `ClaimsPrincipal` miembro del controlador (ASP.NET usa `HttpContext.User`)
- desde allí se compila un MSAL.NET `IConfidentialClientApplication`
- Llamada de TI la `AcquireTokenSilent` método de la aplicación cliente confidencial 

el código es similar al código que se muestra para ASP.NET Core

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamada a una API de web](scenario-web-app-call-api-call-api.md)
