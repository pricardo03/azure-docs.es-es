---
title: 'Aplicación móvil que llama a web API: llamar a una API web | Plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación móvil que llama a las API de Web (llamando a una API Web)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fd65b9f97c373c55a3486e06e83fca7cf824cad
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075121"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Aplicación móvil que llama a web API: llamar a una API web

Una vez que la aplicación ha iniciado sesión un usuario y recibe los tokens, MSAL expone varias piezas de información sobre el usuario, su entorno y los tokens emitidos. La aplicación puede utilizar estos valores para llamar a una API web o mostrar un mensaje de bienvenida a un usuario.

En primer lugar, se explorará el resultado MSAL, a continuación, cómo usar un token de acceso de la `AuthenticationResult` o `result` para llamar a una API web protegida.

## <a name="msal-result"></a>Resultado MSAL

- `AccessToken`: Se usa para llamar a API web protegidas en una solicitud de portador de HTTP.
- `IdToken`: Contiene notificaciones útiles sobre el usuario con sesión iniciada como su nombre, el inquilino principal y el identificador único para el almacenamiento.
- `ExpiresOn`: la fecha de expiración del token. MSAL controla la actualización automática de aplicaciones.
- `TenantId`: El identificador del inquilino del usuario que usó para iniciar sesión. Para los usuarios invitados (Azure AD B2B), este será el inquilino que el usuario ha iniciado sesión con, no su inquilino principal.  
- `Scopes`: los ámbitos que se haya concedido con el token. Esto puede ser un subconjunto de lo que solicita.

Además, MSAL también proporciona una abstracción para un `Account`. Una cuenta representa del usuario actual ha iniciado sesión la cuenta.

- `HomeAccountIdentifier`: El identificador del inquilino principal del usuario.
- `UserName`: Nombre de usuario preferido del usuario. Esto puede estar vacía para los usuarios de Azure AD B2C.
- `AccountIdentifier`: El identificador del usuario con sesión iniciada. Este será el mismo que el `HomeAccountIdentifier` en la mayoría de los casos, a menos que el usuario es un invitado en otro inquilino.

## <a name="calling-an-api"></a>Una llamada a una API

Una vez preparado el token de acceso, es fácil llamar a una API web. La aplicación tomar este token, construir una solicitud HTTP y ejecutarlo.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called graph, process data and send to UI 
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put Access Token in HTTP request 
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

### <a name="ios"></a>iOS

```swift
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)

        // Put Access token in HTTP Request
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")

        URLSession.shared.dataTask(with: request) { data, response, error in
            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }
            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }

            // Successfully got data from Graph
            self.updateLogging(text: "Result from Graph: \(result))")
        }.resume()
```

### <a name="xamarin"></a>Xamarin

```CSharp
httpClient = new HttpClient();

// Put Access token in HTTP request 
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Graph
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

## <a name="making-several-api-requests"></a>Realización de varias solicitudes de API

Si se debe llamar a la misma API varias veces o varias API, existen consideraciones adicionales al compilar la aplicación:

- ***Consentimiento incremental***: Plataforma de identidad de Microsoft permite a las aplicaciones obtener consentimiento del usuario, como permisos son necesarios, en lugar de todo por adelantado. Cada vez que la aplicación está lista para llamar a una API, debe solicitar solo los ámbitos que pretende utilizar.
- ***Acceso condicional***: En determinados escenarios, es posible que obtenga requisitos adicionales de acceso condicional al realizar varias solicitudes de API. Para controlar este escenario, asegúrese de detectar errores de solicitudes silenciosas y prepárese realizar una solicitud interactiva. Esto puede ocurrir si la primera solicitud no tiene aplicado ninguna directiva de acceso condicional y la aplicación intenta acceder en modo silencioso una nueva API que requiera el acceso condicional. Para obtener más información, consulte [orientación para el acceso condicional](conditional-access-dev-guide.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Pasar a producción](scenario-mobile-production.md)
