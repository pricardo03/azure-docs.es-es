---
title: 'API web que llama a otras API web (adquisición de un token para la aplicación): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo crear una API web que llama a otras API web (para adquirir un token para la aplicación).
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
ms.openlocfilehash: 986e2e0f8a481d61dc870af2548290658b44d2d3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65231101"
---
# <a name="web-api-that-calls-web-apis---acquire-a-token-for-the-app"></a>API web que llama a las API web: adquisición de un token para la aplicación

Una vez que ha creado un objeto de aplicación cliente, lo puede usar para adquirir un token y usarlo para llamar a una API web.

## <a name="code-in-the-controller"></a>Código del controlador

Este es un ejemplo de código que se llamará en las acciones de los controladores de API, en el que se llama a una API de nivel inferior (denominada todolist).

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

`BuildConfidentialClient()` es similar a lo que ha visto en el artículo [API web que llama a las API web: configuración de la aplicación ](scenario-web-api-call-api-app-configuration.md). `BuildConfidentialClient()` crea una instancia de `IConfidentialClientApplication` con una caché que contiene únicamente información de una cuenta. La cuenta se proporciona mediante el método `GetAccountIdentifier`.

El método `GetAccountIdentifier` usa las notificaciones asociadas con la identidad del usuario para el que la API web ha recibido el JWT:

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
> [Llamada a una API web](scenario-web-api-call-api-call-api.md)
