---
title: Autenticación con Azure AD y obtención de un token JWT con OAuth 2.0
description: Código de ejemplo que muestra cómo autenticarse con Azure Active Directory mediante OAuth 2.0 para acceder a aplicaciones web y API web protegidas de la organización.
services: active-directory
author: rloutlaw
manager: angerobe
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: routlaw
ms.custom: aaddev
ms.openlocfilehash: eb26101229ad60abae7a8a84f8dfa496488e84ba
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579010"
---
# <a name="request-an-access-token-using-oauth-20-to-access-web-apis-and-applications-secured-by-azure-active-directory"></a>Solicitud de un token de acceso mediante OAuth 2.0 para acceder a las API web y aplicaciones protegidas por Azure Active Directory

En este artículo se muestra cómo obtener un JSON Web Token (JWT) para acceder a recursos protegidos por Azure AD. Se supone que tiene un [token de autorización](/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code) de permisos concedidos por el usuario o mediante una [entidad de servicio](/azure/active-directory/develop/active-directory-application-objects).

## <a name="build-the-request"></a>Compilar la solicitud

Use la siguiente solicitud HTTP `POST` para obtener un token JWT para acceder a una aplicación o una API específicas protegidas por Azure AD.

```http
POST https://{tenant}/oauth2/v2.0/token?client_id={client-id}
&scope={scope}
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh   
```

### <a name="request-headers"></a>Encabezados de solicitud

Los siguientes encabezados son obligatorios:

|Encabezado de solicitud|DESCRIPCIÓN|  
|--------------------|-----------------|  
| *Host:* | https://login.microsoftonline.com |
| *Content-Type:*| application/x-www-form-urlencoded |
 

### <a name="uri-parameters"></a>Parámetros del identificador URI

| Parámetro     |                       | DESCRIPCIÓN                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tenant        | requerido              | El valor `{tenant}` de la ruta de acceso de la solicitud se puede usar para controlar quién puede iniciar sesión en la aplicación. Los valores permitidos son `common`, `organizations`, `consumers` y los identificadores de inquilinos. Para obtener más información, consulte los [conceptos básicos sobre el protocolo](active-directory-v2-protocols.md#endpoints).                                                                                                                                                   |
| client_id     | requerido              | El identificador de la aplicación que el portal de registro ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) asignó a su aplicación.                                                                                                                                                                                                                             |
| grant_type    | requerido              | Debe ser `authorization_code` para el flujo de código de autorización.                                                                                                                                                                                                                                                                                                                                                                            |
| ámbito         | requerido              | Una lista de ámbitos separada por espacios. Los ámbitos solicitados en esta fase deben ser un subconjunto de los ámbitos de la llamada a `/authorize` o un equivalente de este. Si los ámbitos especificados en esta solicitud abarcan varios servidores de recursos, el extremo v2.0 devolverá un token para el recurso especificado en el primer ámbito. Para obtener una explicación más detallada de los ámbitos, consulte [permisos, consentimiento y ámbitos](v2-permissions-and-consent.md). |
| código          | requerido              | El authorization_code que obtuvo en la llamada a `/authorize`                                                                                                                                                                                                                                                                                                                                                                |
| redirect_uri  | requerido              | El mismo valor redirect_uri usado para adquirir el código de autorización.                                                                                                                                                                                                                                                                                                                                                             |
| client_secret | necesario para las aplicaciones web | El secreto de la aplicación que creó en el portal de registro de aplicaciones para su aplicación. No debería utilizarse en una aplicación nativa, porque los client_secrets no se pueden almacenar de forma confiable en los dispositivos. Es necesario para aplicaciones web y las API web, que tienen la capacidad de almacenar el client_secret de forma segura en el lado del servidor.  Los secretos de cliente deben codificarse como dirección URL antes de enviarse.                                                                                 |
| code_verifier | opcional              | El mismo valor de code_verifier que usó para obtener el valor de authorization_code. Se requiere si PKCE se utilizó en la solicitud de concesión de código de autorización. Para obtener más información, consulte [PKCE RFC](https://tools.ietf.org/html/rfc7636).                                                                                                                                                                                                                                                                                             |
## <a name="handle-the-response"></a>Control de la respuesta

Una respuesta correcta del token contiene un token de JWT y tendrá el siguiente aspecto:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parámetro     | DESCRIPCIÓN                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| access_token  | El token de acceso solicitado. La aplicación puede utilizar este token para autenticarse en el recursos protegido, como una API web.                                                                                                                                                                                                                                                                                                                                    |
| token_type    | Indica el valor de tipo de token. El único tipo que admite Azure AD es el portador                                                                                                                                                                                                                                                                                                                                                                           |
| expires_in    | Durante cuánto tiempo es válido el token de acceso (en segundos).                                                                                                                                                                                                                                                                                                                                                                                                       |
| ámbito         | Los ámbitos para los que el access_token es válido.                                                                                                                                                                                                                                                                                                                                                                                                         |
| refresh_token | Un token de actualización de OAuth 2.0. La aplicación puede utilizar este token para adquirir tokens de acceso adicionales una vez que expire el token de acceso actual. Los refresh_tokens son de larga duración y pueden usarse para conservar el acceso a los recursos durante largos períodos de tiempo. Para más información, consulte la [referencia a los tokens v2.0](v2-id-and-access-tokens.md). <br> **Nota:** Solo se proporciona si se solicitó el ámbito `offline_access`.                                               |
| ID_token      | Un token web JSON (JWT) sin firmar. La aplicación puede decodificar los segmentos de este token para solicitar información acerca del usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe confiar en ellos para cualquier autorización o límite de seguridad. Para más información sobre los parámetros id_tokens, consulte [Referencia de los tokens del punto de conexión v2.0](v2-id-and-access-tokens.md). <br> **Nota:** Solo se proporciona si se solicitó el ámbito `openid`. |



