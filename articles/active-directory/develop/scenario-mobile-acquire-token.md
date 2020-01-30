---
title: Obtención de un token para llamar a una API web (aplicaciones móviles) | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una aplicación móvil que llama a otras API web (para obtener un token para la aplicación).
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
ms.reviwer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 2a86e8352958524bc51b185712d6b60ec347b98e
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702103"
---
# <a name="mobile-app-that-calls-web-apis---get-a-token"></a>Aplicación móvil que llama a las API web: obtener un token

Antes de poder empezar a llamar a las API web protegidas, la aplicación necesitará un token de acceso. Este artículo le guiará a través del proceso para obtener un token usando la biblioteca de autenticación de Microsoft (MSAL).

## <a name="scopes-to-request"></a>Solicitud de ámbitos

Cuando solicite un token, deberá definir un ámbito. El ámbito determina a qué datos puede acceder la aplicación.  

El enfoque más sencillo consiste en combinar el `App ID URI` de la API web deseada con el ámbito `.default`. Al hacerlo, se indica a la plataforma de identidad de Microsoft que su aplicación requiere todos los ámbitos establecidos en el portal.

#### <a name="android"></a>Android
```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
```

#### <a name="ios"></a>iOS
```swift
let scopes = ["https://graph.microsoft.com/.default"]
```

#### <a name="xamarin"></a>Xamarin
```csharp 
var scopes = new [] {"https://graph.microsoft.com/.default"};
```

## <a name="get-tokens"></a>Obtención de tokens

### <a name="acquire-tokens-via-msal"></a>Adquisición de tokens a través de MSAL

MSAL permite que las aplicaciones adquieran tokens de forma silenciosa e interactiva. Simplemente llame a estos métodos y MSAL devolverá un token de acceso para los ámbitos solicitados. El patrón correcto consiste en realizar una solicitud silenciosa y revertir a una solicitud interactiva.

#### <a name="android"></a>Android

```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);

// Check if there are any accounts we can sign in silently.
// Result is in the silent callback (success or error).
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {

        if (accounts.isEmpty() && accounts.size() == 1) {
            // TODO: Create a silent callback to catch successful or failed request.
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts or > 1 account. */
        }
    }
});    

[...]

// No accounts found. Interactively request a token.
// TODO: Create an interactive callback to catch successful or failed request.
sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());        
```

#### <a name="ios"></a>iOS

**En primer lugar, intente adquirir un token silenciosamente:**

Objective-C:

```objc

NSArray *scopes = @[@"https://graph.microsoft.com/.default"];
NSString *accountIdentifier = @"my.account.id";
    
MSALAccount *account = [application accountForIdentifier:accountIdentifier error:nil];
    
MSALSilentTokenParameters *silentParams = [[MSALSilentTokenParameters alloc] initWithScopes:scopes account:account];
[application acquireTokenSilentWithParameters:silentParams completionBlock:^(MSALResult *result, NSError *error) {
        
    if (!error)
    {
        // You'll want to get the account identifier to retrieve and reuse the account
        // for later acquireToken calls
        NSString *accountIdentifier = result.account.identifier;
            
        // Access token to call the Web API
        NSString *accessToken = result.accessToken;
    }
        
    // Check the error
    if (error && [error.domain isEqual:MSALErrorDomain] && error.code == MSALErrorInteractionRequired)
    {
        // Interactive auth will be required, call acquireTokenWithParameters:error:
        return;
    }
}];
```
 
Swift:

```swift

let scopes = ["https://graph.microsoft.com/.default"]
let accountIdentifier = "my.account.id"
        
guard let account = try? application.account(forIdentifier: accountIdentifier) else { return }
let silentParameters = MSALSilentTokenParameters(scopes: scopes, account: account)
application.acquireTokenSilent(with: silentParameters) { (result, error) in
            
    guard let authResult = result, error == nil else {
                
    let nsError = error! as NSError
                
    if (nsError.domain == MSALErrorDomain &&
        nsError.code == MSALError.interactionRequired.rawValue) {
                    
            // Interactive auth will be required, call acquireToken()
            return
         }
         return
     }
            
    // You'll want to get the account identifier to retrieve and reuse the account
    // for later acquireToken calls
    let accountIdentifier = authResult.account.identifier
            
    // Access token to call the Web API
    let accessToken = authResult.accessToken
}
```

**Después, si MSAL devuelve `MSALErrorInteractionRequired`, intente adquirir tokens de forma interactiva:**

Objective-C:

```objc
UIViewController *viewController = ...; // Pass a reference to the view controller that should be used when getting a token interactively
MSALWebviewParameters *webParameters = [[MSALWebviewParameters alloc] initWithParentViewController:viewController];
MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopes webviewParameters:webParameters];
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) {
    if (!error) 
    {
        // You'll want to get the account identifier to retrieve and reuse the account
        // for later acquireToken calls
        NSString *accountIdentifier = result.account.identifier;
            
        NSString *accessToken = result.accessToken;
    }
}];
```

Swift:

```swift
let viewController = ... // Pass a reference to the view controller that should be used when getting a token interactively
let webviewParameters = MSALWebviewParameters(parentViewController: viewController)
let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopes, webviewParameters: webviewParameters)
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in
                
    guard let authResult = result, error == nil else {
        print(error!.localizedDescription)
        return
    }
                
    // Get access token from result
    let accessToken = authResult.accessToken
})
```

MSAL para iOS y macOS admite varios modificadores al obtener un token de forma interactiva o silenciosa.
* [Parámetros comunes al obtener un token](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALTokenParameters.html#/Configuration%20parameters)
* [Parámetros para la adquisición interactiva de tokens](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALInteractiveTokenParameters.html#/Configuring%20MSALInteractiveTokenParameters)
* [Parámetros para la adquisición silenciosa de tokens](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALSilentTokenParameters.html)

#### <a name="xamarin"></a>Xamarin

El ejemplo siguiente muestra un código mínimo para obtener un token de forma interactiva para leer el perfil del usuario con Microsoft Graph.

```csharp
string[] scopes = new string[] {"user.read"};
var app = PublicClientApplicationBuilder.Create(clientId).Build();
var accounts = await app.GetAccountsAsync();
AuthenticationResult result;
try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
             .ExecuteAsync();
}
catch(MsalUiRequiredException)
{
 result = await app.AcquireTokenInteractive(scopes)
             .ExecuteAsync();
}
```

#### <a name="mandatory-parameters-in-msalnet"></a>Parámetros obligatorios en MSAL.NET

`AcquireTokenInteractive` solo tiene un parámetro obligatorio ``scopes``, que contiene una enumeración de cadenas que definen los ámbitos para los que se requiere un token. Si el token es para Microsoft Graph, los ámbitos necesarios se pueden encontrar en la sección denominada "Permisos" en la referencia de API de cada API de Microsoft Graph. Por ejemplo, para [enumerar los contactos del usuario](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts), necesitará usar el ámbito "User.Read", "Contacts.Read". Vea también [Referencia de permisos de Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/permissions_reference).

Si no lo ha especificado al compilar la aplicación, en Android, también deberá especificar la actividad primaria (mediante `.WithParentActivityOrWindow`, vea más abajo) para que el token vuelva a esa actividad principal tras la interacción. Si no se especifica, se producirá una excepción al llamar a `.ExecuteAsync()`.

#### <a name="specific-optional-parameters-in-msalnet"></a>Parámetros opcionales específicos de MSAL.NET

##### <a name="withprompt"></a>WithPrompt

`WithPrompt()` se usa para controlar la interactividad con el usuario especificando un símbolo del sistema.

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

La clase define las constantes siguientes:

- ``SelectAccount``: forzará el STS para que presente el cuadro de diálogo de selección de cuenta que contiene las cuentas para las que el usuario tiene una sesión. Esta opción es útil cuando los desarrolladores de aplicaciones quieren permitir que el usuario elija entre diferentes identidades. Esta opción dirige a MSAL para que envíe ``prompt=select_account`` al proveedor de identidades. Esta opción es el valor predeterminado y funciona bien para proporcionar la mejor experiencia posible en función de la información disponible (cuenta, presencia de una sesión para el usuario, etc. ). No la cambie a menos que tenga buena razón para hacerlo.
- ``Consent``: permite que el desarrollador de la aplicación obligue al usuario a que se le pida consentimiento incluso si ya lo otorgó anteriormente. En este caso, MSAL envía `prompt=consent` al proveedor de identidades. Esta opción se puede usar en algunas aplicaciones centradas en la seguridad donde la gobernanza de la organización exige que se presente al usuario el cuadro de diálogo de consentimiento cada vez que se utiliza la aplicación.
- ``ForceLogin``: permite que el desarrollador de la aplicación haga que el servicio solicite al usuario las credenciales incluso si esta petición al usuario no es necesaria. Esta opción puede ser útil si se produce un error al adquirir un token, para permitir que el usuario vuelva a iniciar sesión. En este caso, MSAL envía `prompt=login` al proveedor de identidades. De nuevo, se ha utilizado en algunas aplicaciones centradas en la seguridad donde la gobernanza de la organización exige que el usuario vuelva a iniciar sesión cada vez que accede a determinadas partes de una aplicación.
- ``Never`` (solo para .NET 4.5 y WinRT) no preguntará al usuario, pero intentará usar en su lugar la cookie almacenada en la vista web incrustada oculta (vea a continuación: Vistas web en MSAL.NET). Esta opción puede producir un error, en cuyo caso `AcquireTokenInteractive` generará una excepción para notificar que es necesaria una interacción de la interfaz de usuario y deberá utilizar otro parámetro `Prompt`.
- ``NoPrompt``: No enviará ninguna solicitud al proveedor de identidades. Esta opción solo es útil para las directivas de perfil de edición de Azure AD B2C (consulte las [características específicas de B2C](https://aka.ms/msal-net-b2c-specificities)).

##### <a name="withextrascopetoconsent"></a>WithExtraScopeToConsent

Este modificador se usa en un escenario avanzado donde quiere que el usuario dé previamente su consentimiento a varios recursos por adelantado (y no quiere utilizar el consentimiento incremental, que normalmente se utiliza con MSAL.NET / la plataforma de identidad de Microsoft v2.0). Para obtener más detalles, vea [Cómo tener el consentimiento del usuario por adelantado para varios recursos](scenario-desktop-production.md#have-the-user-consent-upfront-for-several-resources).

```csharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

##### <a name="other-optional-parameters"></a>Otros parámetros opcionales

Obtenga más información sobre los demás parámetros opcionales para `AcquireTokenInteractive` en la documentación de referencia para [AcquireTokenInteractiveParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods).

### <a name="acquire-tokens-via-the-protocol"></a>Adquisición de tokens mediante el protocolo

No se recomienda usar el protocolo directamente. Si lo hace, la aplicación no admitirá algunos inicios de sesión únicos (SSO), la administración de dispositivos ni los escenarios de acceso condicional.

Cuando use el protocolo para obtener tokens para aplicaciones móviles, deberá realizar dos solicitudes: obtener un código de autorización y cambiarlo por un token.

#### <a name="get-authorization-code"></a>Obtener un código de autorización

```Text
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=<CLIENT_ID>
&response_type=code
&redirect_uri=<ENCODED_REDIRECT_URI>
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2F.default
&state=12345
```

#### <a name="get-access-and-refresh-token"></a>Obtener un token de acceso y actualización

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=<CLIENT_ID>
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=<ENCODED_REDIRECT_URI>
&grant_type=authorization_code
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamada a una API web](scenario-mobile-call-api.md)
