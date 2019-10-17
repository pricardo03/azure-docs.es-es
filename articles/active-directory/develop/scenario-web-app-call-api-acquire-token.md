---
title: 'API web que llama a API web (adquisición de un token para la aplicación): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que llame a API web (adquisición de un token para la aplicación).
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
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f30194592989b74aca96a5a483e9128cd3a86eb5
ms.sourcegitcommit: f272ba8ecdbc126d22a596863d49e55bc7b22d37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72274474"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>API web que llama a las API web: adquisición de un token para la aplicación

Ahora que ha compilado su objeto de aplicación cliente, lo usará para adquirir un token para llamar a la API web. En ASP.NET o ASP.NET Core, las llamadas a una API web se hacen en el controlador. Se trata de:

- Obtener un token para la API web mediante la caché de tokens. Para obtener este token, llame a `AcquireTokenSilent`.
- Llamar a la API protegida con el token de acceso.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Los métodos de controlador están protegidos por un atributo `[Authorize]` que obliga a los usuarios a autenticarse para usar la aplicación web. Este es el código que llama a Microsoft Graph.

```CSharp
[Authorize]
public class HomeController : Controller
{
 readonly ITokenAcquisition tokenAcquisition;

 public HomeController(ITokenAcquisition tokenAcquisition)
 {
  this.tokenAcquisition = tokenAcquisition;
 }

 // Code for the controller actions(see code below)

}
```

ASP.NET inserta el servicio `ITokenAcquisition` mediante la inserción de dependencias.


Este es un código simplificado de la acción de HomeController, que obtiene un token para llamar a Microsoft Graph.

```CSharp
public async Task<IActionResult> Profile()
{
 // Acquire the access token
 string[] scopes = new string[]{"user.read"};
 string accessToken = await tokenAcquisition.GetAccessTokenOnBehalfOfUserAsync(scopes);

 // use the access token to call a protected web API
 HttpClient client = new HttpClient();
 client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
 string json = await client.GetStringAsync(url);
}
```

Para entender mejor el código necesario para este escenario, consulte el paso de [2-1-aplicación web llama a Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph) de la fase 2 del tutorial [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial).

Hay varias complejidades adicionales, como:

- Llamada a varias API,
- procesamiento del consentimiento incremental y acceso condicional.

Estos pasos avanzados se procesan en el capítulo 3 del tutorial [3-WebApp-multi-APIs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/3-WebApp-multi-APIs).

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Las cosas son similares en ASP.NET:

- Una acción de controlador protegida por un atributo [Authorize] extrae el identificador de inquilino y el identificador de usuario del miembro `ClaimsPrincipal` del controlador. (ASP.NET usa `HttpContext.User`).
- Desde allí se compila un MSAL.NET `IConfidentialClientApplication`.
- Por útimo, llama al método `AcquireTokenSilent` de la aplicación cliente confidencial.

El código es similar al código que se muestra para ASP.NET Core.

# <a name="javatabjava"></a>[Java](#tab/java)

En el ejemplo de Java, el código que llama a una API está en el método getUsersFromGraph [AuthPageController.java#L62](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L62).

Intenta llamar a `getAuthResultBySilentFlow`. Si el usuario necesita dar su consentimiento a más ámbitos, el código procesa `MsalInteractionRequiredException` para desafiar al usuario.

```java
@RequestMapping("/msal4jsample/graph/users")
    public ModelAndView getUsersFromGraph(HttpServletRequest httpRequest, HttpServletResponse response)
            throws Throwable {

        IAuthenticationResult result;
        ModelAndView mav;
        try {
            result = authHelper.getAuthResultBySilentFlow(httpRequest, response);
        } catch (ExecutionException e) {
            if (e.getCause() instanceof MsalInteractionRequiredException) {

                // If silent call returns MsalInteractionRequired, then redirect to Authorization endpoint
                // so user can consent to new scopes
                String state = UUID.randomUUID().toString();
                String nonce = UUID.randomUUID().toString();

                SessionManagementHelper.storeStateAndNonceInSession(httpRequest.getSession(), state, nonce);

                String authorizationCodeUrl = authHelper.getAuthorizationCodeUrl(
                        httpRequest.getParameter("claims"),
                        "User.ReadBasic.all",
                        authHelper.getRedirectUriGraphUsers(),
                        state,
                        nonce);

                return new ModelAndView("redirect:" + authorizationCodeUrl);
            } else {

                mav = new ModelAndView("error");
                mav.addObject("error", e);
                return mav;
            }
        }
    // Code omitted here.
```

# <a name="pythontabpython"></a>[Python](#tab/python)

En el ejemplo de Python, el código que llama a Microsoft Graph está en [app.py#L53-L62](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L53-L62).

Intenta obtener un token de la memoria caché del token y, a continuación, llama a la API EB después de establecer el encabezado de autorización. Si no es posible, se vuelve a iniciar la sesión del usuario.

```python
@app.route("/graphcall")
def graphcall():
    token = _get_token_from_cache(app_config.SCOPE)
    if not token:
        return redirect(url_for("login"))
    graph_data = requests.get(  # Use token to call downstream service
        app_config.ENDPOINT,
        headers={'Authorization': 'Bearer ' + token['access_token']},
        ).json()
    return render_template('display.html', result=graph_data)
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamada a una API de web](scenario-web-app-call-api-call-api.md)
