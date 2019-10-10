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
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1131cba204b7b7af33cc0441ee455b6e333aba20
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310079"
---
# <a name="web-app-that-calls-web-apis---code-configuration"></a>Aplicación web que llama a las API web: configuración de código

Como se puede observar en el [escenario de usuarios de inicio de sesión de la aplicación web](scenario-web-app-sign-user-overview.md), la aplicación web usa el [flujo de código de autorización](v2-oauth2-auth-code-flow.md) de OAuth 2.0 para iniciar sesión en el usuario. Este flujo consta de dos partes:

1. Solicitud de un código de autorización. Esta parte delega a la Plataforma de identidad de Microsoft un diálogo privado con el usuario. El usuario inicia sesión y da su consentimiento al uso de las API web. Cuando este diálogo privado finaliza correctamente, la aplicación recibe un código de autorización en el URI de redirección.
1. Solicite un token de acceso para la API mediante el canje del código de autorización.

[El escenario de inicios de sesión de los usuarios en la aplicación web](scenario-web-app-sign-user-overview.md) solo se realiza en el primer segmento. Obtenga información aquí sobre cómo modificar la API web que inicia sesión de los usuarios, de modo que ahora llame a las API web.

## <a name="libraries-supporting-web-app-scenarios"></a>Bibliotecas que admiten escenarios de aplicación web

Las bibliotecas que admiten el flujo de código de autorización para aplicaciones web son las siguientes:

| Biblioteca MSAL | DESCRIPCIÓN |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Las plataformas admitidas son las plataformas .NET Framework y .NET Core (no UWP, Xamarin.iOS ni Xamarin.Android, ya que estas plataformas se usan para compilar aplicaciones cliente públicas) |
| ![MSAL.Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Desarrollo en curso: en versión preliminar pública |
| ![MSAL.Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Desarrollo en curso: en versión preliminar pública |

Seleccione la pestaña correspondiente a la plataforma que le interese:

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Dado que el usuario que inicia sesión se delega al middleware de Open ID Connect (OIDC), es recomendable que se conecte con el proceso de OIDC. La manera de hacerlo es diferente en función de la plataforma que utilice.
En el caso de ASP.NET Core, se suscribirá a los eventos middleware de OIDC. El principio es el siguiente:

- Permite que ASP.NET Core solicite un código de autorización mediante el middleware de Open ID Connect. De este modo, ASP.NET o ASP.NET Core permitirán al usuario iniciar sesión y dar su consentimiento.
- Deberá suscribirse a la recepción del código de autorización por parte de la aplicación web. Esta suscripción se realiza mediante un delegado de C#.
- Cuando se reciba el código de autenticación, usará las bibliotecas MSAL para canjear el código y los tokens de acceso resultantes y para actualizar el almacén de tokens en la caché de tokens. A partir de entonces, la caché puede utilizarse en otras partes de la aplicación, por ejemplo en los controladores, para adquirir otros tokens de forma silenciosa.

Los fragmentos de código de este artículo y los siguientes se extraen del [capítulo 2 del tutorial incremental de aplicaciones web de ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph). Es posible que desee consultar este tutorial para obtener detalles completos de la implementación.

> [!NOTE]
> Para comprender por completo los fragmentos de código que se indican a continuación, debe estar familiarizado con los [aspectos básicos de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals) y, en particular, en la [inserción de dependencias](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) y [opciones](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options).

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Dado que el usuario que inicia sesión se delega al middleware de Open ID Connect (OIDC), es recomendable que se conecte con el proceso de OIDC. La manera de hacerlo es diferente en función de la plataforma que utilice.
En el caso de ASP.NET, se suscribirá a los eventos middleware de OIDC. El principio es el siguiente:

- Permite que ASP.NET Core solicite un código de autorización mediante el middleware de Open ID Connect. De este modo, ASP.NET o ASP.NET Core permitirán al usuario iniciar sesión y dar su consentimiento.
- Deberá suscribirse a la recepción del código de autorización por parte de la aplicación web. Se trata de un delegado de C#.
- Cuando se recibe el código de autenticación, se usarán las bibliotecas de MSAL para canjear el código. Los tokens de acceso y los tokens de actualización resultantes se almacenan en la caché de tokens. A partir de entonces, la caché puede utilizarse en otras partes de la aplicación, por ejemplo en los controladores, para adquirir otros tokens de forma silenciosa.

Los fragmentos de código de este artículo y de los siguientes se han extraído del [ejemplo de aplicación web de ASP.NET](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect). Es posible que desee consultar dicho ejemplo para obtener detalles completos de la implementación.

# <a name="javatabjava"></a>[Java](#tab/java)

Los fragmentos de código de este artículo y los siguientes se han extraído del ejemplo de aplicación web msal4j de [aplicación web de Java que llama a Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp).
El ejemplo permite actualmente que msal4j genere la dirección URL del código de autorización y controla la navegación al punto de conexión de autorización de la plataforma de identidad de Microsoft. También es posible usar la seguridad de Sprint para iniciar la sesión del usuario. Es posible que desee consultar dicho ejemplo para obtener detalles completos de la implementación.

# <a name="pythontabpython"></a>[Python](#tab/python)

Los fragmentos de código de este artículo y los siguientes se extraen del ejemplo de aplicación web MSAL.Python de [aplicación web de Python que llama a Microsoft Graph](https://github.com/Azure-Samples/ms-identity-python-webapp).
El ejemplo permite actualmente que MSAL.Python genere la dirección URL del código de autorización y controla la navegación al punto de conexión de autorización de la plataforma de identidad de Microsoft. Es posible que desee consultar dicho ejemplo para obtener detalles completos de la implementación.

---

## <a name="code-that-redeems-the-authorization-code"></a>El código que canjea el código de autorización.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

### <a name="startupcs"></a>Startup.cs

En ASP.NET Core, el principio es el que está en el archivo `Startup.cs`. Le recomendamos que se suscriba al evento de Open ID Connect `OnAuthorizationCodeReceived` y, desde este, llame al método de MSAL.NET `AcquireTokenFromAuthorizationCode`, que tiene el efecto de almacenar la caché de tokens, el token de acceso para el valor de `scopes` solicitado y un token de actualización que se usará para actualizar el token de acceso cuando esté a punto de expirar, o para obtener un token en nombre del mismo usuario, pero para un recurso diferente.

En la práctica, el [tutorial de la aplicación web de ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) intenta proporcionarle código reutilizable para las aplicaciones web.

Este es el código [Startup.cs#L40-L42](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/2-WebApp-graph-user/2-1-Call-MSGraph/Startup.cs#L40-L42), que incluye la llamada al método `AddMicrosoftIdentityPlatformAuthentication` que agrega autenticación a la aplicación web y a `AddMsal`, que agrega la capacidad de llamar a las API web. La llamada a `AddInMemoryTokenCaches` se refiere a la elección de una implementación de caché de token entre las que son posibles:

```CSharp
public class Startup
{
  // Code not show here

  public void ConfigureServices(IServiceCollection services)
  {
      // Token acquisition service based on MSAL.NET
      // and chosen token cache implementation
      services.AddMicrosoftIdentityPlatformAuthentication(Configuration)
          .AddMsal(Configuration, new string[] { Constants.ScopeUserRead })
          .AddInMemoryTokenCaches();
  }

  // Code not show here
}
```

`Constants.ScopeUserRead` se define en [Constants.cs#L5](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/2-WebApp-graph-user/2-1-Call-MSGraph/Infrastructure/Constants.cs#L5).

```CSharp
public static class Constants
{
    public const string ScopeUserRead = "User.Read";
}
```

Ya ha estudiado el contenido de `AddMicrosoftIdentityPlatformAuthentication` en [Aplicación web que inicia la sesión de los usuarios: configuración de código](scenario-web-app-sign-user-app-configuration.md?tabs=aspnetcore#initialization-code).

### <a name="the-addmsal-method"></a>Método AddMsal

El código de `AddMsal` se localiza en [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L108-L159](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L108-L159).

```CSharp

/// <summary>
/// Extensions for IServiceCollection for startup initialization.
/// </summary>
public static class WebAppServiceCollectionExtensions
{
  // Code omitted here

  /// <summary>
  /// Add MSAL support to the Web App or Web API
  /// </summary>
  /// <param name="services">Service collection to which to add authentication</param>
  /// <param name="initialScopes">Initial scopes to request at sign-in</param>
  /// <returns></returns>
  public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
  {
      // Ensure that configuration options for MSAL.NET, HttpContext accessor and the Token acquisition service
      // (encapsulating MSAL.NET) are available through dependency injection
      services.Configure<ConfidentialClientApplicationOptions>(options => configuration.Bind(configSectionName, options));
      services.AddHttpContextAccessor();
      services.AddTokenAcquisition();

      services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
      {
          // Response type
          options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

          // This scope is needed to get a refresh token when users sign-in with their Microsoft personal accounts
          // (it's required by MSAL.NET and automatically provided when users sign-in with work or school accounts)
          options.Scope.Add("offline_access");
          if (initialScopes != null)
          {
              foreach (string scope in initialScopes)
              {
                  if (!options.Scope.Contains(scope))
                  {
                      options.Scope.Add(scope);
                  }
              }
          }

          // Handling the auth redemption by MSAL.NET so that a token is available in the token cache
          // where it will be usable from Controllers later (through the TokenAcquisition service)
          var handler = options.Events.OnAuthorizationCodeReceived;
          options.Events.OnAuthorizationCodeReceived = async context =>
          {
              var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
              await tokenAcquisition.AddAccountToCacheFromAuthorizationCodeAsync(context, options.Scope).ConfigureAwait(false);
              await handler(context).ConfigureAwait(false);
          };
      });
      return services;
  }
}
```

El método `AddMsal` garantiza que:

- La aplicación web de ASP.NET Core solicita un IDToken para el usuario y un código de autenticación (`options.ResponseType = OpenIdConnectResponseType.CodeIdToken`).
- Se agrega el ámbito `offline_access`. Es necesario para que el usuario consienta que la aplicación obtenga un token de actualización.
- La aplicación se suscribe al evento `OnAuthorizationCodeReceived` de OIDC y canjea la llamada con MSAL.NET, que se encapsula aquí en un componente reutilizable que implementa `ITokenAcquisition`.

### <a name="the-tokenacquisitionaddaccounttocachefromauthorizationcodeasync-method"></a>Método TokenAcquisition. AddAccountToCacheFromAuthorizationCodeAsync

El método `TokenAcquisition.AddAccountToCacheFromAuthorizationCodeAsync` se encuentra en [Microsoft.Identity.Web/TokenAcquisition.cs#L101-L145](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L101-L145). Garantiza que:

- ASP.NET no intenta canjear el código de autenticación en paralelo a MSAL.NET (`context.HandleCodeRedemption();`).
- Las notificaciones de IDToken están disponibles para que MSAL procese una clave de caché de token para la cuenta del usuario.
- Se crea una instancia de la aplicación MSAL.NET si es necesario.
- La aplicación MSAL.NET canjea el código.
- El nuevo token de identificador se comparte con ASP.NET Core (durante la llamada a `context.HandleCodeRedemption(null, result.IdToken);`). El token de acceso no se comparte con ASP.NET Core. Permanece en la caché del token de MSAL.NET asociada al usuario, donde está listo para usarse en los controladores de ASP.NET Core.

```csharp
public class TokenAcquisition : ITokenAcquisition
{
  string[] scopesRequestedByMsalNet = new string[]{ "openid", "profile", "offline_access" };

  // Code omitted here for clarity


  public async Task AddAccountToCacheFromAuthorizationCodeAsync(AuthorizationCodeReceivedContext context, IEnumerable<string> scopes)
  {
   // Code omitted here for clarity

    try
    {
      // As AcquireTokenByAuthorizationCodeAsync is asynchronous we want to tell ASP.NET core that we are handing the code
      // even if it's not done yet, so that it does not concurrently call the Token endpoint. (otherwise there will be a
      // race condition ending-up in an error from Azure AD telling "code already redeemed")
      context.HandleCodeRedemption();

      // The cache will need the claims from the ID token.
      // If they are not yet in the HttpContext.User's claims, so adding them here.
      if (!context.HttpContext.User.Claims.Any())
      {
          (context.HttpContext.User.Identity as ClaimsIdentity).AddClaims(context.Principal.Claims);
      }

      var application = GetOrBuildConfidentialClientApplication();

      // Do not share the access token with ASP.NET Core otherwise ASP.NET will cache it and will not send the OAuth 2.0 request in
      // case a further call to AcquireTokenByAuthorizationCodeAsync in the future is required for incremental consent (getting a code requesting more scopes)
      // Share the ID Token though
      var result = await application
          .AcquireTokenByAuthorizationCode(scopes.Except(_scopesRequestedByMsalNet), context.ProtocolMessage.Code)
          .ExecuteAsync()
          .ConfigureAwait(false);

      context.HandleCodeRedemption(null, result.IdToken);
  }
  catch (MsalException ex)
  {
      Debug.WriteLine(ex.Message);
      throw;
  }
 }
```

### <a name="the-tokenacquisitionbuildconfidentialclientapplication-method"></a>Método TokenAcquisition. BuildConfidentialClientApplication

En ASP.NET Core, la compilación de la aplicación cliente confidencial usa información que se encuentra en HttpContext. Con acceso mediante la propiedad `CurrentHttpContext`, el elemento HttpContext, asociado a la solicitud, conoce la dirección URL de la aplicación web y el usuario que inició sesión (en un `ClaimsPrincipal`). `BuildConfidentialClientApplication` también usa la configuración de ASP.NET Core, que tiene una sección "AzureAD" y que está enlazada a lo siguiente:

- la estructura de datos `_applicationOptions` de tipo [ConfidentialClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationoptions?view=azure-dotnet)
- la instancia `azureAdOptions` de tipo [AzureAdOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/AzureADOptions.cs) definida en `Authentication.AzureAD.UI` de ASP.NET Core Por último, la aplicación debe mantener las cachés de tokens. Aprenderá más sobre esto en la sección siguiente.

El código para el método `GetOrBuildConfidentialClientApplication()` está en [Microsoft.Identity.Web/TokenAcquisition.cs#L290-L333](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L290-L333). Usa miembros que se insertaron por las dependencias (se pasaron en el constructor de TokenAcquisition en [Microsoft.Identity.Web/TokenAcquisition.cs#L47-L59](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L47-L59)).

```CSharp
public class TokenAcquisition : ITokenAcquisition
{
  // Code omitted here for clarity

  // Members
  private IConfidentialClientApplication application;
  private HttpContext CurrentHttpContext => _httpContextAccessor.HttpContext;

  // The following members are set by dependency injection in the TokenAcquisition constructor
  private readonly AzureADOptions _azureAdOptions;
  private readonly ConfidentialClientApplicationOptions _applicationOptions;
  private readonly IMsalAppTokenCacheProvider _appTokenCacheProvider;
  private readonly IMsalUserTokenCacheProvider _userTokenCacheProvider;
  private readonly IHttpContextAccessor _httpContextAccessor;

  /// <summary>
  /// Creates an MSAL Confidential client application if needed
  /// </summary>
  private IConfidentialClientApplication GetOrBuildConfidentialClientApplication()
  {
    if (application == null)
    {
        application = BuildConfidentialClientApplication();
    }
    return application;
  }

  /// <summary>
  /// Creates an MSAL Confidential client application
  /// </summary>
  /// <param name="claimsPrincipal"></param>
  /// <returns></returns>
  private IConfidentialClientApplication BuildConfidentialClientApplication()
  {
    var request = CurrentHttpContext.Request;
    var azureAdOptions = _azureAdOptions;
    var applicationOptions = _applicationOptions;
    string currentUri = UriHelper.BuildAbsolute(
        request.Scheme,
        request.Host,
        request.PathBase,
        azureAdOptions.CallbackPath ?? string.Empty);

    string authority = $"{applicationOptions.Instance}{applicationOptions.TenantId}/";

    var app = ConfidentialClientApplicationBuilder
        .CreateWithApplicationOptions(applicationOptions)
        .WithRedirectUri(currentUri)
        .WithAuthority(authority)
        .Build();

    // Initialize token cache providers
    _appTokenCacheProvider?.InitializeAsync(app.AppTokenCache);
    _userTokenCacheProvider?.InitializeAsync(app.UserTokenCache);

    return app;
  }

```

### <a name="summary"></a>Resumen

Para resumir, `AcquireTokenByAuthorizationCode` realmente canjea el código de autorización solicitado por ASP.NET y obtiene los tokens agregados a la caché de tokens de usuario de MSAL.NET. Desde ahí, se utilizan en los controladores de ASP.NET Core.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET controla las cosas de forma similar, a ASP.NET Core, con la excepción de que la configuración de OpenIdConnect y la suscripción al evento `OnAuthorizationCodeReceived` se produce en el archivo [App_Start\Startup.Auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs). Encontrará conceptos similares a los de ASP.NET Core, salvo que en ASP.NET deberá especificar el identificador RedirectUri en el archivo [Web.config#L15](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/master/WebApp/Web.config#L15). Esta configuración es un poco más robusta que lo que se hace en ASP.NET Core, ya que deberá cambiarla al implementar la aplicación.

```CSharp
public partial class Startup
{
  public void ConfigureAuth(IAppBuilder app)
  {
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    // Custom middleware initialization. This is activated when the code obtained from a code_grant is present in the querystring (&code=<code>).
    app.UseOAuth2CodeRedeemer(
        new OAuth2CodeRedeemerOptions
        {
            ClientId = AuthenticationConfig.ClientId,
            ClientSecret = AuthenticationConfig.ClientSecret,
            RedirectUri = AuthenticationConfig.RedirectUri
        }
      );

  app.UseOpenIdConnectAuthentication(
      new OpenIdConnectAuthenticationOptions
      {
        // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
        Authority = AuthenticationConfig.Authority,
        ClientId = AuthenticationConfig.ClientId,
        RedirectUri = AuthenticationConfig.RedirectUri,
        PostLogoutRedirectUri = AuthenticationConfig.RedirectUri,
        Scope = AuthenticationConfig.BasicSignInScopes + " Mail.Read", // a basic set of permissions for user sign in & profile access "openid profile offline_access"
        TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = false,
            // In a real application you would use IssuerValidator for additional checks, like making sure the user's organization has signed up for your app.
            //     IssuerValidator = (issuer, token, tvp) =>
            //     {
            //        //if(MyCustomTenantValidation(issuer))
            //        return issuer;
            //        //else
            //        //    throw new SecurityTokenInvalidIssuerException("Invalid issuer");
            //    },
            //NameClaimType = "name",
        },
        Notifications = new OpenIdConnectAuthenticationNotifications()
        {
            AuthorizationCodeReceived = OnAuthorizationCodeReceived,
            AuthenticationFailed = OnAuthenticationFailed,
        }
      });
  }
  
  private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
  {
      // Upon successful sign in, get the access token & cache it using MSAL
      IConfidentialClientApplication clientApp = MsalAppBuilder.BuildConfidentialClientApplication(new ClaimsPrincipal(context.AuthenticationTicket.Identity));
      AuthenticationResult result = await clientApp.AcquireTokenByAuthorizationCode(new[] { "Mail.Read" }, context.Code).ExecuteAsync();
  }
  
  private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
  {
      notification.HandleResponse();
      notification.Response.Redirect("/Error?message=" + notification.Exception.Message);
      return Task.FromResult(0);
  }
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

Consulte [Aplicación web que inicia la sesión de los usuarios: configuración del código](scenario-web-app-sign-user-app-configuration.md?tabs=java#initialization-code) para comprender cómo el ejemplo de Java obtiene el código de autorización. Una vez recibida por la aplicación, [AuthFilter.java#L51-L56](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java#L51-L56) se delega en el método `AuthHelper.processAuthenticationCodeRedirect` de [AuthHelper.java#L67-L97](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L67-L97) y, después, llama a `getAuthResultByAuthCode`:

```Java
class AuthHelper {
  // code omitted
  void processAuthenticationCodeRedirect(HttpServletRequest httpRequest, String currentUri, String fullUrl)
            throws Throwable {

  // code omitted
  AuthenticationResponse authResponse = AuthenticationResponseParser.parse(new URI(fullUrl), params);

  // code omitted
  IAuthenticationResult result = getAuthResultByAuthCode(
                    httpRequest,
                    oidcResponse.getAuthorizationCode(),
                    currentUri);

// code omitted
  }
}
```

El método `getAuthResultByAuthCode` se define en [AuthHelper.java#L176](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L176). Crea un MSAL `ConfidentialClientApplication` y llama a `acquireToken()` con `AuthorizationCodeParameters` creado a partir del código de autorización.

```Java
   private IAuthenticationResult getAuthResultByAuthCode(
            HttpServletRequest httpServletRequest,
            AuthorizationCode authorizationCode,
            String currentUri) throws Throwable {

        IAuthenticationResult result;
        ConfidentialClientApplication app;
        try {
            app = createClientApplication();

            String authCode = authorizationCode.getValue();
            AuthorizationCodeParameters parameters = AuthorizationCodeParameters.builder(
                    authCode,
                    new URI(currentUri)).
                    build();

            Future<IAuthenticationResult> future = app.acquireToken(parameters);

            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        }

        if (result == null) {
            throw new ServiceUnavailableException("authentication result was null");
        }

        SessionManagementHelper.storeTokenCacheInSession(httpServletRequest, app.tokenCache().serialize());

        return result;
    }

    private ConfidentialClientApplication createClientApplication() throws MalformedURLException {
        return ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.create(clientSecret)).
                authority(authority).
                build();
    }
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Cuando se solicita el flujo de código de autorización, como se aprecia en [Aplicación web que inicia la sesión de los usuarios: configuración del código](scenario-web-app-sign-user-app-configuration.md?tabs=python#initialization-code), el código de autorización se recibe en la función `authorized` que Flask enruta desde la dirección URL de /getAToken. Consulte [app.py#L30-L44](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L30-L44).

```python
 @app.route("/getAToken")  # Its absolute URL must match your app's redirect_uri set in AAD
def authorized():
    if request.args['state'] != session.get("state"):
        return redirect(url_for("login"))
    cache = _load_cache()
    result = _build_msal_app(cache).acquire_token_by_authorization_code(
        request.args['code'],
        scopes=app_config.SCOPE,  # Misspelled scope would cause an HTTP 400 error here
        redirect_uri=url_for("authorized", _external=True))
    if "error" in result:
        return "Login failure: %s, %s" % (
            result["error"], result.get("error_description"))
    session["user"] = result.get("id_token_claims")
    _save_cache(cache)
    return redirect(url_for("index"))
```

---

En lugar de un secreto de cliente, la aplicación cliente confidencial también puede demostrar su identidad mediante un certificado de cliente o una aserción de cliente.
El uso de aserciones de cliente es un escenario avanzado, que se detalla en [aserciones de cliente](msal-net-client-assertions.md).

## <a name="token-cache"></a>Memoria caché de tokens

> [!IMPORTANT]
> En las aplicaciones web (o API web), la implementación de la caché de tokens es distinta a las implementaciones de la caché de tokens de las aplicaciones de escritorio (que a menudo están [basadas en archivos](scenario-desktop-acquire-token.md#file-based-token-cache)).
> Por motivos de seguridad y rendimiento, es importante asegurar que, para las aplicaciones web y las API web, debe haber una caché de tokens por usuario (por cuenta). Es preciso serializar la caché de tokens de cada cuenta.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

En el tutorial de ASP.NET Core se usa la inserción de dependencias para permitir decidir la implementación de la caché de tokens en el archivo Startup.cs de la aplicación. Microsoft.Identity.Web incluye varios serializadores de caché de tokens pregenerados que se describen en [Serialización del almacenamiento en caché de los tokens](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/README.md#token-cache-serialization). Una posibilidad interesante es elegir las [cachés de memoria distribuidas](https://docs.microsoft.com/aspnet/core/performance/caching/distributed#distributed-memory-cache) de ASP.NET Core:

```csharp
// or use a distributed Token Cache by adding
    services.AddMicrosoftIdentityPlatformAuthentication(Configuration)
            .AddMsal(new string[] { scopesToRequest })
            .AddDistributedTokenCaches();

// and then choose your implementation

// For instance the distributed in memory cache (not cleared when you stop the app)
services.AddDistributedMemoryCache()

// Or a Redis cache
services.AddStackExchangeRedisCache(options =>
{
 options.Configuration = "localhost";
 options.InstanceName = "SampleInstance";
});

// Or even a SQL Server token cache
services.AddDistributedSqlServerCache(options =>
{
 options.ConnectionString = _config["DistCache_ConnectionString"];
 options.SchemaName = "dbo";
 options.TableName = "TestCache";
});
```

Para más información sobre los proveedores de caché de tokens, consulte la fase [tutoriales de aplicación web de ASP.NET Core | Cachés de tokens](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache) del tutorial.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En las aplicaciones web (o API web), la implementación de la caché de tokens es distinta a las implementaciones de la caché de tokens de las aplicaciones de escritorio (que a menudo están [basadas en archivos](scenario-desktop-acquire-token.md#file-based-token-cache)). Puede usar la sesión ASP.NET o la memoria del servidor. Vea por ejemplo cómo se enlaza la implementación de la memoria caché después de la creación de la aplicación de MSAL.NET en [MsalAppBuilder.cs#L39-L51](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Utils/MsalAppBuilder.cs#L39-L51).

```csharp
public static class MsalAppBuilder
{
 // Omitted code
    public static IConfidentialClientApplication BuildConfidentialClientApplication(ClaimsPrincipal currentUser)
    {
      IConfidentialClientApplication clientapp = ConfidentialClientApplicationBuilder.Create(AuthenticationConfig.ClientId)
            .WithClientSecret(AuthenticationConfig.ClientSecret)
            .WithRedirectUri(AuthenticationConfig.RedirectUri)
            .WithAuthority(new Uri(AuthenticationConfig.Authority))
            .Build();
  
      // After the ConfidentialClientApplication is created, we overwrite its default UserTokenCache with our implementation
      MSALPerUserMemoryTokenCache userTokenCache = new MSALPerUserMemoryTokenCache(clientapp.UserTokenCache, currentUser ?? ClaimsPrincipal.Current);
  
      return clientapp;
  }
```

# <a name="javatabjava"></a>[Java](#tab/java)

msal4j proporciona métodos para serializar y deserializar la caché de tokens. En el ejemplo de Java se controla la serialización de la sesión, como se muestra en el método `getAuthResultBySilentFlow` de [AuthHelper.java#L99-L122](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L99-L122).

```Java
IAuthenticationResult getAuthResultBySilentFlow(HttpServletRequest httpRequest, HttpServletResponse httpResponse)
      throws Throwable {

  IAuthenticationResult result =  SessionManagementHelper.getAuthSessionObject(httpRequest);

  IConfidentialClientApplication app = createClientApplication();

  Object tokenCache = httpRequest.getSession().getAttribute("token_cache");
  if (tokenCache != null) {
      app.tokenCache().deserialize(tokenCache.toString());
  }

  SilentParameters parameters = SilentParameters.builder(
          Collections.singleton("User.ReadBasic.All"),
          result.account()).build();

  CompletableFuture<IAuthenticationResult> future = app.acquireTokenSilently(parameters);
  IAuthenticationResult updatedResult = future.get();

  //update session with latest token cache
  SessionManagementHelper.storeTokenCacheInSession(httpRequest, app.tokenCache().serialize());

  return updatedResult;
}
```

Los detalles de la clase `SessionManagementHelper` se proporcionan en [](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/SessionManagementHelper.java).

# <a name="pythontabpython"></a>[Python](#tab/python)

En el ejemplo de Python, se garantiza la creación de una aplicación cliente confidencial para cada solicitud y su serialización en la memoria caché de la sesión de Flask:

```python
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)

# Code omitted here for simplicity

def _load_cache():
    cache = msal.SerializableTokenCache()
    if session.get("token_cache"):
        cache.deserialize(session["token_cache"])
    return cache

def _save_cache(cache):
    if cache.has_state_changed:
        session["token_cache"] = cache.serialize()

def _build_msal_app(cache=None):
    return msal.ConfidentialClientApplication(
        app_config.CLIENT_ID, authority=app_config.AUTHORITY,
        client_credential=app_config.CLIENT_SECRET, token_cache=cache)
```

---

## <a name="next-steps"></a>Pasos siguientes

En este momento, cuando el usuario inicia sesión, un token se almacena en la caché de tokens. A continuación, veremos cómo se usa en otras partes de la aplicación web.

> [!div class="nextstepaction"]
> [Inicio de sesión en la aplicación web](scenario-web-app-call-api-sign-in.md)
