---
title: 'Web API que llama a web API (llamada de API): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo crear una web API que web de bajada llamadas API (llamando a una API web).
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
ms.openlocfilehash: 1cd093cc68a21558dc326221eeaa8c034c24f1c2
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074731"
---
# <a name="web-api-that-calls-web-apis---call-an-api"></a>Web API que llama a web API: llamar a una API

Una vez que un token, puede llamar a una API web protegida. Esto se realiza desde el controlador de la API web de ASP.NET/ASP.NET Core.

## <a name="controller-code"></a>Código de controlador

Esta es la continuación del código de ejemplo se muestra en [llamadas de API web Protected web API: adquirir un token](scenario-web-api-call-api-acquire-token.md), denominado en las acciones de los controladores de API, al llamar a una API de nivel inferior (denominada todolist).

Una vez que ha adquirido el token, usarlo como un token de portador para llamar a la API de bajada.

```CSharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...

// Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Pasar a producción](scenario-web-api-call-api-production.md)
