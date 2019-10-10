---
title: 'Aplicación web que llama a las API web (inicio de sesión): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que llame a API web (inicio de sesión)
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
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3036f8cb72f2a07673743a77e8be37614002563f
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71720197"
---
# <a name="web-app-that-calls-web-apis---sign-in"></a>Aplicación web que llama a las API web: inicio de sesión

Ya sabe cómo agregar el inicio de sesión a una aplicación web. Obtuvo información sobre esto en [Aplicación web que permite iniciar sesión a los usuarios (inicio de sesión)](scenario-web-app-sign-user-sign-in.md).

Lo diferente de aquí es que, cuando el usuario ha cerrado la sesión, desde esta aplicación o desde cualquier aplicación, querrá quitar de la caché de tokens los tokens asociados con el usuario.

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>Interceptación de la devolución de llamada después del cierre de sesión: cierre de sesión único

La aplicación puede interceptar el evento posterior a `logout`, por ejemplo para borrar la entrada de la caché de tokens asociada con la cuenta que ha cerrado la sesión. La aplicación web almacenará los tokens de acceso para el usuario en una memoria caché. Interceptar la devolución de llamada posterior a `logout` permite que la aplicación web quite al usuario de la caché de tokens.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Este mecanismo se ilustra en el método `AddMsal()` de [WebAppServiceCollectionExtensions.cs#L151-L157](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L157)

La **dirección URL de cierre de sesión** que ha registrado para la aplicación le permite implementar el cierre de sesión único. El punto de conexión `logout` de la Plataforma de identidad de Microsoft llamará a la **URL de cierre de sesión** registrada en la aplicación. Esta llamada se produce si el cierre de sesión se ha iniciado desde la aplicación web, o desde otra aplicación web o el explorador. Para más información, consulte [Cierre de sesión único](v2-protocols-oidc.md#single-sign-out).

```CSharp
public static class WebAppServiceCollectionExtensions
{
 public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
 {
  // Code omitted here

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Code omitted here

   // Handling the sign-out: removing the account from MSAL.NET cache
   options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
   {
    // Remove the account from MSAL.NET token cache
    var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
    await tokenAcquisition.RemoveAccountAsync(context).ConfigureAwait(false);
   };
  });
  return services;
 }
}
```

El código de RemoveAccountAsync está disponible en [Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288).

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

El ejemplo de ASP.NET no quita las cuentas de la memoria caché en el cierre de sesión global

# <a name="javatabjava"></a>[Java](#tab/java)

El ejemplo de Java no quita las cuentas de la memoria caché en el cierre de sesión global

# <a name="pythontabpython"></a>[Python](#tab/python)

El ejemplo de Python no quita las cuentas de la memoria caché en el cierre de sesión global

---

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquisición de un token para la aplicación web](scenario-web-app-call-api-acquire-token.md)
