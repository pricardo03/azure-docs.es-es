---
title: API de web - configuración de aplicación de código protegida | Azure
description: Obtenga información sobre cómo compilar una API Web protegida y configurar el código de la aplicación.
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
ms.openlocfilehash: e206cb29338445e30a7462bcbaf0079236e75510
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074971"
---
# <a name="protected-web-api---code-configuration"></a>API - configuración de código web protegida

Para configurar correctamente el código de la API web protegida, deberá comprender lo que hace que las API protegidas, lo que necesita configurar el token de portador y cómo validar el token.

## <a name="what-makes-aspnetaspnet-core-apis-protected"></a>¿Cuáles son las API de Core ASP.NET/ASP.NET protegidas?

Al igual que en las aplicaciones web, el ASP.NET/ASP.NET core web API están "protegidas" porque tienen el prefijo sus acciones de controlador la `[Authorize]` atributo. Por lo tanto, solo se pueden llamar las acciones del controlador si se llama a la API con una identidad que está autorizada.

Tenga en cuenta las siguientes preguntas:

- ¿Cómo sabe la API web de la identidad de la aplicación que lo llama (solo una aplicación puede llamar a una API web)?
- Si la aplicación llama a la API web en nombre de un usuario, ¿qué es la identidad del usuario?

## <a name="bearer-token"></a>Token de portador

La información sobre la identidad de la aplicación y el usuario (a menos que la aplicación web acepta llamadas de servicio a servicio desde una aplicación de demonio), se mantiene en el token de portador que se establece en el encabezado al llamar a la aplicación.

Este es un C# ejemplo de código que se muestra un cliente llama a la API después de adquirir un token con MSAL.NET.

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
> Se solicitó el token de portador por una aplicación cliente para el punto de conexión de plataforma de identidad de Microsoft **para la API web**. La API web es la única aplicación que debe comprobar el token y examine las notificaciones que contiene. Las aplicaciones cliente nunca deben mirar las notificaciones en los tokens (la API web podría decidir, en el futuro, que exige que el token se cifren y esto interrumpirá la aplicación cliente que puede descifrar abrir los tokens de acceso).

## <a name="jwtbearer-configuration"></a>Configuración de JwtBearer

En esta sección cubre lo que necesita configurar el token de portador.

### <a name="config-file"></a>Archivo config

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line of business app)
      Otherwise you can leave them set to common
      This can be:
      - A guid (Tenant ID = Directory ID)
      - 'common' (Any organization and personal accounts)
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

Cuando se llama a la aplicación en la explotación de acción de controlador un `[Authorize]` atributo, core ASP.NET/ASP.NET examina el token de portador en el encabezado de autorización de la solicitud que realiza la llamada y extrae el token de acceso que se reenvía posteriormente a la JwtBearer middleware, que llama a las extensiones del modelo de identidad de Microsoft para. NET.

En ASP.NET Core, este software intermedio se inicializa en el `Startup.cs` archivo.

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

El software intermedio se agrega a la API web mediante la siguiente instrucción:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Actualmente, las plantillas de ASP.NET Core crean las API de web de Azure AD v1.0. Sin embargo, puede cambiar fácilmente para que usen el punto de conexión de plataforma de identidad de Microsoft, agregue el código siguiente en el `Startup.cs` archivo.

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform v2.0 web API
    options.Authority += "/v2.0";

    // The web API accepts as audiences are both the Client ID (options.Audience) and api://{ClientID}
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line of business apps),
    // we inject our own multi-tenant validation logic (which even accepts both V1 and V2 tokens)
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ValidateAadIssuer;
});
```

## <a name="token-validation"></a>Validación de tokens

El software intermedio JwtBearer, al igual que el middleware OpenID Connect en aplicaciones web, se dirige por el `TokenValidationParameters` para validar el token. El token se descifra (si es necesario), las notificaciones se extraen y se comprueba la firma. A continuación, el token se valida mediante la comprobación de los datos siguientes:

- Está destinada a la web API (público)
- Se ha emitido para una aplicación que se puede llamar a la web API (sub)
- Se ha emitido por un control Token de seguridad servidor (STS) (emisor)
- Duración del token está en el intervalo (expiración)
- No se ha manipulado de (firma)

También puede haber validaciones especiales. Por ejemplo, es posible validar que las claves de firma (cuando se insertan en un token) son de confianza y que no se está reproduciendo el token. Por último, algunos protocolos requieren validaciones específicas.

### <a name="validators"></a>Validadores

Los pasos de validación se capturan en los validadores, que están en el [extensión de modelo de identidad de Microsoft para .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) biblioteca de código abierto, en un archivo de origen: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs)

En la tabla siguiente se describen los validadores:

| Control de validación | DESCRIPCIÓN |
|---------|---------|
| `ValidateAudience` | Garantiza que el token para la aplicación que valida el token (para mí). |
| `ValidateIssuer` | Garantiza que el token fue emitido por un STS de confianza (de alguien que confía). |
| `ValidateIssuerSigningKey` | Garantiza que la aplicación valida el token confianzas la clave que se usó para firmar el token (caso especial que la clave se incrusta en el token, normalmente no es necesario). |
| `ValidateLifetime` | Garantiza que el token es válido aún (o ya). Realiza comprobando que la duración del token (`notbefore`, `expires` notificaciones) está en el intervalo. |
| `ValidateSignature` | Garantiza que el token no ha sido alterado. |
| `ValidateTokenReplay` | Garantiza que el token no se reproduce (caso especial para algunos protocolos de uso onetime). |

Los validadores están asociados con las propiedades de la `TokenValidationParameters` clase propios inicializado a partir de la configuración de Core ASP.NET/ASP.NET. En la mayoría de los casos, no tendrá que cambiar los parámetros. Hay una excepción para aplicaciones que no usan inquilinos únicos (es decir, las aplicaciones web que acepten los usuarios de cualquier organización o cuentas personales de Microsoft), en este caso, se debe validar el emisor.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Pasar a producción](scenario-protected-web-api-production.md)
