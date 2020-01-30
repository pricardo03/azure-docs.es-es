---
title: 'Flujo de concesión implícita de OAuth 2.0: plataforma de identidad de Microsoft | Azure'
description: Proteja las aplicaciones de página única con el flujo implícito de la plataforma de identidad de Microsoft.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 42d315b44a76e79d6f1db48e5024094099564a98
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76700488"
---
# <a name="microsoft-identity-platform-and-implicit-grant-flow"></a>Plataforma de identidad de Microsoft y concesión implícita de flujo

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Con el punto de conexión de la plataforma de identidad de Microsoft, puede iniciar la sesión de los usuarios en las aplicaciones de página única tanto con cuentas Microsoft personales como profesionales o educativas. Las aplicaciones de página única y otras aplicaciones JavaScript que se ejecutan principalmente en un explorador se enfrentan con algunos retos interesantes en lo que a la autenticación se refiere:

* Las características de seguridad de estas aplicaciones son significativamente diferentes de las de las aplicaciones web tradicionales basadas en el servidor.
* Muchos proveedores de identidades y servidores de autorización no admiten solicitudes CORS.
* Los redireccionamientos del explorador a una página completa fuera de la aplicación se vuelven especialmente invasivos para la experiencia del usuario.

Para estas aplicaciones (AngularJS, Ember.js, React.js, etc.), la plataforma de identidad de Microsoft admite el flujo de concesión implícita de OAuth 2.0. El flujo implícito se describe en la [especificación de OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.2). Su principal ventaja es que hace posible que la aplicación obtenga tokens de la plataforma de identidad de Microsoft sin necesidad de realizar un intercambio de credenciales con el servidor back-end. De esta forma, la aplicación puede iniciar la sesión del usuario, mantenerla y recibir tokens para otras API web, y todo dentro del código de cliente de JavaScript. Existen algunas consideraciones de seguridad importantes que se deben tener en cuenta al usar el flujo implícito, en concreto en lo referente a la suplantación de identidad del [cliente](https://tools.ietf.org/html/rfc6749#section-10.3) y el [usuario](https://tools.ietf.org/html/rfc6749#section-10.3).

En este artículo se describe cómo programar directamente con el protocolo de la aplicación.  Cuando sea posible, se recomienda usar las bibliotecas de autenticación de Microsoft (MSAL) admitidas, en lugar de [adquirir tokens y API web protegidas por llamadas](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows).  Además, eche un vistazo a las [aplicaciones de ejemplo que usan MSAL](sample-v2-code.md).

Pero si prefiere no usar una biblioteca en la aplicación de página única y enviar mensajes de protocolo usted mismo, siga estos pasos generales.

> [!NOTE]
> No todas las características y escenarios de Azure Active Directory (Azure AD) son compatibles con el punto de conexión de la plataforma de identidad de Microsoft. Para determinar si debe usar el punto de conexión de la plataforma de identidad de Microsoft, conozca las [limitaciones de dicha plataforma](active-directory-v2-limitations.md).

## <a name="protocol-diagram"></a>Diagrama de protocolo

En el diagrama siguiente se muestra el aspecto que tiene el flujo implícito de inicio de sesión completo y en las secciones siguientes se describe cada paso con más detalle.

![Diagrama que muestra el flujo de inicio de sesión implícito](./media/v2-oauth2-implicit-grant-flow/convergence-scenarios-implicit.svg)

## <a name="send-the-sign-in-request"></a>Envío de la solicitud de inicio de sesión

Al principio, para iniciar la sesión del usuario en la aplicación, puede enviar una solicitud de autenticación [OpenID Connect](v2-protocols-oidc.md) y obtener un `id_token` del punto de conexión de la plataforma de identidad de Microsoft.

> [!IMPORTANT]
> Para solicitar con éxito un token de identificador y un token de acceso, el registro de aplicación en la página [Azure Portal: Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) tiene que tener habilitado el flujo de concesión implícita correspondiente, para ello, seleccione **Tokens de identificador** y **Tokens de acceso** en la sección **Concesión implícita**. Si no está habilitado, se devolverá un error `unsupported_response`: **The provided value for the input parameter 'response_type' is not allowed for this client (El valor proporcionado para el parámetro de entrada “response_type” no se admite para este cliente). Expected value is 'code'** ("No se permite el valor proporcionado para el parámetro de entrada "response_type" para este cliente. El valor esperado es "code"").

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Para probar el inicio de sesión con el flujo implícito, haga clic en <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a> Después de iniciar sesión, el explorador se redirigirá a `https://localhost/myapp/` con un elemento `id_token` en la barra de direcciones.
>

| Parámetro |  | Descripción |
| --- | --- | --- |
| `tenant` | requerido |El valor `{tenant}` de la ruta de acceso de la solicitud se puede usar para controlar quién puede iniciar sesión en la aplicación. Los valores permitidos son `common`, `organizations`, `consumers` y los identificadores de inquilinos. Para obtener más información, consulte los [conceptos básicos sobre el protocolo](active-directory-v2-protocols.md#endpoints). |
| `client_id` | requerido | El identificador de aplicación (cliente) que la página [Azure Portal: Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) asignó a la aplicación. |
| `response_type` | requerido |Debe incluir `id_token` para el inicio de sesión en OpenID Connect. También puede incluir el `token` response_type. El uso de `token` aquí permitirá a la aplicación recibir un token de acceso inmediatamente desde el punto de conexión autorizado sin tener que realizar una segunda solicitud para autorizar el punto de conexión. Si usa el `token` response_type, el parámetro `scope` debe contener un ámbito que indica para qué recurso se va a emitir el token (por ejemplo, user.read en Microsoft Graph).  |
| `redirect_uri` | recomendado |El redirect_uri de su aplicación, a donde su aplicación puede enviar y recibir las respuestas de autenticación. Debe coincidir exactamente con uno de los redirect_uris que registró en el portal, con la excepción de que debe estar codificado como URL. |
| `scope` | requerido |Una lista de [ámbitos](v2-permissions-and-consent.md) separada por espacios. Asegúrese de incluir el ámbito `openid` para OpenID Connect (id_tokens), que se traduce en el permiso de inicio de sesión en la interfaz de usuario de consentimiento. Opcionalmente, también puede incluir los ámbitos `email` y `profile` para obtener acceso a datos de usuario adicionales. También puede incluir otros ámbitos en esta solicitud para solicitar consentimiento para diversos recursos, si se solicita un token de acceso. |
| `response_mode` | opcional |Especifica el método que debe usarse para enviar el token resultante de nuevo a la aplicación. El valor predeterminado es "query" para un token de acceso, pero "fragment" si la solicitud incluye un valor id_token. |
| `state` | recomendado |Un valor incluido en la solicitud que se devolverá también en la respuesta del token. Puede ser una cadena de cualquier contenido que desee. Normalmente se usa un valor único generado de forma aleatoria para [evitar los ataques de falsificación de solicitudes entre sitios](https://tools.ietf.org/html/rfc6749#section-10.12). El estado también se usa para codificar información sobre el estado del usuario en la aplicación antes de que se haya producido la solicitud de autenticación, por ejemplo, la página o vista en la que estaban. |
| `nonce` | requerido |Un valor incluido en la solicitud, generada por la aplicación, que se incluirá en el id_token resultante como una notificación. La aplicación puede comprobar este valor para mitigar los ataques de reproducción de token. Normalmente, el valor es una cadena única aleatoria que puede utilizarse para identificar el origen de la solicitud. Solo es necesario cuando se solicita un valor id_token. |
| `prompt` | opcional |Indica el tipo de interacción necesaria con el usuario. Los únicos valores válidos en este momento son "login", "none", "select_account" y "consent". `prompt=login` obligará al usuario a escribir sus credenciales en esa solicitud, negando el inicio de sesión único. `prompt=none` es lo contrario, se asegurará de que al usuario no se le presenta ninguna solicitud interactiva del tipo que sea. Si la solicitud no se puede completar sin notificaciones mediante el inicio de sesión único, el punto de conexión de la plataforma de identidad de Microsoft devolverá un error. `prompt=select_account` envía al usuario a un selector de cuenta donde aparecerán todas las cuentas que se recuerdan en la sesión. `prompt=consent` desencadenará el cuadro de diálogo de consentimiento de OAuth después de que el usuario inicia sesión, y solicitará a este que conceda permisos a la aplicación. |
| `login_hint`  |opcional |Puede usarse para rellenar previamente el campo de nombre de usuario y dirección de correo electrónico de la página de inicio de sesión del usuario, si sabe su nombre de usuario con antelación. A menudo las aplicaciones usarán este parámetro durante la reautenticación, dado que ya han extraído el nombre de usuario de un inicio de sesión anterior mediante la notificación `preferred_username`.|
| `domain_hint` | opcional |Si se incluye, omitirá el proceso de detección basado en correo electrónico por el que pasa el usuario en la página de inicio de sesión, con lo que la experiencia de usuario será ligeramente más sencilla. Esto se usa normalmente para las aplicaciones de línea de negocio que operan en un solo inquilino y que proporcionarán un nombre de dominio dentro de un inquilino determinado.  Esto redirigirá al usuario al proveedor de federación de ese inquilino.  Tenga en cuenta que este parámetro impedirá que los invitados inicien sesión en la aplicación.  |

En este punto, se le pedirá al usuario que escriba sus credenciales y que complete la autenticación. El punto de conexión de la plataforma de identidad de Microsoft se asegurará también de que el usuario ha dado su consentimiento a los permisos indicados en el parámetro de la consulta `scope`. Si el usuario no ha dado su consentimiento a **ninguno** de esos permisos, se le solicitará para los permisos necesarios. Para obtener más información, vea [Permisos, consentimiento y aplicaciones de varios inquilinos](v2-permissions-and-consent.md).

Una vez que el usuario se autentica y otorga su consentimiento, el punto de conexión de la plataforma de identidad de Microsoft devuelve una respuesta a la aplicación en el `redirect_uri` indicado mediante el método especificado en el parámetro `response_mode`.

#### <a name="successful-response"></a>Respuesta correcta

Una respuesta correcta que usa `response_mode=fragment` y `response_type=id_token+token` es como la siguiente (con saltos de línea para mejorar la legibilidad):

```
GET https://localhost/myapp/#
&token_type=Bearer
&expires_in=3599
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parámetro | Descripción |
| --- | --- |
| `access_token` |Incluido si `response_type` incluye `token`. El token de acceso que solicitó la aplicación. El token de acceso no se debe descodificar o inspeccionar en forma alguna; se debe tratar como una cadena opaca. |
| `token_type` |Incluido si `response_type` incluye `token`. Siempre será `Bearer`. |
| `expires_in`|Incluido si `response_type` incluye `token`. Indica el número de segundos que el token es válido para el almacenamiento en caché. |
| `scope` |Incluido si `response_type` incluye `token`. Indica los ámbitos para los que será válido access_token. Es posible que no incluya todos los ámbitos solicitados, si no eran aplicables al usuario (en el caso de que se soliciten ámbitos exclusivos de Azure AD cuando se usa una cuenta personal para iniciar sesión). |
| `id_token` | Un token web JSON (JWT) firmado. La aplicación puede decodificar los segmentos de este token para solicitar información acerca del usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe confiar en ellos para cualquier autorización o límite de seguridad. Para más información sobre los parámetros id_tokens, consulte [`id_token reference`](id-tokens.md). <br> **Nota:** Solo se proporciona si se solicitó el ámbito `openid`. |
| `state` |Si se incluye un parámetro de estado en la solicitud, debería aparecer el mismo valor en la respuesta. La aplicación debería comprobar que los valores de estado de la solicitud y la respuesta son idénticos. |

#### <a name="error-response"></a>Respuesta de error

Las respuestas de error también pueden enviarse al `redirect_uri` para que la aplicación pueda controlarlas adecuadamente:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parámetro | Descripción |
| --- | --- |
| `error` |Una cadena de código de error que puede utilizarse para clasificar los tipos de errores que se producen y para reaccionar ante ellos. |
| `error_description` |Un mensaje de error específico que puede ayudar a un desarrollador a identificar la causa de un error de autenticación. |

## <a name="getting-access-tokens-silently-in-the-background"></a>Obtención de tokens de acceso silenciosamente en segundo plano

Ahora que ha iniciado la sesión del usuario en su aplicación de página única, puede obtener tokens de acceso de forma silenciosa para llamar a las API web protegidas por la plataforma de identidad de Microsoft, como las de [Microsoft Graph](https://developer.microsoft.com/graph). Incluso si ya recibió un token mediante el `token` response_type, puede usar este método para adquirir tokens para recursos adicionales sin tener que redirigir al usuario para que vuelva a iniciar sesión.

En el flujo normal de OpenID Connect y OAuth, haría esto mediante una solicitud al punto de conexión `/token` de la plataforma de identidad de Microsoft. Sin embargo, el punto de conexión de la plataforma de identidad de Microsoft no admite solicitudes CORS, así que las llamadas a AJAX para obtener y actualizar los tokens no son posibles. En su lugar, puede usar el flujo implícito en un iframe oculto para obtener nuevos tokens para otras API web: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read 
&response_mode=fragment
&state=12345
&nonce=678910
&prompt=none
&login_hint=myuser@mycompany.com
```

Para obtener más información sobre los parámetros de consulta en la dirección URL, vea [Envío de la solicitud de inicio de sesión](#send-the-sign-in-request).

> [!TIP]
> Pruebe a copiar y pegar la siguiente solicitud en una pestaña del explorador. (No olvide reemplazar los valores `login_hint` por los valores correctos para el usuario).
>
>`https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&login_hint={your-username}`
>

Gracias al parámetro `prompt=none` , esta solicitud tendrá éxito o dará error inmediatamente y volverá a la aplicación. Se enviará una respuesta correcta a la aplicación en el `redirect_uri` indicado, mediante el método especificado en el parámetro `response_mode`.

#### <a name="successful-response"></a>Respuesta correcta

Una respuesta correcta al usar `response_mode=fragment` tiene el siguiente aspecto:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parámetro | Descripción |
| --- | --- |
| `access_token` |Incluido si `response_type` incluye `token`. El token de acceso que la aplicación ha solicitado, en este caso para Microsoft Graph. El token de acceso no se debe descodificar o inspeccionar en forma alguna; se debe tratar como una cadena opaca. |
| `token_type` | Siempre será `Bearer`. |
| `expires_in` | Indica el número de segundos que el token es válido para el almacenamiento en caché. |
| `scope` | Indica los ámbitos para los que será válido access_token. Es posible que no incluya todos los ámbitos solicitados, si no eran aplicables al usuario (en el caso de que se soliciten ámbitos exclusivos de Azure AD cuando se usa una cuenta personal para iniciar sesión). |
| `id_token` | Un token web JSON (JWT) firmado. Incluido si `response_type` incluye `id_token`. La aplicación puede decodificar los segmentos de este token para solicitar información acerca del usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe confiar en ellos para cualquier autorización o límite de seguridad. Para obtener más información sobre los parámetros id_token, vea la [Referencia de `id_token`](id-tokens.md). <br> **Nota:** Solo se proporciona si se solicitó el ámbito `openid`. |
| `state` |Si se incluye un parámetro de estado en la solicitud, debería aparecer el mismo valor en la respuesta. La aplicación debería comprobar que los valores de estado de la solicitud y la respuesta son idénticos. |

#### <a name="error-response"></a>Respuesta de error

Las respuestas de error también pueden enviarse al `redirect_uri` para que la aplicación pueda controlarlas adecuadamente. En el caso de `prompt=none`, un error esperado será:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parámetro | Descripción |
| --- | --- |
| `error` |Una cadena de código de error que puede utilizarse para clasificar los tipos de errores que se producen y para reaccionar ante ellos. |
| `error_description` |Un mensaje de error específico que puede ayudar a un desarrollador a identificar la causa de un error de autenticación. |

Si recibe este error en la solicitud de iframe, el usuario debe iniciar sesión de nuevo de manera interactiva para recuperar un nuevo token. Puede elegir tratar este caso de la manera que más sentido tenga para su aplicación.

## <a name="refreshing-tokens"></a>Actualización de tokens

La concesión implícita no proporciona tokens de actualización. Tanto los elementos `id_token` como `access_token` caducan tras un corto período de tiempo, así que su aplicación debe estar preparada para actualizar estos tokens de manera periódica. Para actualizar cualquier tipo de token, puede realizar la misma solicitud de iframe oculto que antes mediante el parámetro `prompt=none` para controlar el comportamiento de la plataforma de identidad. Si quiere recibir un nuevo `id_token`, asegúrese de usar `id_token` en `response_type` y `scope=openid`, así como un parámetro `nonce`.

## <a name="send-a-sign-out-request"></a>Envío de una solicitud de cierre de sesión

`end_session_endpoint` de OpenID Connect permite a la aplicación enviar una solicitud para el punto de conexión de la plataforma de identidad de Microsoft para finalizar una sesión de usuario y borrar las cookies establecidas por dicho punto de conexión de plataforma de identidad de Microsoft. Para que un usuario cierre por completo la sesión de una aplicación web, la aplicación debe finalizar su propia sesión con el usuario (normalmente borrando una caché de tokens o eliminando las cookies) y luego redirigir el explorador para:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parámetro |  | Descripción |
| --- | --- | --- |
| `tenant` |requerido |El valor `{tenant}` de la ruta de acceso de la solicitud se puede usar para controlar quién puede iniciar sesión en la aplicación. Los valores permitidos son `common`, `organizations`, `consumers` y los identificadores de inquilinos. Para obtener más información, consulte los [conceptos básicos sobre el protocolo](active-directory-v2-protocols.md#endpoints). |
| `post_logout_redirect_uri` | recomendado | La dirección URL a la que se debe redirigir al usuario después de completar el cierre de sesión. Este valor debe coincidir con uno de los URI de redirección registrados en la aplicación. Si no se incluye, el usuario verá un mensaje genérico del punto de conexión de la plataforma de identidad de Microsoft. |

## <a name="next-steps"></a>Pasos siguientes

* Repase los [ejemplos de MSAL JS](sample-v2-code.md) para comenzar a codificar.
