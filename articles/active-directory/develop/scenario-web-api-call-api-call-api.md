---
title: 'API web que llama a otras API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a compilar una API web que llama a otras API web.
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
ms.openlocfilehash: 701f1f7c26731f3e9653955907f5f16d2688cdb2
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76043437"
---
# <a name="a-web-api-that-calls-web-apis-call-an-api"></a>Una API web que llama a las API web: Llamar a una API

Una vez que disponga de un token, puede llamar a una API web protegida. Esto se hace desde el controlador de la API web de ASP.NET o ASP.NET Core.

## <a name="controller-code"></a>Código del controlador

El código siguiente continúa el código de ejemplo que se muestra en [Una API web que llama a las API web: Adquisición de un token para la aplicación](scenario-web-api-call-api-acquire-token.md). El código se llama en las acciones de los controladores de API. Llama a una API de nivel inferior denominada *todolist*.

Una vez que haya adquirido el token, úselo como token de portador para llamar a la API de nivel inferior.

```csharp
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

// After the token has been returned by Microsoft Authentication Library (MSAL), add it to the HTTP authorization header before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Una API web que llama a las API web: Paso a producción](scenario-web-api-call-api-production.md)
