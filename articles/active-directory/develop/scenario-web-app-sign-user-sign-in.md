---
title: 'Aplicación web que permite iniciar sesión a los usuarios (inicio de sesión): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que permita iniciar sesión a los usuarios (inicio de sesión)
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
ms.openlocfilehash: be7801515355452306cd5e7afa709a0681c7c314
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562194"
---
# <a name="web-app-that-signs-in-users---sign-in"></a>Aplicación web que permite iniciar sesión a los usuarios (inicio de sesión)

Obtenga información sobre cómo agregar el inicio de sesión al código de una aplicación web que permita iniciar sesión a los usuarios.

## <a name="sign-in"></a>Inicio de sesión

El código incluido en el artículo anterior [configuración del código de aplicación](scenario-web-app-sign-user-app-configuration.md) es todo lo que necesita para implementar el cierre de sesión. Una vez que el usuario ha iniciado sesión en la aplicación, probablemente quiera permitirle cerrar la sesión. ASP.NET Core controla cierre de sesión automáticamente.

## <a name="what-sign-out-involves"></a>Qué implica cerrar sesión

Cerrar sesión desde una aplicación web implica más que tan solo quitar la información sobre la cuenta con sesión iniciada del estado de la aplicación web.
La aplicación web también debe redirigir al usuario al punto de conexión `logout` de la Plataforma de identidad de Microsoft para cerrar la sesión. Cuando la aplicación web redirige al usuario a punto de conexión `logout`, este punto de conexión borra la sesión del usuario del explorador. Si la aplicación no ha ido al punto de conexión `logout`, el usuario se volvería a autenticar en su aplicación sin escribir sus credenciales de nuevo, porque tendría una sesión válida de inicio de sesión único con el punto de conexión de la Plataforma de identidad de Microsoft.

Para más información, consulte la sección [Envío de una solicitud de cierre de sesión](v2-protocols-oidc.md#send-a-sign-out-request) en la documentación conceptual de la [Plataforma de identidad de Microsoft y protocolo OpenID Connect](v2-protocols-oidc.md).

## <a name="application-registration"></a>Registro de la aplicación

Durante el registro de la aplicación, habrá registrado un **URI posterior al cierre de sesión**. En este tutorial, ha registrado `https://localhost:44321/signout-oidc` en el campo **URL de cierre de sesión** de la sección **Configuración avanzada** de la página **Autenticación**. Para obtener información detallada, consulte [ Register the webApp app](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp) (Registrar la aplicación webApp).

## <a name="aspnet-core-code"></a>Código ASP.NET Core

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

### <a name="signout-action-of-the-accountcontroller"></a>Acción `Signout()` de `AccountController`

Al presionar el botón **Cerrar sesión** en la aplicación web desencadena la acción `SignOut` en el controlador `Account`. En versiones anteriores de las plantillas ASP.NET Core, el controlador `Account` estaba incrustado en la aplicación web, pero esto ya no es así, ya que ahora forma parte del marco de ASP.NET Core en sí. 

El código para `AccountController` está disponible en el repositorio de ASP.NET Core en [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs). El control de cuentas:

- Establece un URI de redirección de OpenID a `/Account/SignedOut` para volver a llamar al controlador cuando Azure AD haya realizado el cierre de sesión.
- Llama a `Signout()`, que permite que el middleware de OpenIdConnect se ponga en contacto con el punto de conexión `logout` de la Plataforma de identidad de Microsoft, que:

  - Borra la cookie de sesión del explorador.
  - Llama finalmente de nuevo a la **URL de cierre de sesión**, que, de forma predeterminada, muestra la página de vista de cierre de sesión [SignedOut.html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) que también se proporciona como parte de ASP.NET Core.

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>Interceptar la llamada al punto de conexión `logout`

El middleware OpenIdConnect de ASP.NET Core permite a la aplicación interceptar la llamada al punto de conexión `logout` de la Plataforma de identidad de Microsoft al proporcionar un evento de OpenIdConnect denominado `OnRedirectToIdentityProviderForSignOut`. La aplicación web lo usa para intentar evitar que el cuadro de diálogo de selección de cuenta aparezca ante el usuario al cerrar sesión. Esta interceptación se hace en `AddAzureAdV2Authentication` de la biblioteca reutilizable `Microsoft.Identity.Web`. Consulte [StartupHelpers.cs L58-L66](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L58-L66)

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

## <a name="aspnet-code"></a>ASP.NET Code

En ASP.NET, el cierre de sesión se desencadena desde el método SignOut() en un controlador (por ejemplo AccountController). Este método no forma parte del marco de ASP.NET (al contrario de lo que ocurre en ASP.NET Core) y no usa async y, por eso:

- Envía un desafío de cierre de sesión de OpenId
- Elimina la caché
- Redirige a la página que quiere

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

## <a name="protocol"></a>Protocolo

Si no quiere usar ASP.NET Core ni ASP.NET, puede consultar la documentación del protocolo, que está disponible en [Open ID Connect](./v2-protocols-oidc.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paso a producción](scenario-web-app-sign-user-production.md)
