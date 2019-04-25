---
title: Solicitar un token de acceso - Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre cómo solicitar un token de acceso de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 5670d8b3c97cc1f9f6d149e8eadaa60d527e45f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60361122"
---
# <a name="request-an-access-token-in-azure-active-directory-b2c"></a>Solicitar un token de acceso en Azure Active Directory B2C

Un *token de acceso* contiene notificaciones que puede usar en Azure Active Directory (Azure AD) B2C para identificar los permisos concedidos a las API. Al llamar a un servidor de recursos, un token de acceso debe estar presente en la solicitud HTTP. Un token de acceso se indica como **access_token** en las respuestas de Azure AD B2C. 

Este artículo muestra cómo solicitar un token de acceso para una aplicación web y API web. Para obtener más información acerca de los tokens de Azure AD B2C, consulte el [información general de tokens de Azure Active Directory B2C](active-directory-b2c-reference-tokens.md).

> [!NOTE]
> **Las cadenas de la API web (en nombre de) no son compatibles con Azure AD B2C.** -Muchas arquitecturas incluyen una API web que necesita llamar a otro nivel inferior web API, ambas protegidas mediante Azure AD B2C. Este escenario es común en los clientes que tienen un API web back-end, que a su vez llama a un servicio de otro. Este escenario de API web encadenadas puede admitirse mediante la concesión de credenciales de portador JWT de OAuth 2.0, también conocido como flujo "en nombre de". Sin embargo, el flujo "en nombre de" no está implementado actualmente en Azure AD B2C.

## <a name="prerequisites"></a>Requisitos previos

- [Cree un flujo de usuario](tutorial-create-user-flows.md) para permitir a los usuarios registrarse e iniciar sesión en la aplicación.
- Si no ya lo ha hecho, [agregar una aplicación de API a su inquilino de Azure Active Directory B2C web](add-web-application.md).

## <a name="scopes"></a>Ámbitos

Los ámbitos proporcionan una manera de administrar los permisos a los recursos protegidos. Cuando se solicita un token de acceso, la aplicación cliente debe especificar los permisos deseados en el **ámbito** parámetro de la solicitud. Por ejemplo, para especificar el **valor de ámbito** de `read` para la API que tiene el **App ID URI** de `https://contoso.onmicrosoft.com/api`, el ámbito sería `https://contoso.onmicrosoft.com/api/read`.

La API web utiliza los ámbitos para implementar el control de acceso basado en el ámbito. Por ejemplo, los usuarios de la API web pueden tener ambos accesos de lectura y de escritura, o pueden tener solo acceso de lectura. Para adquirir varios permisos en la misma solicitud, puede agregar varias entradas en la única **ámbito** parámetro de la solicitud, separada por espacios.

El ejemplo siguiente muestra los ámbitos que se puede descodificados en una dirección URL:

```
scope=https://contoso.onmicrosoft.com/api/read openid offline_access
```

El ejemplo siguiente muestra los ámbitos codificados en una dirección URL:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fapi%2Fread%20openid%20offline_access
```

Si solicitar más ámbitos que lo que se concede para la aplicación cliente, la llamada se realiza correctamente si se concede al menos un permiso. El **scp** notificación en el token de acceso resultante se rellena solo con los permisos que otorgó correctamente. El estándar de OpenID Connect especifica varios valores de ámbito especial. Los ámbitos siguientes representan el permiso para tener acceso al perfil del usuario:

- **OpenID** -solicita un token de identificador.
- **offline_access** -solicita un token de actualización mediante [fluye el código de autenticación](active-directory-b2c-reference-oauth-code.md).

Si el **response_type** parámetro en un `/authorize` solicitud incluye `token`, **ámbito** parámetro debe incluir al menos un ámbito de recurso distinto `openid` y `offline_access`que se otorgará. En caso contrario, el `/authorize` solicitud produce un error.

## <a name="request-a-token"></a>Solicitar un token

Para solicitar un token de acceso, tiene un código de autorización. A continuación es un ejemplo de una solicitud para el `/authorize` punto de conexión para un código de autorización. No se admiten dominios personalizados para su uso con tokens de acceso. Usar su dominio de inquilino name.onmicrosoft.com en la dirección URL de solicitud.

En el ejemplo siguiente, reemplace estos valores:

- `<tenant-name>`: el nombre de su inquilino de Azure AD B2C.
- `<policy-name>`: el nombre de su directiva o flujo de usuarios personalizados.
- `<application-ID>` -El identificador de aplicación de la aplicación web que ha registrado para admitir el flujo de usuario.
- `<redirect-uri>`: el **identificador URI de redirección** que especificó al registrar la aplicación cliente.

```
GET https://<tenant-name>.b2clogin.com/tfp/<tenant-name>.onmicrosoft.com/<policy-name>/oauth2/v2.0/authorize?
client_id=<application-ID>
&nonce=anyRandomValue
&redirect_uri=https://jwt.ms
&scope=https://tenant-name>.onmicrosoft.com/api/read
&response_type=code 
```

La respuesta con el código de autorización debe ser similar a este ejemplo:

```
https://jwt.ms/?code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
```

Después de recibir correctamente el código de autorización, puede usar para solicitar un token de acceso:

```
POST <tenant-name>.onmicrosoft.com/oauth2/v2.0/token?p=<policy-name> HTTP/1.1
Host: https://<tenant-name>.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&client_id=<application-ID>
&scope=https://<tenant-name>.onmicrosoft.com/api/read
&code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
&redirect_uri=https://jwt.ms
&client_secret=2hMG2-_:y12n10vwH...
```

Debería ver algo parecido a la siguiente respuesta:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrN...",
    "token_type": "Bearer",
    "not_before": 1549647431,
    "expires_in": 3600,
    "expires_on": 1549651031,
    "resource": "f2a76e08-93f2-4350-833c-965c02483b11",
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiJjNjRhNGY3ZC0zMDkxLTRjNzMtYTcyMi1hM2YwNjk0Z..."
}
```

Cuando se usa https://jwt.ms para examinar el token de acceso que se devolvió, debería ver algo parecido al ejemplo siguiente:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dl..."
}.{
  "iss": "https://contoso0926tenant.b2clogin.com/c64a4f7d-3091-4c73-a7.../v2.0/",
  "exp": 1549651031,
  "nbf": 1549647431,
  "aud": "f2a76e08-93f2-4350-833c-965...",
  "oid": "1558f87f-452b-4757-bcd1-883...",
  "sub": "1558f87f-452b-4757-bcd1-883...",
  "name": "David",
  "tfp": "B2C_1_signupsignin1",
  "nonce": "anyRandomValue",
  "scp": "read",
  "azp": "38307aee-303c-4fff-8087-d8d2...",
  "ver": "1.0",
  "iat": 1549647431
}.[Signature]
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [configuración de tokens de Azure AD B2C](configure-tokens.md)
