---
title: 'Llamada de la aplicación de demonio web API (adquisición de tokens para la aplicación): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación demonio que llama a web API (adquisición de tokens)
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
ms.openlocfilehash: aa4f5dc7a5aceaf81f71eacd36d131471a57e5c0
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075376"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Aplicación de demonio que llama a web API: adquirir un token

Una vez que se construye la aplicación cliente confidencial, puede adquirir un token para la aplicación mediante una llamada a ``AcquireTokenForClient``, pasando el ámbito y forzar o no una actualización del token.

## <a name="scopes-to-request"></a>Para solicitar los ámbitos

El ámbito de solicitud de un flujo de credenciales de cliente es el nombre del recurso seguido `/.default`. Esta notación indica a Azure AD para usar el **permisos de nivel de aplicación** declarado estáticamente durante el registro de aplicación. Además, tal como se muestra anteriormente, se deben conceder estos permisos de API por un administrador de inquilinos

### <a name="net"></a>.NET

```CSharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

### <a name="python"></a>Python

En MSAL. Python, el archivo de configuración sería similar al siguiente fragmento de código:

```Python
{
    "authority": "https://login.microsoftonline.com/organizations",
    "client_id": "your_client_id",
    "secret": "This is a sample only. You better NOT persist your password."
    "scope": ["https://graph.microsoft.com/.default"]
}
```

### <a name="java"></a>Java

```Java
public final static String KEYVAULT_DEFAULT_SCOPE = "https://vault.azure.net/.default";
```

### <a name="all"></a>Todo

El ámbito usado para las credenciales del cliente siempre debe resourceId + "/ predeterminado"

### <a name="case-of-v10-resources"></a>Caso de los recursos de la versión 1.0

> [!IMPORTANT]
> Para MSAL (punto de conexión v2.0) al solicitar un token de acceso para un recurso que acepta un token de acceso de v1.0, Azure AD analiza la audiencia deseada desde el ámbito solicitado por tomar todo el contenido antes de la última barra y usarlo como el identificador de recurso.
> Por lo tanto, if, como SQL Azure (**https://database.windows.net**) el recurso espera una audiencia termina con una barra diagonal (para SQL Azure: `https://database.windows.net/`), deberá solicitar un ámbito de `https://database.windows.net//.default` (tenga en cuenta la doble barra diagonal). Vea también MSAL.NET problema [#747](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747): Se omite la barra diagonal final de la dirección url de recursos, que produjo el error de autenticación de sql.

## <a name="acquiretokenforclient-api"></a>AcquireTokenForClient API

### <a name="net"></a>.NET

```CSharp
using Microsoft.Identity.Client;

// With client credentials flows the scopes is ALWAYS of the shape "resource/.default", as the
// application permissions need to be set statically (in the portal or by PowerShell), and then granted by
// a tenant administrator
string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

AuthenticationResult result = null;
try
{
 result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
    // The application does not have sufficient permissions
    // - did you declare enough app permissions in during the app creation?
    // - did the tenant admin needs to grant permissions to the application.
}
catch (MsalServiceException ex) when (ex.Message.Contains("AADSTS70011"))
{
    // Invalid scope. The scope has to be of the form "https://resourceurl/.default"
    // Mitigation: change the scope to be as expected !
}
```

#### <a name="application-token-cache"></a>Caché de tokens de aplicación

En MSAL.NET, `AcquireTokenForClient` usa el **caché de tokens de aplicación** (todos los demás métodos AcquireTokenXX utilizan la caché de tokens de usuario) no llame a `AcquireTokenSilent` antes de llamar a `AcquireTokenForClient` como `AcquireTokenSilent` usa el **usuario** memoria caché de tokens. `AcquireTokenForClient` comprueba la **aplicación** propia caché de token y lo actualiza.

### <a name="python"></a>Python

```Python
# Firstly, looks up a token from cache
# Since we are looking for token for the current app, NOT for an end user,
# notice we give account parameter as None.
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])
```

### <a name="java"></a>Java

```Java
ClientCredentialParameters parameters = ClientCredentialParameters
        .builder(Collections.singleton(KEYVAULT_DEFAULT_SCOPE))
        .build();

CompletableFuture<AuthenticationResult> future = cca.acquireToken(parameters);

// You can complete the future in many different ways. Here we use .get() for simplicity
AuthenticationResult result = future.get();
```

### <a name="protocol"></a>Protocol

Si no tiene todavía una biblioteca para el lenguaje elegido, es posible que desea usar el protocolo directamente:

#### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primer caso: solicitud de token de acceso con un secreto compartido

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

#### <a name="second-case-access-token-request-with-a-certificate"></a>Segundo caso: solicitud de token de acceso con un certificado

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

### <a name="learn-more-about-the-protocol"></a>Más información sobre el protocolo

Para obtener más información, consulte la documentación del protocolo: [Flujo de credenciales de Azure Active Directory v2.0 y el cliente de OAuth 2.0](v2-oauth2-client-creds-grant-flow.md).

## <a name="troubleshooting"></a>solución de problemas

### <a name="did-you-use-the-resourcedefault-scope"></a>¿Ha utilizado el ámbito de recursos/predeterminado?

Si recibe un mensaje de error que indica que ha usado un ámbito no válido, probablemente no ha utilizado el `resource/.default` ámbito.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>¿Olvidó proporcionar el consentimiento del administrador? ¡Aplicaciones de demonio necesitan!

Si se produce un error al llamar a la API de **no tiene privilegios suficientes para completar la operación**, el Administrador de inquilinos debe conceder permisos a la aplicación. Vea el paso 6 de registrar la aplicación de cliente anterior.
Normalmente verá y error como la descripción del error siguiente:

```JSon
Failed to call the web API: Forbidden
Content: {
  "error": {
    "code": "Authorization_RequestDenied",
    "message": "Insufficient privileges to complete the operation.",
    "innerError": {
      "request-id": "<guid>",
      "date": "<date>"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: llamar a una API web](scenario-daemon-call-api.md)