---
title: API web que llama a las API web de nivel inferior (configuración del código de la aplicación) - Plataforma de identidad de Microsoft
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
ms.openlocfilehash: 27b95b82f996368bca312be1c6ada25a7219b66e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562292"
---
# <a name="web-api-that-calls-web-apis---code-configuration"></a>API web que llama a otras API web: configuración de código

Una vez registrada su API web, puede configurar el código de la aplicación.

El código para configurar su API web para que llame a las API web de nivel inferior se basa en el código usado para proteger una API web. Para más información, consulte [API web protegida: configuración de la aplicación](scenario-protected-web-api-app-configuration.md).

## <a name="code-subscribed-to-ontokenvalidated"></a>Código suscrito a OnTokenValidated

Además de la configuración de código para cualquier API web protegida, debe suscribirse a la validación del token de portador que se recibe cuando se llama a su API:

```CSharp
/// <summary>
/// Protects the web API with Microsoft Identity Platform (a.k.k AAD v2.0)
/// This supposes that the configuration files have a section named "AzureAD"
/// </summary>
/// <param name="services">Service collection to which to add authentication</param>
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
        // to MSAL.NET's cache so that it can be used from the controllers.
        options.Events = new JwtBearerEvents();

        options.Events.OnTokenValidated = async context =>
        {
            context.Success();

            // Adds the token to the cache, and also handles the incremental consent 
            // and claim challenges
            AddAccountToCacheFromJwt(context, scopes);
            await Task.FromResult(0);
        };
    });
    return services;
}
```

## <a name="on-behalf-of-flow"></a>Flujos en nombre de

El método AddAccountToCacheFromJwt() debe:

- Crear una instancia de una aplicación cliente confidencial de MSAL.
- Llamar a `AcquireTokenOnBehalf` para intercambiar el token de portador que ha adquirido el cliente para la API web por un token de portador para el mismo usuario, pero para que nuestra API llame a una API de nivel inferior.

### <a name="instantiate-a-confidential-client-application"></a>Creación de una instancia de una aplicación cliente confidencial

Este flujo solo está disponible en el flujo de cliente confidencial, por lo que la API web protegida proporciona las credenciales del cliente (certificado o secreto de cliente) para [ConfidentialClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder) mediante los métodos `WithClientSecret` o `WithCertificate`, respectivamente.

![imagen](https://user-images.githubusercontent.com/13203188/55967244-3d8e1d00-5c7a-11e9-8285-a54b05597ec9.png)

```CSharp
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

Por último, en lugar de un secreto de cliente o un certificado, las aplicaciones cliente confidenciales también pueden demostrar su identidad mediante aserciones de cliente.
Este escenario avanzado se detalla en las [aserciones de cliente](msal-net-client-assertions.md).

### <a name="how-to-call-on-behalf-of"></a>Cómo llamar con derechos delegados

La llamada con derechos delegados (OBO) se realiza llamando al método [AcquireTokenOnBehalf](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenonbehalfofparameterbuilder) en la interfaz `IConfidentialClientApplication`.

`UserAssertion` se compila a partir del token de portador recibido por la API web desde sus propios clientes. Hay [dos constructores](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientcredential.-ctor?view=azure-dotnet), uno que toma un token de portador JWT y otro que toma cualquier tipo de aserción de usuario (otra clase de token de seguridad, cuyo tipo se especifica posteriormente en un parámetro adicional llamado `assertionType`).

![imagen](https://user-images.githubusercontent.com/13203188/37082180-afc4b708-21e3-11e8-8af8-a6dcbd2dfba8.png)

En la práctica, el flujo OBO a menudo se usa para adquirir un token para una API de nivel inferior y almacenarlo en la caché de tokens de usuario de MSAL.NET para que otras partes de la API web puedan llamar posteriormente en las [invalidaciones](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase.acquiretokensilent?view=azure-dotnet) de ``AcquireTokenOnSilent`` para llamar a las API de nivel inferior. Esta llamada tiene el efecto de actualizar los tokens, si es necesario.

```CSharp
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

  // .Result to make sure that the cache is filled-in before the controller tries to get access tokens
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

## <a name="protocol"></a>Protocolo

Para más información acerca del protocolo de derechos delegados, consulte [Plataforma de identidad de Microsoft y flujo con derechos delegados de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquisición de un token para la aplicación](scenario-web-api-call-api-acquire-token.md)
