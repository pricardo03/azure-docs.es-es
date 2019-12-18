---
title: Configuración de aplicaciones de API web protegidas | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una API web protegida y configurar el código de la aplicación.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: 7f78fa35096b7e17d3736190bfa49619c2c81520
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965405"
---
# <a name="protected-web-api-code-configuration"></a>API web protegida: Configuración del código

Para configurar el código de la API web protegida, deberá entender lo que define que las API sean protegidas, cómo configurar un token de portador y cómo validar el token.

## <a name="what-defines-aspnetaspnet-core-apis-as-protected"></a>¿Qué define que las API de ASP.NET o ASP.NET Core sean protegidas?

Al igual que las aplicaciones web, las API web de ASP.NET o ASP.NET Core están "protegidas" porque las acciones del controlador tienen como prefijo el atributo `[Authorize]`. Por lo tanto, las acciones del controlador se pueden llamar solo si se llama a la API con una identidad que está autorizada.

Tenga en cuenta las preguntas siguientes:

- ¿Cómo conoce la API web la identidad de la aplicación que le llama? (Solo una aplicación puede llamar a una API web).
- Si la aplicación ha llamado a la API web en nombre de un usuario, ¿cuál es la identidad del usuario?

## <a name="bearer-token"></a>Token de portador

La información sobre la identidad de la aplicación y el usuario (a menos que la aplicación web acepte llamadas de servicio a servicio desde una aplicación demonio), se mantiene en el token de portador que se ha establecido en el encabezado cuando se llama a la aplicación.

Este es un ejemplo de código de C# que muestra a un cliente que llama a la API después de adquirir un token con la biblioteca de autenticación de Microsoft para .NET (MSAL.NET):

```CSharp
var scopes = new[] {$"api://.../access_as_user}";
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> Una aplicación cliente ha solicitado el token de portador para el punto de conexión de la plataforma de identidad de Microsoft *para la API web*. La API web es la única aplicación que debe comprobar el token y ver las notificaciones que contiene. Las aplicaciones cliente nunca deben intentar inspeccionar las notificaciones de los tokens. (La API web puede necesitar, en el futuro, que se cifre el token. Este requisito podría impedir el acceso para las aplicaciones cliente que pueden ver los tokens de acceso).

## <a name="jwtbearer-configuration"></a>Configuración de JwtBearer

En esta sección se describe cómo configurar un token de portador.

### <a name="config-file"></a>Archivo config

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line-of-business app).
      Otherwise, you can leave them set to common.
      This can be:
      - A GUID (Tenant ID = Directory ID)
      - 'common' (any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>Inicialización del código

Cuando se llama a una aplicación en una acción de controlador que contiene un atributo `[Authorize]`, ASP.NET o ASP.NET Core examinan el token de portador en el encabezado de autorización de la solicitud que realiza la llamada y extrae el token de acceso. Después, el token se reenvía al middleware JwtBearer, que llama a las extensiones de Microsoft IdentityModel para .NET.

En ASP.NET Core, este middleware se inicializa en el archivo Startup.cs:

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

El middleware se agrega a la API web mediante esta instrucción:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Actualmente, las plantillas de ASP.NET Core crean API web de Azure Active Directory (Azure AD) a las que se conectan usuarios dentro de su organización o de cualquier organización, no con cuentas personales. Pero puede cambiarlas fácilmente para que usen el punto de conexión de la plataforma de identidad de Microsoft mediante la adición de este código al archivo Startup.cs:

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform web API.
    options.Authority += "/v2.0";

    // The web API accepts as audiences both the Client ID (options.Audience) and api://{ClientID}.
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line-of-business apps),
    // we inject our own multitenant validation logic (which even accepts both v1 and v2 tokens).
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;;
});
```

Este fragmento de código se extrae del tutorial incremental de la API web de ASP.NET Core en [Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/154282843da2fc2958fad151e2a11e521e358d42/Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63). El método `AddProtectedWebApi`, que hace mucho más, se llama desde Startup.cs

## <a name="token-validation"></a>Validación de tokens

El middleware JwtBearer, al igual que el middleware OpenID Connect de las aplicaciones web, lo dirige `TokenValidationParameters` con el fin de validar el token. El token se descifra (según sea necesario), se extraen las notificaciones y se comprueba la firma. Después, el middleware valida el token mediante la comprobación de estos datos:

- Está destinada a la API web (público).
- Se ha emitido para una aplicación que puede llamar a la API web (sub).
- Lo ha emitido un servicio de token de seguridad (STS) de confianza (emisor).
- Su duración está dentro del rango (expiración).
- No se ha alterado con (firma).

También puede haber validaciones especiales. Por ejemplo, es posible validar que las claves de firma (cuando se insertan en un token) sean de confianza y que el token no se esté reproduciendo. Por último, algunos protocolos requieren validaciones específicas.

### <a name="validators"></a>Validadores

Los pasos de validación se capturan en los validadores, que están en la biblioteca de código abierto [extensiones de Microsoft IdentityModel para .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet), en un archivo de origen: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs).

En esta tabla se describen los validadores:

| Validador | DESCRIPCIÓN |
|---------|---------|
| `ValidateAudience` | Garantiza que el token es para la aplicación que valida el token (para mí). |
| `ValidateIssuer` | Garantiza que el token lo ha emitido un STS de confianza (de alguien en quien confío). |
| `ValidateIssuerSigningKey` | Garantiza que la aplicación que valida el token confía en la clave que se ha usado para firmar el token. (Caso especial en el que la clave se inserta en el token. Normalmente no es necesario). |
| `ValidateLifetime` | Garantiza que el token todavía es válido (o que ya lo es). El validador comprueba si la duración del token (notificaciones `notbefore` y `expires`) está dentro del rango. |
| `ValidateSignature` | Garantiza que el token no se ha alterado. |
| `ValidateTokenReplay` | Garantiza que el token no se ha reproducido. (Caso especial para algunos protocolos de un solo uso). |

Los validadores están asociados con las propiedades de la clase `TokenValidationParameters`, y se inicializan a partir de la configuración ASP.NET o ASP.NET Core. En la mayoría de los casos, no se tendrán que cambiar los parámetros. Hay una excepción para las aplicaciones que no son inquilinos únicos. (Es decir, aplicaciones web que aceptan usuarios de cualquier organización o de cuentas personales de Microsoft). En este caso, el emisor debe validarse.

## <a name="token-validation-in-azure-functions"></a>Validación de tokens en Azure Functions

También es posible validar los tokens de acceso entrantes en Azure Functions. Puede encontrar ejemplos de validación de tokens en Azure Functions en [Dotnet](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions), [NodeJS](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions)y [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Comprobación de los ámbitos y roles de aplicación en el código](scenario-protected-web-api-verification-scope-app-roles.md)
