---
title: 'Aplicación web que permite iniciar sesión a los usuarios (inicio de sesión): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que permita iniciar sesión a los usuarios (inicio de sesión)
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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ed61a09ffc76b4813dcb97330d3a1a436aa16eb
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086470"
---
# <a name="web-app-that-signs-in-users---sign-in"></a>Aplicación web que permite iniciar sesión a los usuarios (inicio de sesión)

Obtenga información sobre cómo agregar el inicio de sesión al código de una aplicación web que permita iniciar sesión a los usuarios.

## <a name="sign-in"></a>Inicio de sesión

El código que se ha revisado en el artículo anterior sobre [configuración del código de aplicación](scenario-web-app-sign-user-app-configuration.md) es todo lo que necesita para implementar el inicio de sesión.
Una vez que el usuario ha iniciado sesión en la aplicación, probablemente quiera permitirle cerrar la sesión. ASP.NET Core controla cierre de sesión automáticamente.

## <a name="what-sign-out-involves"></a>Qué implica cerrar sesión

Cerrar sesión desde una aplicación web implica más que tan solo quitar la información sobre la cuenta con sesión iniciada del estado de la aplicación web.
La aplicación web también debe redirigir al usuario al punto de conexión `logout` de la Plataforma de identidad de Microsoft para cerrar la sesión. Cuando la aplicación web redirige al usuario a punto de conexión `logout`, este punto de conexión borra la sesión del usuario del explorador. Si la aplicación no ha ido al punto de conexión `logout`, el usuario se volvería a autenticar en su aplicación sin escribir sus credenciales de nuevo, porque tendría una sesión válida de inicio de sesión único con el punto de conexión de la Plataforma de identidad de Microsoft.

Para más información, consulte la sección [Envío de una solicitud de cierre de sesión](v2-protocols-oidc.md#send-a-sign-out-request) en la documentación conceptual de la [Plataforma de identidad de Microsoft y protocolo OpenID Connect](v2-protocols-oidc.md).

### <a name="application-registration"></a>Registro de la aplicación

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Durante el registro de la aplicación, habrá registrado un **URI posterior al cierre de sesión**. En este tutorial, ha registrado `https://localhost:44321/signout-oidc` en el campo **URL de cierre de sesión** de la sección **Configuración avanzada** de la página **Autenticación**. Para obtener información detallada, consulte [ Register the webApp app](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp) (Registrar la aplicación webApp).

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Durante el registro de la aplicación, habrá registrado un **URI posterior al cierre de sesión**. En este tutorial, ha registrado `https://localhost:44308/Account/EndSession` en el campo **URL de cierre de sesión** de la sección **Configuración avanzada** de la página **Autenticación**. Para información detallada, consulte [Registrar la aplicación webApp](https://github.com/Azure-Samples/active-directory-dotnet-web-single-sign-out#register-the-service-app-webapp-distributedsignout-dotnet).

# <a name="javatabjava"></a>[Java](#tab/java)

Durante el registro de la aplicación, registrará un **URI posterior al cierre de sesión**. En este tutorial, ha registrado `http://localhost:8080/msal4jsample/` en el campo **URL de cierre de sesión** de la sección **Configuración avanzada** de la página **Autenticación**.

# <a name="pythontabpython"></a>[Python](#tab/python)

Durante el registro de la aplicación, no es necesario registrar una dirección URL de cierre de sesión. El ejemplo no implementa el cierre de sesión global.

---

### <a name="sign-out-button"></a>Botón de cierre de sesión

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

En ASP.NET Core, el botón de cierre de sesión se expone en `Views\Shared\_LoginPartial.cshtml` y solo se muestra cuando hay una cuenta autenticada (es decir, cuando el usuario ha iniciado previamente).

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En ASP.NET MVC, el botón de cierre de sesión se expone en `Views\Shared\_LoginPartial.cshtml` y solo se muestra cuando hay una cuenta autenticada (es decir, cuando el usuario ha iniciado previamente).

```html
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

En el inicio rápido de Java, el botón de cierre de sesión se encuentra en el archivo main/resources/templates/auth_page.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<body>

<form action="/msal4jsample/sign_out">
    <input type="submit" value="Sign out">
</form>
...
```

# <a name="pythontabpython"></a>[Python](#tab/python)

En el inicio rápido de Python, el botón de cierre de sesión se encuentra en el archivo [templates/display.html](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e1199b4c3cdcb637cf0d8306832efbd85492e123/templates/display.html#L18-L20).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Acquire Token Result </title>
</head>
<body>
    {% if cond  %}
        <p1><b>Your information</b> </p1>
        <table>
        {% for key, value in auth_result.items() %}
           <tr>
                <th> {{ key }} </th>
                <td> {{ value }} </td>
           </tr>
        {% endfor %}
        </table>
        <form action="/logout" >
            <input type="submit" value=" Logout"/>
        </form>
    {% else %}
        <p1><b> {{auth_result}} </b> </p1>
        <form action="/authenticate" >
            <input type="submit" value=" Sign-in"/>
        </form>
    {% endif %}
</body>
</html>
```

---

### <a name="signout-action-of-the-controller"></a>Acción `Signout()` del controlador

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

En ASP.NET, al presionar el botón **Cerrar sesión** en la aplicación web desencadena la acción `SignOut` en el controlador `AccountController`. En versiones anteriores de las plantillas ASP.NET Core, el controlador `Account` estaba insertado en la aplicación web, pero esto ya no es así, ya que ahora forma parte del marco de ASP.NET Core en sí.

El código para `AccountController` está disponible en el repositorio de ASP.NET Core en [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs). El control de cuentas:

- Establece un URI de redirección de OpenID a `/Account/SignedOut` para volver a llamar al controlador cuando Azure AD haya realizado el cierre de sesión.
- Llama a `Signout()`, que permite que el middleware de OpenIdConnect se ponga en contacto con el punto de conexión `logout` de la Plataforma de identidad de Microsoft, que:

  - Borra la cookie de sesión del explorador.
  - Llama finalmente de nuevo a la **URL de cierre de sesión**, que, de forma predeterminada, muestra la página de vista de cierre de sesión [SignedOut.html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) que también se proporciona como parte de ASP.NET Core.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En ASP.NET, el cierre de sesión se desencadena desde el método `SignOut()` en un controlador (por ejemplo, AccountController). Este método no forma parte del marco de ASP.NET (lo contrario a lo que sucede en ASP.NET Core). No usa `async`, por lo cual:

- Envía un desafío de cierre de sesión de OpenId
- Elimina la caché
- Redirige a la página que quiere

```CSharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 Response.Redirect("/");
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

En Java, el cierre de sesión se controla llamando directamente al punto de conexión de cierre de sesión de la plataforma de identidad de Microsoft y proporcionando el post_logout_redirect_uri.

```Java
@RequestMapping("/msal4jsample/sign_out")
    public void signOut(HttpServletRequest httpRequest, HttpServletResponse response) throws IOException {

        httpRequest.getSession().invalidate();

        String endSessionEndpoint = "https://login.microsoftonline.com/common/oauth2/v2.0/logout";

        String redirectUrl = "http://localhost:8080/msal4jsample/";
        response.sendRedirect(endSessionEndpoint + "?post_logout_redirect_uri=" +
                URLEncoder.encode(redirectUrl, "UTF-8"));
    }
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
@app.route("/logout")
def logout():
    return flask.redirect(flask.url_for('index'))
```

---

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>Interceptar la llamada al punto de conexión `logout`

El URI posterior al cierre de sesión permite a las aplicaciones participar en el cierre de sesión global.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

El middleware OpenIdConnect de ASP.NET Core permite a la aplicación interceptar la llamada al punto de conexión `logout` de la Plataforma de identidad de Microsoft al proporcionar un evento de OpenIdConnect denominado `OnRedirectToIdentityProviderForSignOut`. Consulte [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156) para ver un ejemplo de cómo suscribirse a este evento (para borrar la caché de tokens).

```CSharp
               // Handling the global sign-out
                options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
                {
                    // Forget about the signed-in user
                };
```

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En ASP.NET, se delega en el middleware la ejecución del cierre de sesión, borrando la cookie de sesión:

```CSharp
public class AccountController : Controller
{
 ...
 public void EndSession()
 {
  Request.GetOwinContext().Authentication.SignOut();
  Request.GetOwinContext().Authentication.SignOut(Microsoft.AspNet.Identity.DefaultAuthenticationTypes.ApplicationCookie);
  this.HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
 }
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

En el inicio rápido de Java, el URI de redireccionamiento posterior al cierre de sesión solo muestra la página index.html. 

# <a name="pythontabpython"></a>[Python](#tab/python)

En el inicio rápido de Python, el URI de redireccionamiento posterior al cierre de sesión solo muestra la página index.html.

---

## <a name="protocol"></a>Protocolo

Si desea más información sobre el cierre de sesión, lea la documentación del protocolo, que está disponible en [Open ID Connect](./v2-protocols-oidc.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paso a producción](scenario-web-app-sign-user-production.md)
