---
title: Aplicación web que llama a las API web (configuración del código) - Plataforma de identidad de Microsoft
description: Obtenga información sobre cómo compilar una aplicación web que llama a las API web (configuración del código de la aplicación)
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
ms.openlocfilehash: 6c78a951258e3c279f96f44ceac469e4c38cf22c
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785571"
---
# <a name="web-app-that-calls-web-apis---code-configuration"></a>Aplicación web que llama a las API web: configuración de código

Tal como se muestra en el [escenario de usuarios que inician sesión en la aplicación web](scenario-web-app-sign-user-overview.md), dado que el usuario que inicia sesión se delega al middleware de Open ID Connect (OIDC), es recomendable que se conecto con el proceso de OIDC. Esto puede hacerse de distintas maneras según el marco que haya usado (aquí, ASP.NET y ASP.NET Core) pero, en última instancia, se suscribirá a eventos de OIDC de middleware. El principio es el siguiente:

- Permitirá a ASP.NET o ASP.NET Core solicitar un código de autorización. De este modo, ASP.NET o ASP.NET Core permitirán al usuario iniciar sesión y dar su consentimiento.
- Deberá suscribirse a la recepción del código de autorización por parte de la aplicación web.
- Cuando se reciba el código de autenticación, usará las bibliotecas MSAL para canjear el código y los tokens de acceso resultantes y para actualizar el almacén de tokens en la caché de tokens. A partir de entonces, la caché puede utilizarse en otras partes de la aplicación para adquirir otros tokens de forma silenciosa.

## <a name="libraries-supporting-web-app-scenarios"></a>Bibliotecas que admiten escenarios de aplicación web

Las bibliotecas que admiten el flujo de código de autorización para aplicaciones web son las siguientes:

| Biblioteca MSAL | DESCRIPCIÓN |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Las plataformas admitidas son las plataformas .NET Framework y .NET Core (no UWP, Xamarin.iOS ni Xamarin.Android, ya que estas plataformas se usan para compilar aplicaciones cliente públicas) |
| ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Desarrollo en curso: en versión preliminar pública |
| ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Desarrollo en curso: en versión preliminar pública |

## <a name="aspnet-core-configuration"></a>Configuración de ASP.NET Core

En ASP.NET Core, suceden cosas en el archivo `Startup.cs`. Deberá suscribirse al evento de Open ID Connect `OnAuthorizationCodeReceived` y, desde este evento, llamará al método de MSAL.NET `AcquireTokenFromAuthorizationCode` que tiene el efecto de almacenar en la caché de tokens, el token de acceso para los ámbitos solicitados y un token de actualización que se usará para actualizar el token de acceso cuando esté cerca de expirar o para obtener un token en nombre del mismo usuario, pero para un recurso diferente.

Los comentarios en el código siguiente le ayudarán a comprender algunos aspectos complejos de la configuración de MSAL.NET y ASP.NET Core. Se proporcionan detalles completos en el [tutorial incremental sobre aplicaciones web de AsP.NET Core, capítulo 2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph).

```CSharp
  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Response type. We ask ASP.NET to request an Auth Code, and an IDToken
   options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

   // This "offline_access" scope is needed to get a refresh token when users sign in with
   // their Microsoft personal accounts
   // (it's required by MSAL.NET and automatically provided by Azure AD when users
   // sign in with work or school accounts, but not with their Microsoft personal accounts)
   options.Scope.Add(OidcConstants.ScopeOfflineAccess);
   options.Scope.Add("user.read"); // for instance

   // Handling the auth redemption by MSAL.NET so that a token is available in the token cache
   // where it will be usable from Controllers later (through the TokenAcquisition service)
   var handler = options.Events.OnAuthorizationCodeReceived;
   options.Events.OnAuthorizationCodeReceived = async context =>
   {
    // As AcquireTokenByAuthorizationCode is asynchronous we want to tell ASP.NET core
    // that we are handing the code even if it's not done yet, so that it does 
    // not concurrently call the Token endpoint.
    context.HandleCodeRedemption();

    // Call MSAL.NET AcquireTokenByAuthorizationCode
    var application = BuildConfidentialClientApplication(context.HttpContext,
                                                         context.Principal);
    var result = await application.AcquireTokenByAuthorizationCode(scopes.Except(scopesRequestedByMsalNet), context.ProtocolMessage.Code)
                                  .ExecuteAsync();

    // Do not share the access token with ASP.NET Core otherwise ASP.NET will cache it
    // and will not send the OAuth 2.0 request in case a further call to
    // AcquireTokenByAuthorizationCodeAsync in the future for incremental consent 
    // (getting a code requesting more scopes)
    // Share the ID Token so that the identity of the user is known in the application (in 
    // HttpContext.User)
    context.HandleCodeRedemption(null, result.IdToken);

    // Call the previous handler if any
    await handler(context);
   };
```

En ASP.NET Core, la compilación de la aplicación cliente confidencial usa información que se encuentra en HttpContext. Este elemento HttpContext conoce la dirección URL de la aplicación web y el usuario con sesión iniciada (en `ClaimsPrincipal`). También usa la configuración de ASP.NET Core, que tiene una sección "AzureAD" y que está enlazada a la estructura de datos `_applicationOptions`. Por último, la aplicación debe mantener las cachés de tokens.

```CSharp
/// <summary>
/// Creates an MSAL Confidential client application
/// </summary>
/// <param name="httpContext">HttpContext associated with the OIDC response</param>
/// <param name="claimsPrincipal">Identity for the signed-in user</param>
/// <returns></returns>
private IConfidentialClientApplication BuildConfidentialClientApplication(HttpContext httpContext, ClaimsPrincipal claimsPrincipal)
{
 var request = httpContext.Request;

 // Find the URI of the application)
 string currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, azureAdOptions.CallbackPath ?? string.Empty);

 // Updates the authority from the instance (including national clouds) and the tenant
 string authority = $"{azureAdOptions.Instance}{azureAdOptions.TenantId}/";

 // Instantiates the application based on the application options (including the client secret)
 var app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
               .WithRedirectUri(currentUri)
               .WithAuthority(authority)
               .Build();

 // Initialize token cache providers. In the case of Web applications, there must be one
 // token cache per user (here the key of the token cache is in the claimsPrincipal which
 // contains the identity of the signed-in user)
 if (this.UserTokenCacheProvider != null)
 {
  this.UserTokenCacheProvider.Initialize(app.UserTokenCache, httpContext, claimsPrincipal);
 }
 if (this.AppTokenCacheProvider != null)
 {
  this.AppTokenCacheProvider.Initialize(app.AppTokenCache, httpContext);
 }
 return app;
}
```

`AcquireTokenByAuthorizationCode` realmente canjea el código de autorización solicitado por ASP.NET y obtiene los tokens agregados a la caché de tokens de usuario de MSAL.NET. Desde ahí, se utilizan en los controladores de ASP.NET Core.

## <a name="aspnet-configuration"></a>Configuración de ASP.NET Core

ASP.NET controla las cosas de forma similar, con la excepción de que la configuración de OpenIdConnect y la suscripción al evento `OnAuthorizationCodeReceived` se produce en el archivo `App_Start\Startup.Auth.cs`. Encontrará conceptos similares, salvo que aquí deberá especificar el identificador RedirectUri en el archivo de configuración, que es un poco menos sólido:

```CSharp
private void ConfigureAuth(IAppBuilder app)
{
 app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

 app.UseCookieAuthentication(new CookieAuthenticationOptions { });

 app.UseOpenIdConnectAuthentication(
 new OpenIdConnectAuthenticationOptions
 {
  Authority = Globals.Authority,
  ClientId = Globals.ClientId,
  RedirectUri = Globals.RedirectUri,
  PostLogoutRedirectUri = Globals.RedirectUri,
  Scope = Globals.BasicSignInScopes, // a basic set of permissions for user sign in & profile access
  TokenValidationParameters = new TokenValidationParameters
  {
   NameClaimType = "name",
  },
  Notifications = new OpenIdConnectAuthenticationNotifications()
  {
   AuthorizationCodeReceived = OnAuthorizationCodeReceived,
  }
 });
}
```

```CSharp
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
{
 // Upon successful sign in, get & cache a token using MSAL
 string userId = context.AuthenticationTicket.Identity.FindFirst(ClaimTypes.NameIdentifier).Value;
 IConfidentialClientApplication clientapp;
 clientApp = ConfidentialClientApplicationBuilder.Create(Globals.ClientId)
                  .WithClientSecret(Globals.ClientSecret)
                  .WithRedirectUri(Globals.RedirectUri)
                  .WithAuthority(new Uri(Globals.Authority))
                  .Build();

  EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);

 AuthenticationResult result = await clientapp.AcquireTokenByAuthorizationCode(scopes, context.Code)
                                              .ExecuteAsync();
}
```

### <a name="msalnet-token-cache-for-a-aspnet-core-web-app"></a>Caché de tokens MSAL.NET para una aplicación web ASP.NET (Core)

En las aplicaciones web (o API web), la implementación de la caché de tokens es distinta a las implementaciones de la caché de tokens de las aplicaciones de escritorio (que a menudo están [basadas en archivos](scenario-desktop-acquire-token.md#file-based-token-cache)). Puede usar la sesión de ASP.NET/ASP.NET Core, una memoria caché de Redis, una base de datos o incluso Azure Blob Storage. En el fragmento de código anterior, este es el objeto de la llamada al método `EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);`, que enlaza un servicio de caché. Los detalles de lo que sucede aquí quedan fuera del ámbito de esta guía de escenario, pero se proporcionan vínculos a continuación.

> [!IMPORTANT]
> Algo que es importante tener en cuenta es que para las aplicaciones web y las API web, debe haber una caché de tokens por usuario (por cuenta). Es preciso serializar la caché de tokens de cada cuenta.

Hay ejemplos de uso de las cachés de tokens para aplicaciones web y API web disponibles en el [tutorial de aplicaciones web de ASP.NET Core](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial), en la fase [2-2 Token Cache](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache). Para ver las implementaciones, diríjase a la siguiente carpeta [TokenCacheProviders](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web/TokenCacheProviders) de la biblioteca [microsoft-authentication-extensions-for-dotnet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) (en la carpeta [ Microsoft.Identity.Client.Extensions.Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web) carpeta).

## <a name="next-steps"></a>Pasos siguientes

En este momento, cuando el usuario inicia sesión, un token se almacena en la caché de tokens. A continuación, veremos cómo se usa en otras partes de la aplicación web.

> [!div class="nextstepaction"]
> [Inicio de sesión en la aplicación web](scenario-web-app-call-api-sign-in.md)
