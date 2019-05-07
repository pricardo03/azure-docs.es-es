---
title: Web API que otras llamadas a API web (adquirir un token para la aplicación) - plataforma Microsoft identity
description: Obtenga información sobre cómo crear una web API que otras llamadas a web API (adquirir un token para la aplicación).
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
ms.openlocfilehash: ada4323b0e61c6dc9fb87af133c40ec9c35e3834
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074986"
---
# <a name="web-api-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web API que llama a web API: adquirir un token para la aplicación

Una vez que ha creado una aplicación cliente de objetos, usarlo para adquirir un token que puede usar para llamar a una API web.

## <a name="code-in-the-controller"></a>En el controlador de código

Este es un ejemplo de código que se llamará en las acciones de los controladores de API, una llamada a una API de nivel inferior (denominada todolist).

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
}
```

`BuildConfidentialClient()` es similar a lo que ha visto en el artículo [API - configuración de la aplicación web de API Web que llama a](scenario-web-api-call-api-app-configuration.md). `BuildConfidentialClient()` crea una instancia de `IConfidentialClientApplication` con una memoria caché que contiene únicamente la información de una cuenta. La cuenta es proporcionada por el `GetAccountIdentifier` método.

El `GetAccountIdentifier` método usa las notificaciones asociadas con la identidad del usuario para el que la API web recibe el token JWT:

```CSharp
public static string GetMsalAccountId(this ClaimsPrincipal claimsPrincipal)
{
 string userObjectId = GetObjectId(claimsPrincipal);
 string tenantId = GetTenantId(claimsPrincipal);

 if (    !string.IsNullOrWhiteSpace(userObjectId)
      && !string.IsNullOrWhiteSpace(tenantId))
 {
  return $"{userObjectId}.{tenantId}";
 }

 return null;
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Una llamada a una API web](scenario-web-api-call-api-call-api.md)
