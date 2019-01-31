---
title: Definir un perfil técnico de OpenID Connect en una directiva personalizada de Azure Active Directory B2C | Microsoft Docs
description: Define un perfil técnico de OpenID Connect en una directiva personalizada de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 730d32fd5b9451c2b4edfe0fb1e50c06c2156815
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159478"
---
# <a name="define-a-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Definir un perfil técnico de OpenID Connect en una directiva personalizada de Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C ofrece compatibilidad con el proveedor de identidades del protocolo [OpenID Connect](https://openid.net/2015/04/17/openid-connect-certification-program/). OpenID Connect 1.0 define un nivel de identidad sobre OAuth 2.0 y representa la tecnología más avanzada en los protocolos de autenticación moderna.  Con el perfil técnico de OpenID Connect, puede establecer una federación con un proveedor de identidades basado en OpenID Connect, como Azure AD, lo que permite a los usuarios iniciar sesión con sus identidades existentes empresariales o de redes sociales.

## <a name="protocol"></a>Protocolo

El atributo **Name** del elemento **Protocol** tiene que establecerse en `OpenIdConnect`. Por ejemplo, el protocolo del perfil técnico **MSA-OIDC** es `OpenIdConnect`:

```XML
<TechnicalProfile Id="MSA-OIDC">
  <DisplayName>Microsoft Account</DisplayName>
  <Protocol Name="OpenIdConnect" />
  ...    
```

## <a name="input-claims"></a>Notificaciones de entrada

Los elementos **InputClaims** y **InputClaimsTransformations** no son necesarios. Pero puede que quiera enviar otros parámetros al proveedor de identidades. En el ejemplo siguiente se agrega el parámetro de cadena de consulta **domain_hint** con el valor de `contoso.com` a la solicitud de autorización.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Notificaciones de salida

El elemento **OutputClaims** contiene una lista de notificaciones proporcionada por el proveedor de identidades de OpenID Connect. Puede que tenga que asignar el nombre de la notificación definida en la directiva al nombre definido en el proveedor de identidades. También puede incluir notificaciones no especificadas por el proveedor de identidades, siempre que establezca el atributo `DefaultValue`.

El elemento **OutputClaimsTransformations** puede contener una colección de elementos **OutputClaimsTransformation** que se usan para modificar las notificaciones de salida o para generar nuevas.

En el ejemplo siguiente, se muestran las notificaciones proporcionadas por el proveedor de identidades de cuentas Microsoft:

- La notificación **sub** que se asigna a la notificación **socialIdpUserId**.
- La notificación **name** que se asigna a la notificación **displayName**.
- El **correo electrónico** sin asignación de nombre.

El perfil técnico también muestra la notificaciones no proporcionadas por el proveedor de identidades:

- La notificación **identityProvider** que contiene el nombre del proveedor de identidades.
- La notificación **authenticationSource** con un valor predeterminado de **socialIdpAuthentication**.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
  <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
</OutputClaims>
```

## <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| client_id | SÍ | El identificador de la aplicación del proveedor de identidades. |
| IdTokenAudience | Sin  | El público de id_token. Si se especifica, Azure AD B2C comprueba si el token se encuentra en una notificación proporcionada por el proveedor de identidades y si es igual al especificado. |
| METADATOS | SÍ | URL que apunta a un documento de configuración JSON con un formato que se corresponde con la especificación Detección de OpenID Connect, que también se conoce como punto de conexión de configuración de OpenID conocido. |
| ProviderName | Sin  | Nombre del proveedor de identidades. |
| response_types | Sin  | Tipo de respuesta según la especificación OpenID Connect Core 1.0. Valores posibles: `id_token`, `code` o `token`. |
| response_mode | Sin  | Método que usará el proveedor de identidades para enviar de vuelta el resultado Azure AD B2C. Valores posibles: `query`, `form_post` (predeterminado) o `fragment`. |
| ámbito | Sin  | Ámbito de la solicitud de acceso definida según la especificación OpenID Connect Core 1.0. Por ejemplo, `openid`, `profile` y `email`. |
| HttpBinding | Sin  | Enlace HTTP esperado al token de acceso y los puntos de conexión del token de notificaciones. Valores posibles: `GET` o `POST`.  |
| ValidTokenIssuerPrefixes | Sin  | Clave que puede usarse para iniciar sesión en todos los inquilinos al usar un proveedor de identidades multiinquilino, como Azure Active Directory. |
| UsePolicyInRedirectUri | Sin  | Indica si se usará una directiva al crear el URI de redireccionamiento. Al configurar la aplicación en el proveedor de identidades, necesita especificar el URI de redireccionamiento. El URI de redireccionamiento apunta a Azure AD B2C, `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` (login.microsoftonline.com se puede cambiar por your-tenant-name.b2clogin.com).  Si especifica `false`, tendrá que agregar un URI de redireccionamiento por cada directiva que use. Por ejemplo: `https://login.microsoftonline.com/te/{tenant}/{policy}/oauth2/authresp`. |
| MarkAsFailureOnStatusCode5xx | Sin  | Indica si una solicitud a un servicio externo tiene que marcarse como un error si el código de estado HTTP se encuentra en el intervalo 5xx. El valor predeterminado es `false`. |
| DiscoverMetadataByTokenIssuer | Sin  | Indica si los metadatos de OIDC tienen que detectarse con el emisor en el token JWT. |

## <a name="cryptographic-keys"></a>Claves de cifrado

El elemento **CryptographicKeys** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| client_secret | SÍ | Secreto de cliente de la aplicación del proveedor de identidades. La clave de cifrado es necesaria solo si los metadatos de **response_type** se establecen en `code`. En este caso, Azure AD B2C realiza otra llamada para cambiar el código de autorización por un token de acceso. Si los metadatos se establecen en `id_token`, puede omitir la clave criptográfica.  |  

## <a name="redirect-uri"></a>URI de redireccionamiento
 
Al configurar el URI de redireccionamiento del proveedor de identidades, escriba `https://login.microsoftonline.com/te/tenant/oauth2/authresp`. Asegúrese de reemplazar **tenant** por el nombre del inquilino (por ejemplo, contosob2c.onmicrosoft.com) o por el id. del inquilino. El URI de redireccionamiento necesita estar escrito todo en minúsculas.

Si usa el dominio **b2clogin.com** en lugar de **login.microsoftonline.com**, asegúrese de usar b2clogin.com en lugar de login.microsoftonline.com.

Ejemplos:

- [Agregar una cuenta Microsoft (MSA) como un proveedor de identidades mediante directivas personalizadas](active-directory-b2c-custom-setup-msa-idp.md)
- [Iniciar sesión con cuentas de Azure AD](active-directory-b2c-setup-aad-custom.md)
- [Permitir que los usuarios inicien sesión en un proveedor de identidades de Azure AD multiinquilino mediante directivas personalizadas](active-directory-b2c-setup-commonaad-custom.md)

 














