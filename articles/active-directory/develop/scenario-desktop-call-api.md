---
title: 'Aplicación de escritorio que llama a las API web: llamada a una API web - Plataforma de identidad de Microsoft'
description: 'Obtener información sobre cómo compilar una aplicación de escritorio que llama a las API web: llamada a una API web'
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
ms.openlocfilehash: 56d3d01e39adfeb6bf2ef5e7e7d595f49c90f5a5
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268275"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Aplicación de escritorio que llama a las API web: llamada a una API web

Ahora que dispone de un token, puede llamar a una API web protegida.

## <a name="calling-a-web-api-from-net"></a>Llamada a una API web desde .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-a-web-api-in-msal-for-ios-and-macos"></a>Llamada a una API web en MSAL para iOS y macOS

Los métodos para adquirir tokens devuelven un objeto `MSALResult`. `MSALResult` expone una propiedad `accessToken` que se puede usar para llamar a una API web. El token de acceso debe agregarse al encabezado de autorización HTTP antes de realizar la llamada para acceder a la API web protegida.

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

## <a name="calling-several-apis---incremental-consent-and-conditional-access"></a>Llamada a varias API: consentimiento incremental y acceso condicional

Si necesita llamar a varias API para el mismo usuario, una vez que obtenga un token para la primera API, simplemente puede llamar a `AcquireTokenSilent` y la mayoría de las veces obtendrá un token para las otras API de forma silenciosa.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Los casos donde se requiere interacción son los siguientes:

- Cuando el usuario ha dado su consentimiento para la primera API, pero ahora debe dar su consentimiento para más ámbitos (consentimiento incremental).
- Cuando la primera API no requería autenticación multifactor, pero la siguiente sí.

```CSharp
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
> [Paso a producción](scenario-desktop-production.md)
