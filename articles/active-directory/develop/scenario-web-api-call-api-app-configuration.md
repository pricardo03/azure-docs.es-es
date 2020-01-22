---
title: Configuración de una API web que llama a otras API web | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una API web que llama a otras API web (configuración del código de la aplicación)
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
ms.openlocfilehash: 1e54e26bba1618a3b5835480ed1b1fe698bc8db4
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76043416"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>API web que llama a API web: Configuración del código

Una vez registrada su API web, puede configurar el código de la aplicación.

El código que usa para configurar su API web para que llame a las API web de bajada se basa en el código usado para proteger una API web. Para más información, consulte [API web protegida: configuración de la aplicación](scenario-protected-web-api-app-configuration.md).

## <a name="code-subscribed-to-ontokenvalidated"></a>Código suscrito a OnTokenValidated

Además de la configuración de código para cualquier API web protegida, debe suscribirse a la validación del token de portador que se recibe cuando se llama a su API:

```csharp
/// <summary>
/// Protects the web API with the Microsoft identity platform, or Azure Active Directory (Azure AD) developer platform
/// This supposes that the configuration files have a section named "AzureAD"
/// </summary>
/// <param name="services">The service collection to which to add authentication</param>
/// <param name="configuration">Configuration</param>
/// <returns></returns>
public static IServiceCollection AddProtectedApiCallsWebApis(this IServiceCollection services,
                                                             IConfiguration configuration,
                                                             IEnumerable<string> scopes)
{
    services.AddTokenAcquisition();
    services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        // When an access token for our own web API is validated, we add it 
        // to the MSAL.NET cache so that it can be used from the controllers.
        options.Events = new JwtBearerEvents();

        options.Events.OnTokenValidated = async context =>
        {
            context.Success();

            // Adds the token to the cache and handles the incremental consent 
            // and claim challenges
            AddAccountToCacheFromJwt(context, scopes);
            await Task.FromResult(0);
        };
    });
    return services;
}
```

## <a name="on-behalf-of-flow"></a>Flujo con derechos delegados

El método AddAccountToCacheFromJwt() debe:

- Crear una instancia de una aplicación cliente confidencial de biblioteca de autenticación de Microsoft (MSAL).
- Llame al método `AcquireTokenOnBehalf` . Esta llamada intercambia el token de portador que ha adquirido el cliente para la API web por un token de portador para el mismo usuario, pero hace que la API llame a una API de bajada.

### <a name="instantiate-a-confidential-client-application"></a>Creación de una instancia de una aplicación cliente confidencial

Este flujo solo está disponible en el flujo de cliente confidencial, por lo que la API web protegida proporciona las credenciales del cliente (certificado o secreto de cliente) para la [clase ConfidentialClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder) mediante los métodos `WithClientSecret` o `WithCertificate`.

![Lista de métodos IConfidentialClientApplication](https://user-images.githubusercontent.com/13203188/55967244-3d8e1d00-5c7a-11e9-8285-a54b05597ec9.png)

```csharp
IConfidentialClientApplication app;

#if !VariationWithCertificateCredentials
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .Build();
#else
// Building the client credentials from a certificate
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .Build();
#endif
```

Por último, en lugar de demostrar su identidad mediante un secreto de cliente o un certificado, las aplicaciones cliente confidenciales pueden demostrar su identidad mediante aserciones de cliente.
Para obtener más información acerca de este escenario avanzado, consulte [Aserciones de cliente confidenciales](msal-net-client-assertions.md).

### <a name="how-to-call-on-behalf-of"></a>Cómo llamar con derechos delegados

La llamada con derechos delegados (OBO) se realiza llamando al [método AcquireTokenOnBehalf](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenonbehalfofparameterbuilder) en la interfaz `IConfidentialClientApplication`.

La clase `UserAssertion` se compila a partir del token de portador recibido por la API web desde sus propios clientes. Hay [dos constructores](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientcredential.-ctor?view=azure-dotnet):
* Uno que toma un token de portador JSON Web Token (JWT)
* y otro que toma cualquier tipo de aserción de usuario, otra clase de token de seguridad, cuyo tipo se especifica posteriormente en un parámetro adicional llamado `assertionType`.

![Propiedades y métodos de UserAssertion](https://user-images.githubusercontent.com/13203188/37082180-afc4b708-21e3-11e8-8af8-a6dcbd2dfba8.png)

En la práctica, el flujo con derechos delegados se usa a menudo para adquirir un token para una API de bajada y almacenarlo en la caché de tokens de usuario MSAL.NET. De esta forma, otras partes de la API web pueden llamar posteriormente en las [invalidaciones](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase.acquiretokensilent?view=azure-dotnet) de ``AcquireTokenOnSilent`` para llamar a las API de bajada. Esta llamada tiene el efecto de actualizar los tokens, si es necesario.

```csharp
private void AddAccountToCacheFromJwt(IEnumerable<string> scopes, JwtSecurityToken jwtToken, ClaimsPrincipal principal, HttpContext httpContext)
{
    try
    {
        UserAssertion userAssertion;
        IEnumerable<string> requestedScopes;
        if (jwtToken != null)
        {
            userAssertion = new UserAssertion(jwtToken.RawData, "urn:ietf:params:oauth:grant-type:jwt-bearer");
            requestedScopes = scopes ?? jwtToken.Audiences.Select(a => $"{a}/.default");
        }
        else
        {
            throw new ArgumentOutOfRangeException("tokenValidationContext.SecurityToken should be a JWT Token");
        }

        // Create the application
        var application = BuildConfidentialClientApplication(httpContext, principal);

        // .Result to make sure that the cache is filled in before the controller tries to get access tokens
        var result = application.AcquireTokenOnBehalfOf(requestedScopes.Except(scopesRequestedByMsalNet),
                                                        userAssertion)
                                .ExecuteAsync()
                                .GetAwaiter().GetResult();
     }
     catch (MsalException ex)
     {
         Debug.WriteLine(ex.Message);
         throw;
     }
}
```

También puede ver un ejemplo de implementación del flujo con derechos delegados en [Node.js y Azure Functions](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/MiddleTierAPI/MyHttpTrigger/index.js#L61).

## <a name="protocol"></a>Protocolo

Para más información acerca del protocolo OBO, consulte [Plataforma de identidad de Microsoft y flujo con derechos delegados de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API web que llama a las API web: adquisición de un token para la aplicación](scenario-web-api-call-api-acquire-token.md)
