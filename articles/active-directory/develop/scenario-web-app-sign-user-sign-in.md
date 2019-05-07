---
title: 'Aplicación Web que los usuarios inicia sesión (inicio de sesión): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que los usuarios inicia sesión (inicio de sesión)
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
ms.openlocfilehash: 3fb7fbba7ec48da580d2a630ae51aa20b3307848
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074626"
---
# <a name="web-app-that-signs-in-users---sign-in"></a>Aplicación que los usuarios inicia sesión Web: iniciar sesión

Obtenga información sobre cómo agregar inicio de sesión en el código de la aplicación web que los usuarios inicia sesión.

## <a name="sign-in"></a>Inicio de sesión

El código que haya en el artículo anterior [configuración de la aplicación de código](scenario-web-app-sign-user-app-configuration.md) es todo lo que necesita para implementar cierre de sesión. Una vez que el usuario ha iniciado sesión a la aplicación, probablemente desee habilitarlas para cerrar la sesión. ASP.NET core controla cierre de sesión para usted.

## <a name="what-sign-out-involves"></a>Cerrar la sesión lo que implica

Cierre de sesión desde una aplicación web es acerca de cómo quitar más de la información acerca de la cuenta con sesión iniciada desde el estado de la aplicación web.
La aplicación web también debe redirigir al usuario a la versión 2.0 de Microsoft identity platform `logout` punto de conexión para cerrar la sesión. Cuando la aplicación web redirige al usuario a la `logout` este punto de conexión de punto de conexión, borra la sesión del usuario desde el explorador. Si la aplicación no vaya a la `logout` punto de conexión, el usuario podría autenticarse de nuevo a la aplicación sin escribir sus credenciales de nuevo, porque tendrían una válido única inicio de sesión con el punto de conexión de v2.0 de plataforma de Microsoft Identity.

Para obtener más información, consulte el [enviar una solicitud de cierre de sesión](v2-protocols-oidc.md#send-a-sign-out-request) sección la [v2.0 de plataforma de Microsoft Identity y el protocolo OpenID Connect](v2-protocols-oidc.md) documentación conceptual.

## <a name="application-registration"></a>Registro de la aplicación

Durante el registro de aplicación, se habrá registrado un **registrar URI de cierre de sesión**. En este tutorial, ha registrado `https://localhost:44321/signout-oidc` en el **dirección URL de cierre de sesión** campo de la **configuración avanzada** sección la **autenticación** página. Para obtener información detallada, consulte [ registrar la aplicación webApp](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp)

## <a name="aspnet-core-code"></a>Código de ASP.NET Core

### <a name="signout-button"></a>Botón de cierre de sesión

El botón de cierre de sesión se expone en `Views\Shared\_LoginPartial.cshtml` y solo se muestra cuando hay una cuenta autenticada (es decir, cuando el usuario ha iniciado previamente).

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

### <a name="signout-action-of-the-accountcontroller"></a>`Signout()` acción de la `AccountController`

Al presionar el **cerrar sesión** botón sobre los desencadenadores de aplicación web la `SignOut` acción en el `Account` controlador. En versiones anteriores de las plantillas ASP.NET core, el `Account` controlador estaba incrustado con la aplicación web, pero esto ya no es el caso ya que ahora forma parte del marco ASP.NET Core en Sí. 

El código para el `AccountController` está disponible en el repositorio principal de ASP.NET en desde [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs). El control de cuentas:

- Establece un OpenID redirigir la URI para `/Account/SignedOut` para que el controlador se vuelve a llamar cuando Azure AD ha realizado el cierre de sesión
- Las llamadas `Signout()`, que permite que el middleware de OpenIdConnect póngase en contacto con la plataforma Microsoft identity `logout` punto de conexión que:

  - Borra la cookie de sesión desde el explorador, y
  - Llamadas finalmente llama de nuevo el **dirección URL de cierre de sesión**, qué) de forma predeterminada, muestra la página de vista firmado [SignedOut.html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) también se proporciona como parte de ASP.NET Core.

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>Interceptar la llamada a la `logout` punto de conexión

El middleware de ASP.NET Core OpenIdConnect permite a la aplicación interceptar la llamada a la plataforma Microsoft identity `logout` extremo al proporcionar un evento de OpenIdConnect denominado `OnRedirectToIdentityProviderForSignOut`. La aplicación web usa para intentar evitar el cuadro de diálogo Seleccione cuenta que se presentará al usuario cuando se cierre de sesión. Esta interceptación se realiza en el `AddAzureAdV2Authentication` de la `Microsoft.Identity.Web` biblioteca reutilizable. Consulte [StartupHelpers.cs L58-L66](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L58-L66)

```CSharp
public static IServiceCollection AddAzureAdV2Authentication(this IServiceCollection services,
                                                            IConfiguration configuration)
{
    ...
    services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
    {
        ...
        options.Authority = options.Authority + "/v2.0/";
        ...
        // Attempt to avoid displaying the select account dialog when signing out
        options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
        {
            var user = context.HttpContext.User;
            context.ProtocolMessage.LoginHint = user.GetLoginHint();
            context.ProtocolMessage.DomainHint = user.GetDomainHint();
            await Task.FromResult(0);
        };
    }
}
```

## <a name="aspnet-code"></a>Código de ASP.NET

En ASP.NET, el cierre de sesión se desencadena desde el método SignOut() en un controlador (por ejemplo AccountController). Este método no forma parte del marco de ASP.NET (al contrario de lo que ocurre en ASP.NET Core) y no usa async y por eso:

- envía un desafío de cierre de sesión de OpenId
- Borra la memoria caché
- redirige a la página que desea

```CSharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 MsalAppBuilder.ClearUserTokenCache();
 Response.Redirect("/");
}
```

## <a name="protocol"></a>Protocol

Si no desea usar ASP.NET Core o ASP.NET, puede consultar la documentación del protocolo, que está disponible en [Open ID Connect](./v2-protocols-oidc.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Pasar a producción](scenario-web-app-sign-user-production.md)
