---
title: 'Uso avanzado de la autenticación y autorización: Azure App Service | Microsoft Docs'
description: Muestra cómo personalizar la autenticación y autorización en App Service y obtener las notificaciones de usuario y los diferentes tokens.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/08/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: f3e30309b230ec44ddf39648b943f3f76dc7805d
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53722658"
---
# <a name="advanced-usage-of-authentication-and-authorization-in-azure-app-service"></a>Uso avanzado de la autenticación y autorización en Azure App Service

En este artículo se muestra cómo personalizar la [autenticación y autorización integradas en App Service](overview-authentication-authorization.md) y cómo administrar la identidad desde la aplicación. 

Para comenzar inmediatamente, consulte uno de los siguientes tutoriales:

* [Tutorial: Autenticación y autorización de usuarios de un extremo a otro en Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)
* [Tutorial: Autenticación y autorización de usuarios de un extremo a otro en Azure App Service para Linux](containers/tutorial-auth-aad.md)
* [Configuración de la aplicación para usar el inicio de sesión de Azure Active Directory](configure-authentication-provider-aad.md)
* [Configuración de la aplicación para usar el inicio de sesión de Facebook](configure-authentication-provider-facebook.md)
* [Configuración de la aplicación para usar el inicio de sesión de Google](configure-authentication-provider-google.md)
* [Configuración de la aplicación para usar el inicio de sesión de la cuenta Microsoft](configure-authentication-provider-microsoft.md)
* [Configuración de la aplicación para usar el inicio de sesión de Twitter](configure-authentication-provider-twitter.md)

## <a name="use-multiple-sign-in-providers"></a>Uso de varios proveedores de inicio de sesión

La configuración del portal no ofrece una manera preparada para presentar varios proveedores de inicio de sesión a los usuarios (por ejemplo, Facebook y Twitter). Sin embargo, no es difícil agregar la funcionalidad a la aplicación. A continuación se describen los pasos necesarios:

Primero, en la página **Autenticación / Autorización** de Azure Portal, configure cada uno de los proveedores de identidades que desea habilitar.

En **Action to take when request is not authenticated** (Acción necesaria cuando la solicitud no está autenticada), seleccione **Allow Anonymous requests (no action)** (Permitir solicitudes anónimas (sin acción)).

En la página de inicio de sesión, en la barra de navegación o en cualquier otra ubicación de la aplicación, agregue un vínculo de inicio de sesión a cada uno de los proveedores que ha habilitado (`/.auth/login/<provider>`). Por ejemplo: 

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Cuando el usuario haga clic en uno de los vínculos, la página de inicio de sesión correspondiente se abrirá para que el usuario inicie sesión.

Para redirigir al usuario después del inicio de sesión a una dirección URL personalizada, use el parámetro de cadena de consulta `post_login_redirect_url` (que no debe confundirse con el URI de redireccionamiento den su configuración de proveedor de identidades). Por ejemplo, para redirigir al usuario a `/Home/Index` después de iniciar sesión, utilice el siguiente código HTML:

```HTML
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="validate-tokens-from-providers"></a>Validar los tokens de los proveedores

En un inicio de sesión que dirige el cliente, la aplicación consigue de forma manual que el cliente inicie sesión en el proveedor y envía el token de autenticación a App Service para su validación (consulte el [flujo de autenticación](overview-authentication-authorization.md#authentication-flow)). Esta validación en sí misma no le otorga acceso a los recursos de aplicación que quiere, pero una validación exitosa le dará un token de sesión que puede usar para obtener acceso a los recursos de la aplicación. 

Para validar el token del proveedor, la aplicación App Service debe configurarse primero con el proveedor que quiera usar. En el entorno de ejecución, después de recuperar el token de autenticación de su proveedor, publique el token en `/.auth/login/<provider>` para su validación. Por ejemplo:  

```
POST https://<appname>.azurewebsites.net/.auth/login/aad HTTP/1.1
Content-Type: application/json

{"id_token":"<token>","access_token":"<token>"}
```

El formato del token varía ligeramente según el proveedor. Consulte la tabla siguiente para más detalles:

| Valor del proveedor | Necesario en el cuerpo de la solicitud | Comentarios |
|-|-|-|
| `aad` | `{"access_token":"<access_token>"}` | |
| `microsoftaccount` | `{"access_token":"<token>"}` | La propiedad `expires_in` es opcional. <br/>Al solicitar el token de los servicios Live, solicite siempre el ámbito `wl.basic`. |
| `google` | `{"id_token":"<id_token>"}` | La propiedad `authorization_code` es opcional. Cuando se especifica, también puede ir acompañado opcionalmente por la propiedad `redirect_uri`. |
| `facebook`| `{"access_token":"<user_access_token>"}` | Use un [token de acceso de usuario](https://developers.facebook.com/docs/facebook-login/access-tokens) válido de Facebook. |
| `twitter` | `{"access_token":"<access_token>", "access_token_secret":"<acces_token_secret>"}` | |
| | | |

Si el token del proveedor se valida correctamente, la API regresa con un elemento `authenticationToken` en el cuerpo de la respuesta, que es su token de sesión. 

```json
{
    "authenticationToken": "...",
    "user": {
        "userId": "sid:..."
    }
}
```

Una vez que tenga este token de sesión, puede obtener acceso a los recursos de aplicación protegidos agregando el encabezado `X-ZUMO-AUTH` a sus solicitudes HTTP. Por ejemplo:  

```
GET https://<appname>.azurewebsites.net/api/products/1
X-ZUMO-AUTH: <authenticationToken_value>
```

## <a name="sign-out-of-a-session"></a>Cierre de sesión de una sesión

Los usuarios pueden iniciar un cierre de sesión mediante el envío de una solicitud `GET` al punto de conexión de la aplicación `/.auth/logout`. La solicitud `GET` hace lo siguiente:

- Borra las cookies de autenticación de la sesión actual.
- Elimina los tokens del usuario actual del almacén de tokens.
- Para Azure Active Directory y Google, realiza un cierre de sesión del servidor en el proveedor de identidades.

A continuación, se muestra un vínculo de cierre de sesión simple en una página web:

```HTML
<a href="/.auth/logout">Sign out</a>
```

De forma predeterminada, un cierre de sesión correcto redirige el cliente a la dirección URL `/.auth/logout/done`. Puede cambiar la página de redirección del inicio de sesión posterior agregando el parámetro de consulta `post_logout_redirect_uri`. Por ejemplo: 

```
GET /.auth/logout?post_logout_redirect_uri=/index.html
```

Se recomienda [codificar](https://wikipedia.org/wiki/Percent-encoding) el valor de `post_logout_redirect_uri`.

Al utilizar direcciones URL completas, la dirección URL debe hospedarse en el mismo dominio o configurarse como una dirección URL de redirección externa permitida para la aplicación. En el ejemplo siguiente, para redirigir a `https://myexternalurl.com` que no se hospedado en el mismo dominio:

```
GET /.auth/logout?post_logout_redirect_uri=https%3A%2F%2Fmyexternalurl.com
```

Debe ejecutar el siguiente comando en [Azure Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp auth update --name <app_name> --resource-group <group_name> --allowed-external-redirect-urls "https://myexternalurl.com"
```

## <a name="preserve-url-fragments"></a>Conservación de los fragmentos de dirección URL

Después de que los usuarios inician sesión en la aplicación, normalmente desean redirigirse a la misma sección de la misma página, como `/wiki/Main_Page#SectionZ`. Sin embargo, dado que los [fragmentos de dirección URL](https://wikipedia.org/wiki/Fragment_identifier) (por ejemplo, `#SectionZ`) nunca se envían al servidor, no se conservan de forma predeterminada después de que el inicio de sesión de OAuth se complete y se redirija a la aplicación. Los usuarios obtienen una experiencia poco óptima cuando necesitan volver a navegar al delimitador deseado. Esta limitación se aplica a todas las soluciones de autenticación del servidor.

En la autenticación de App Service, puede conservar los fragmentos de dirección URL en el inicio de sesión de OAuth. Para ello, establezca un valor de aplicación denominado `WEBSITE_AUTH_PRESERVE_URL_FRAGMENT` en `true`. Puede hacerlo el [Azure Portal](https://portal.azure.com), o simplemente ejecute el siguiente comando en [Azure Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group <group_name> --settings WEBSITE_AUTH_PRESERVE_URL_FRAGMENT="true"
```

## <a name="access-user-claims"></a>Acceso a las notificaciones de usuario

App Service pasa las notificaciones de usuario a la aplicación mediante encabezados especiales. Las solicitudes externas no están autorizadas para establecer estos encabezados, por lo que solo están presentes si App Service los establece. A continuación puede ver algunos encabezados de ejemplo:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

El código escrito en cualquier lenguaje o plataforma puede obtener la información que necesita de estos encabezados. Para las aplicaciones de ASP.NET 4.6, **ClaimsPrincipal** se establece automáticamente con los valores adecuados.

La aplicación también puede obtener detalles adicionales sobre el usuario autenticado mediante una llamada a `/.auth/me`. Los SDK del servidor de Mobile Apps proporcionan métodos de asistente para trabajar con estos datos. Para más información, consulte [Uso del SDK de Node.js de Azure Mobile Apps](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity) y [Trabajar con el SDK del servidor back-end de .NET para Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Recuperación de los tokens en el código de aplicación

Desde el código de servidor, los tokens específicos del proveedor se inyectan en el encabezado de solicitud, por lo que puede acceder a ellos fácilmente. En la siguiente tabla se muestran los posibles nombres de encabezado de token:

| Proveedor | Nombres de encabezados |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Token de Facebook | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Cuenta Microsoft | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

Desde el código de cliente (por ejemplo, una aplicación móvil o JavaScript en el explorador), envíe la solicitud `GET` HTTP a `/.auth/me`. El JSON devuelto tiene los tokens específicos del proveedor.

> [!NOTE]
> Los tokens de acceso son para acceder a recursos de proveedor, por lo que solo están presentes si se configura el proveedor con un secreto de cliente. Para ver cómo obtener tokens de actualización, vea [Tokens de acceso de actualización](#refresh-access-tokens).

## <a name="refresh-access-tokens"></a>Tokens de acceso de actualización

Cuando el token de acceso del proveedor expira, debe volver a autenticar el usuario. Puede evitar la expiración del token mediante la realización de una llamada `GET` al punto de conexión `/.auth/refresh` de la aplicación. Cuando se llama, App Service actualiza automáticamente los tokens de acceso en el almacén de tokens para el usuario autenticado. Las solicitudes posteriores para los tokens por código de aplicación obtienen los tokens actualizados. Sin embargo, para que la actualización de token funcione, el almacén de tokens debe contener [tokens de actualización](https://auth0.com/learn/refresh-tokens/) para el proveedor. Cada proveedor documenta la manera de obtener tokens de actualización, pero en la lista siguiente se muestra un breve resumen:

- **Google**: anexe un parámetro de cadena de consulta `access_type=offline` en su llamada API `/.auth/login/google`. Si usa el SDK de Mobile Apps, puede agregar el parámetro a una de las sobrecargas `LogicAsync` (vea [Google Refresh Tokens](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens) (Tokens de actualización de Google)).
- **Facebook**: no proporciona tokens de actualización. Los tokens de larga duración expiran en 60 días (vea [Facebook Expiration and Extension of Access Tokens](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension) (Expiración y extensión de tokens de acceso de Facebook)).
- **Twitter**: los tokens de acceso no expiran [vea [Twitter OAuth FAQ](https://developer.twitter.com/en/docs/basics/authentication/FAQ) (Preguntas más frecuentes sobre Twitter OAuth)].
- **Cuenta Microsoft**: cuando [defina la configuración de autenticación de Cuenta Microsoft](configure-authentication-provider-microsoft.md), seleccione el ámbito `wl.offline_access`.
- **Azure Active Directory**: en [https://resources.azure.com](https://resources.azure.com), siga estos pasos:
    1. En la parte superior de la página, seleccione **Lectura y escritura**.
    2. En el explorador de la izquierda, desplácese hasta **subscriptions** > **_\<nombre\_suscripción_** > **resourceGroups** > _**\<nombre\_grupo\_recursos>**_ > **providers** > **Microsoft.Web** > **sites** > _**\<nombre\_aplicación>**_ > **config** > **authsettings**. 
    3. Haga clic en **Editar**.
    4. Modifique la siguiente propiedad. Reemplace _\<app\_id>_ por el identificador de aplicación de Azure Active Directory del servicio al que desea acceder.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    5. Haga clic en **Put**. 

Una vez configurado el proveedor, puede [buscar el token de actualización y el tiempo de expiración para el token de acceso](#retrieve-tokens-in-app-code) en el almacén de tokens. 

Para actualizar el token de acceso en cualquier momento, simplemente llame a `/.auth/refresh` en cualquier lenguaje. El fragmento de código siguiente utiliza jQuery para actualizar los tokens de acceso de un cliente de JavaScript.

```JavaScript
function refreshTokens() {
  var refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Si un usuario revoca los permisos concedidos a la aplicación, la llamada a `/.auth/me` puede producir un error con un respuesta `403 Forbidden`. Para diagnosticar errores, compruebe los registros de aplicación para más detalles.

## <a name="extend-session-expiration-grace-period"></a>Ampliación del período de gracia de expiración de sesión

Después de que expire una sesión autenticada, hay un período de gracia de 72 horas de forma predeterminada. Dentro de este período de gracia, puede actualizar la cookie de sesión o el token de sesión con App Service sin volver a autenticar el usuario. Simplemente puede llamar a `/.auth/refresh` cuando la cookie de sesión o el token de sesión deje de ser válido y no es necesario que realice un seguimiento usted mismo de la expiración del token. Una vez que transcurra el período de gracia de 72 horas, el usuario debe iniciar sesión de nuevo para obtener una cookie de sesión o un token de sesión válido.

Si 72 horas no es tiempo suficiente para usted, puede ampliar este período de expiración. Extender la expiración durante un período prolongado podría tener implicaciones de seguridad importantes (por ejemplo, cuando un token de autenticación se pierde o roba). Por tanto, debe dejarla en el valor predeterminado de 72 horas o establecer el período de extensión en el valor más pequeño.

Para extender el período de expiración predeterminado, ejecute el siguiente comando en [Cloud Shell](../cloud-shell/overview.md).

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> El período de gracia solo se aplica a la sesión autenticada de App Service, no a los tokens de los proveedores de identidades. No hay ningún período de gracia para los tokens del proveedor expirados. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Limitación del dominio de cuentas de inicio de sesión

Cuenta Microsoft y Azure Active Directory le permiten iniciar sesión desde varios dominios. Por ejemplo, Cuenta Microsoft permite cuentas de _outlook.com_, _live.com_ y _hotmail.com_. Azure Active Directory permite cualquier número de dominios personalizados para las cuentas de inicio de sesión. Este comportamiento puede no ser deseable para una aplicación interna, a la que no desea que cualquier persona con una cuenta de _outlook.com_ acceda. Para limitar el nombre de dominio de las cuentas de inicio de sesión, siga estos pasos.

En [https://resources.azure.com](https://resources.azure.com), desplácese hasta **subscriptions** > **_\<nombre\_suscripción_** > **resourceGroups** > _**\<nombre\_grupo\_recursos>**_ > **providers** > **Microsoft.Web** > **sites** > _**\<nombre\_aplicación>**_ > **config** > **authsettings**. 

Haga clic en **Editar**, modifique la propiedad siguiente y luego haga clic en **Put**. No olvide reemplazar _\<domain\_name>_ por el dominio que desee.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```
## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Autenticación y autorización de usuarios de un extremo a otro (Windows)](app-service-web-tutorial-auth-aad.md)
> [Tutorial: Autenticación y autorización de usuarios de un extremo a otro (Linux)](containers/tutorial-auth-aad.md)