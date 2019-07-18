---
title: Uso de la Plataforma de identidad de Microsoft para el inicio de sesión de los usuarios mediante la concesión de credenciales de contraseña de propietario de recursos (ROPC) | Azure
description: Admita flujos de autenticación sin explorador mediante la concesión de credenciales de contraseña de propietario de recursos.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/20/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: da111311de7b873be6453862ffcbd56fe546ea7f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482381"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-resource-owner-password-credential"></a>La Plataforma de identidad de Microsoft y la credencial de contraseña de propietario de recursos OAuth 2.0

La Plataforma de identidad de Microsoft admite la [concesión de credenciales de contraseña de propietario de recursos (ROPC)](https://tools.ietf.org/html/rfc6749#section-4.3), que permite que una aplicación inicie la sesión del usuario al controlar directamente su contraseña. El flujo de ROPC requiere un alto grado de confianza y exposición del usuario, por lo que solo debería usar este flujo cuando no se puedan usar otros flujos más seguros.

> [!IMPORTANT]
>
> * El punto de conexión de la Plataforma de identidad de Microsoft solo admite ROPC para inquilinos de Azure AD, no cuentas personales. Esto significa que debe usar un punto de conexión específico del inquilino (`https://login.microsoftonline.com/{TenantId_or_Name}`) o el punto de conexión `organizations`.
> * Las cuentas personales que se invitan a un inquilino de Azure AD no pueden usar ROPC.
> * Las cuentas sin contraseña no pueden iniciar sesión a través de ROPC. En este escenario, se recomienda usar en su lugar un flujo distinto para la aplicación.
> * Si los usuarios deben usar la autenticación multifactor (MFA) para iniciar sesión en la aplicación, se les bloqueará.

## <a name="protocol-diagram"></a>Diagrama de protocolo

En el diagrama siguiente se muestra el flujo de ROPC.

![Diagrama que muestra el flujo de credenciales de contraseña de propietario del recurso](./media/v2-oauth2-ropc/v2-oauth-ropc.svg)

## <a name="authorization-request"></a>Solicitud de autorización

El flujo de ROPC es una solicitud única&mdash;envía la identificación del cliente y las credenciales del usuario al punto de distribución de emisión (IDP) y después, a cambio, recibe tokens. El cliente debe solicitar la dirección de correo electrónico (UPN) y la contraseña del usuario antes de hacerlo. Inmediatamente después de que una solicitud se realice correctamente, el cliente deberá liberar de manera segura las credenciales del usuario de la memoria. Nunca debe guardarlas.

> [!TIP]
> Pruebe a ejecutar esta solicitud en Postman
> [![Pruebe a ejecutar esta solicitud en Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)


```
// Line breaks and spaces are for legibility only.

POST {tenant}/oauth2/v2.0/token
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=user.read%20openid%20profile%20offline_access
&username=MyUsername@myTenant.com
&password=SuperS3cret
&grant_type=password
```

| Parámetro | Condición | DESCRIPCIÓN |
| --- | --- | --- |
| `tenant` | Obligatorio | El inquilino del directorio en el que desea iniciar la sesión del usuario. Puede estar en formato de nombre descriptivo o GUID. Este parámetro no se puede establecer en `common` ni en `consumers`, pero sí se puede establecer en `organizations`. |
| `grant_type` | Obligatorio | Se debe establecer en `password`. |
| `username` | Obligatorio | La dirección de correo electrónico del usuario. |
| `password` | Obligatorio | La contraseña del usuario. |
| `scope` | Recomendado | Una lista de [ámbitos](v2-permissions-and-consent.md) o permisos separada por espacios que requiere la aplicación. En un flujo interactivo, el administrador o el usuario deben dar su consentimiento a estos ámbitos de antemano. |

### <a name="successful-authentication-response"></a>Respuesta de autenticación correcta

En el ejemplo siguiente se muestra una respuesta de token correcta:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parámetro | Formato | DESCRIPCIÓN |
| --------- | ------ | ----------- |
| `token_type` | Cadena | Siempre se establece en `Bearer`. |
| `scope` | Cadenas separadas por espacios | Si se devolvió un token de acceso, este parámetro muestra los ámbitos para los que es válido el token de acceso. |
| `expires_in`| int | Número de segundos durante los que el token de acceso incluido es válido. |
| `access_token`| Cadena opaca | Se emite para los [ámbitos](v2-permissions-and-consent.md) solicitados. |
| `id_token` | JWT | Se emite si el parámetro `scope` original incluye el ámbito `openid`. |
| `refresh_token` | Cadena opaca | Se emite si el parámetro `scope` original incluye `offline_access`. |

Puede usar el token de actualización para adquirir nuevos tokens de acceso y tokens de actualización con el mismo flujo que se describe en la [Documentación del flujo de código de OAuth](v2-oauth2-auth-code-flow.md#refresh-the-access-token).

### <a name="error-response"></a>Respuesta de error

Si el usuario no ha proporcionado la contraseña o el nombre de usuario adecuado o si el cliente no ha recibido el consentimiento solicitado, la autenticación no se realizará.

| Error | DESCRIPCIÓN | Acción del cliente |
|------ | ----------- | -------------|
| `invalid_grant` | Error de autenticación | Las credenciales no eran las correctas o el cliente no tiene consentimiento para los ámbitos solicitados. Si no se conceden los ámbitos, se devolverá un error `consent_required`. Si esto ocurre, el cliente debería enviar el usuario a una solicitud interactiva mediante una vista web o un explorador. |
| `invalid_request` | La solicitud se construyó de manera inadecuada. | El tipo de concesión no es compatible con los contextos de autenticación `/common` ni `/consumers`.  En su lugar, use `/organizations`. |
| `invalid_client` | La aplicación no está correctamente configurada | Esto puede ocurrir si la propiedad `allowPublicClient` no está establecida en true en el [manifiesto de aplicación](reference-app-manifest.md). La propiedad `allowPublicClient` es necesaria porque la concesión de ROPC no tiene un URI de redireccionamiento. Azure AD no puede determinar si la aplicación es una aplicación cliente pública o una aplicación cliente confidencial, a menos que se establezca esta propiedad. ROPC solo es compatible con aplicaciones cliente públicas. |

## <a name="learn-more"></a>Más información

* Pruebe usted mismo ROPC con la [aplicación de consola de ejemplo](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2).
* Para determinar si debe usar la versión 2.0 del punto de conexión, consulte la información sobre la [Plataforma de identidad de Microsoft](active-directory-v2-limitations.md).
