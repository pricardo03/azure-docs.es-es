---
title: Llamada a una API web desde una aplicación móvil | Azure
titleSuffix: Microsoft identity platform
description: 'Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web: llamada a una API web'
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
ms.openlocfilehash: 6b87809e29940b343a395ffb461c0829295fcd8a
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702069"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Aplicación móvil que llama a las API web: llamada a una API web

Después de que la aplicación haya iniciado la sesión de un usuario y recibido los tokens, MSAL expone alguna información sobre el usuario, el entorno del usuario y los tokens emitidos. La aplicación puede utilizar estos valores para llamar a una API web o mostrar un mensaje de bienvenida al usuario.

En primer lugar, observaremos el resultado de MSAL. Después, veremos cómo usar un token de acceso de `AuthenticationResult` o `result` para llamar a una API web protegida.

## <a name="msal-result"></a>Resultado de MSAL
MSAL proporciona los valores siguientes: 

- `AccessToken`: Se usa para llamar a API web protegidas en una solicitud de portador de HTTP.
- `IdToken`: Contiene información útil sobre el usuario que inició sesión, como el nombre del usuario, el inquilino principal y un identificador único para el almacenamiento.
- `ExpiresOn`: El tiempo de expiración del token. MSAL controla la actualización automática para las aplicaciones.
- `TenantId`: El identificador del inquilino con el que el usuario ha iniciado sesión. Para los usuarios invitados (Azure Active Directory B2B), este valor identificará al inquilino con el que el usuario ha iniciado sesión, no con el inquilino principal del usuario.  
- `Scopes`: Los ámbitos que se hayan concedido con el token. Los ámbitos concedidos pueden ser un subconjunto de los ámbitos que ha solicitado.

MSAL también proporciona una abstracción para `Account`. `Account` representa la cuenta de usuario con sesión iniciada actual.

- `HomeAccountIdentifier`: El identificador del inquilino principal del usuario.
- `UserName`: El nombre de usuario preferido del usuario. Este campo podría estar vacío para los usuarios de Azure Active Directory B2C.
- `AccountIdentifier`: El identificador del usuario que ha iniciado sesión. En la mayoría de los casos, este valor será el mismo que el valor `HomeAccountIdentifier`, a menos que el usuario sea un invitado de otro inquilino.

## <a name="call-an-api"></a>Llamar a una API

Una vez que tiene el token de acceso, es fácil llamar a una API web. La aplicación usará el token para construir una solicitud HTTP y después ejecutar la solicitud.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing.
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called Graph. Process data and send to UI.
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error.
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put access token in HTTP request.
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="msal-for-ios-and-macos"></a>MSAL para iOS y macOS

Los métodos para adquirir tokens devuelven un objeto `MSALResult`. `MSALResult` expone una propiedad `accessToken` que se puede usar para llamar a una API web. El token de acceso debe agregarse al encabezado de autorización HTTP, antes de realizar la llamada para acceder a la API web protegida.

Objective-C:

```objc
NSMutableURLRequest *urlRequest = [NSMutableURLRequest new];
urlRequest.URL = [NSURL URLWithString:"https://contoso.api.com"];
urlRequest.HTTPMethod = @"GET";
urlRequest.allHTTPHeaderFields = @{ @"Authorization" : [NSString stringWithFormat:@"Bearer %@", accessToken] };
        
NSURLSessionDataTask *task =
[[NSURLSession sharedSession] dataTaskWithRequest:urlRequest
     completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {}];
[task resume];
```

Swift:

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]
     
let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

### <a name="xamarin"></a>Xamarin

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

## <a name="making-several-api-requests"></a>Realización de varias solicitudes de API

Si tiene que llamar varias veces a la misma API, o si debe llamar a varias API, tenga en cuenta lo siguiente al compilar la aplicación:

- **Consentimiento incremental**: La plataforma de identidad de Microsoft permite que las aplicaciones obtengan el consentimiento del usuario a medida que se necesitan los permisos, en lugar de todos al principio. Cada vez que la aplicación está lista para llamar a una API, debe solicitar solo los ámbitos que debe usar.
- **Acceso condicional**: En determinados escenarios, podría tener requisitos adicionales de acceso condicional al realizar varias solicitudes de API. Esto puede ocurrir si la primera solicitud no tiene aplicada ninguna directiva de acceso condicional y la aplicación intenta acceder en modo silencioso a una nueva API que requiere acceso condicional. Para controlar este escenario, asegúrese de que detecta los errores de solicitudes silenciosas y prepárese para realizar una solicitud interactiva.  Para obtener más información, vea [Orientación para desarrolladores para el acceso condicional de Azure Active Directory](conditional-access-dev-guide.md).

## <a name="calling-several-apis-in-xamarin-or-uwp---incremental-consent-and-conditional-access"></a>Llamada a varias API en Xamarin o UWP: consentimiento incremental y acceso condicional

Si necesita llamar a varias API para el mismo usuario, una vez que haya adquirido un token para un usuario, puede evitar solicitar repetidamente las credenciales del usuario mediante una llamada a `AcquireTokenSilent` para obtener un token.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Los casos donde se requiere interacción son los siguientes:

- Cuando el usuario ha dado su consentimiento para la primera API, pero ahora debe dar su consentimiento para más ámbitos (consentimiento incremental).
- Cuando la primera API no requería autenticación multifactor, pero la siguiente sí.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paso a producción](scenario-mobile-production.md)
