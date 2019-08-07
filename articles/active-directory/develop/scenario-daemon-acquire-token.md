---
title: 'Aplicación de demonio que llama a las API web (adquisición de tokens para la aplicación): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación de demonio que llame a las API web (adquisición de tokens)
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
ms.openlocfilehash: 6a5f15aa5264c0abf87cb15f0468e8a3a924e0b5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562357"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Aplicación de demonio que llama a las API web: adquisición de un token

Una vez que se ha construido la aplicación cliente confidencial, puede adquirir un token para la aplicación mediante una llamada a ``AcquireTokenForClient``, pasando el ámbito y forzando o no una actualización del token.

## <a name="scopes-to-request"></a>Solicitud de ámbitos

El ámbito que va a solicitar para un flujo de credenciales de cliente es el nombre del recurso seguido de `/.default`. Esta notación indica a Azure AD que use los **permisos de nivel de aplicación** declarados estáticamente durante el registro de la aplicación. Además, tal como se vio anteriormente, un administrador de inquilinos deben conceder estos permisos de API.

### <a name="net"></a>.NET

```CSharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

### <a name="python"></a>Python

En MSAL.Python, el archivo de configuración sería similar al siguiente fragmento de código:

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

El ámbito usado para las credenciales del cliente siempre deben ser idRecurso+"/.default"

### <a name="case-of-azure-ad-v10-resources"></a>Caso de recursos de Azure AD (v1.0)

> [!IMPORTANT]
> Si MSAL (punto de conexión de la Plataforma de identidad de Microsoft) pide un token de acceso para un recurso que acepta tokens de la versión 1.0, Azure AD analiza la audiencia deseada desde el ámbito solicitado tomando todo el contenido antes de la última barra diagonal y usándolo como el identificador del recurso.
> Por lo tanto, si, al igual que SQL Azure ( **https://database.windows.net** ), el recurso espera una audiencia que termina con una barra diagonal (para SQL Azure: `https://database.windows.net/` ), deberá solicitar un ámbito de `https://database.windows.net//.default` (tenga en cuenta la doble barra diagonal). Consulte también el problema [747](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747) de MSAL.NET: falta la barra diagonal final de la dirección URL del recurso, lo que ha provocado un error de autenticación de SQL.

## <a name="acquiretokenforclient-api"></a>API AcquireTokenForClient

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

En MSAL.NET, `AcquireTokenForClient` usa la **caché de tokens de aplicación** (todos los demás métodos AcquireTokenXX usan la caché de tokens de usuario). No llame a `AcquireTokenSilent` antes de llamar a `AcquireTokenForClient`, ya que `AcquireTokenSilent` usa la caché de tokens de **usuario**. `AcquireTokenForClient` comprueba la propia caché de tokens de **aplicación** y la actualiza.

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

### <a name="protocol"></a>Protocolo

Si no tiene todavía una biblioteca para el lenguaje elegido, es posible que quiera usar el protocolo directamente:

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

Para más información, consulte la documentación del protocolo: [La Plataforma de identidad de Microsoft y el flujo de credenciales de cliente de OAuth 2.0](v2-oauth2-client-creds-grant-flow.md)

## <a name="troubleshooting"></a>solución de problemas

### <a name="did-you-use-the-resourcedefault-scope"></a>¿Ha usado el ámbito de recurso/.default?

Si recibe un mensaje de error que indica que ha usado un ámbito no válido, probablemente no ha usado el ámbito `resource/.default`.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>¿Olvidó proporcionar el consentimiento del administrador? Las aplicaciones de demonio lo necesitan.

Si se produce un error **No tiene privilegios suficientes para completar la operación** al llamar a la API, el administrador de inquilinos debe conceder permisos a la aplicación. Consulte el paso 6 del registro de la aplicación de cliente anterior.
Normalmente verá un error con una descripción de error como la siguiente:

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
> [Llamada de demonio: llamar a una API web](scenario-daemon-call-api.md)