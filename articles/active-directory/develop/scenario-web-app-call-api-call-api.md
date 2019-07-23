---
title: 'Aplicación web que llama a las API web (llamada a una API web): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que llame a las API web (llamada a una API web)
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
ms.openlocfilehash: dd44dda06b2f6fc48538f2fb74c0bf8e04d0362b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074641"
---
# <a name="web-app-that-calls-web-apis---call-a-web-api"></a>Aplicación web que llama a las API web: llamada a una API web

Ahora que dispone de un token, puede llamar a una API web protegida.

## <a name="aspnet-core"></a>ASP.NET Core

Este es un código simplificado de la acción de `HomeController`. Este código obtiene un token para llamar a Microsoft Graph. Se agregó este código de tiempo, que muestra cómo llamar a Microsoft Graph como API de REST.

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

 // Calls the web API (here the graph)
 HttpClient httpClient = new HttpClient();
 httpClient.DefaultRequestHeaders.Authorization =
     new AuthenticationHeaderValue(Constants.BearerAuthorizationScheme,accessToken);
 var response = await httpClient.GetAsync($"{webOptions.GraphApiUrl}/beta/me");

 if (response.StatusCode == HttpStatusCode.OK)
 {
  var content = await response.Content.ReadAsStringAsync();

  dynamic me = JsonConvert.DeserializeObject(content);
  return me;
 }

 ViewData["Me"] = me;
 return View();
}
```

> [!NOTE]
> Puede usar el mismo principio para llamar a cualquier API web.
>
> La mayoría de las API web de Azure proporcionan un SDK que simplifica la llamada. Esto también sucede con Microsoft Graph. En el siguiente artículo obtendrá información de dónde encontrar un tutorial que ilustra estos aspectos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paso a producción](scenario-web-app-call-api-production.md)
