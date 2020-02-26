---
title: Definición de un perfil técnico de Azure AD en una directiva personalizada
titleSuffix: Azure AD B2C
description: Defina un perfil técnico de Azure Active Directory en una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/13/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 9f00bebfbab7b3726930e212893ae9dd2f5c17c8
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77193484"
---
# <a name="define-an-azure-active-directory-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Definición de un perfil técnico de Azure Active Directory en una directiva personalizada en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) proporciona compatibilidad con la administración de usuarios de Azure Active Directory. En este artículo se describen los detalles para que un perfil técnico interactúe con un proveedor de notificaciones que admita este protocolo estandarizado.

## <a name="protocol"></a>Protocolo

El atributo **Name** del elemento **Protocol** tiene que establecerse en `Proprietary`. El atributo **handler** debe contener el nombre completo del ensamblado de controlador de protocolo `Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Todos los perfiles técnicos de Azure AD incluyen el perfil técnico **AAD-Common**. Los perfiles técnicos siguientes no especifican el protocolo porque el protocolo está configurado en el perfil técnico **AAD-Common**:

- **AAD-UserReadUsingAlternativeSecurityId** y **AAD-UserReadUsingAlternativeSecurityId-NoError**: buscar una cuenta de redes sociales en el directorio.
- **AAD-UserWriteUsingAlternativeSecurityId**: crear una cuenta de redes sociales.
- **AAD-UserReadUsingEmailAddress**: buscar una cuenta local en el directorio.
- **AAD-UserWriteUsingLogonEmail**: crear una cuenta local.
- **AAD-UserWritePasswordUsingObjectId**: actualizar una contraseña de una cuenta local.
- **AAD-UserWriteProfileUsingObjectId**: actualizar un perfil de usuario de una cuenta local o social.
- **AAD-UserReadUsingObjectId**: leer un perfil de usuario de una cuenta local o social.
- **AAD-UserWritePhoneNumberUsingObjectId**: escribir el número de teléfono de MFA de una cuenta local o social

En el ejemplo siguiente se muestra el perfil técnico **AAD-Common**:

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />

  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
  </CryptographicKeys>

  <!-- We need this here to suppress the SelfAsserted provider from invoking SSO on validation profiles. -->
  <IncludeInSso>false</IncludeInSso>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Notificaciones de entrada

Los perfiles técnicos siguientes incluyen **InputClaims** para las cuentas locales y sociales:

- Los perfiles técnicos de cuenta social **AAD-UserReadUsingAlternativeSecurityId** y **AAD-UserWriteUsingAlternativeSecurityId** incluyen la notificación **AlternativeSecurityId**. Esta notificación contiene el identificador de usuario de la cuenta de red social.
- Los perfiles técnicos de cuenta local **AAD UserReadUsingEmailAddress** y **AAD-UserWriteUsingLogonEmail** incluyen la notificación **email**. Esta notificación contiene el nombre de inicio de sesión de la cuenta local.
- Los perfiles técnicos (local y de red social) unificados **AAD-UserReadUsingObjectId**, **AAD-UserWritePasswordUsingObjectId**, **AAD-UserWriteProfileUsingObjectId** y **AAD-UserWritePhoneNumberUsingObjectId** incluyen la notificación **objectId**. El identificador único de una cuenta.

El elemento **InputClaimsTransformations** puede contener una colección de elementos **InputClaimsTransformation** que se usan para modificar las notificaciones de entrada o generar otras nuevas.

## <a name="output-claims"></a>Notificaciones de salida

El elemento **OutputClaims** contiene una lista de notificaciones devuelta por el perfil técnico de Azure AD. Es posible que tenga que asignar el nombre de la notificación definida en la directiva al nombre definido en Azure Active Directory. También puede incluir las notificaciones que Azure Active Directory no devuelve, siempre y cuando establezca el atributo `DefaultValue`.

El elemento **OutputClaimsTransformations** puede contener una colección de elementos **OutputClaimsTransformation** que se usan para modificar las notificaciones de salida o para generar nuevas.

Por ejemplo, el perfil técnico **AAD-UserWriteUsingLogonEmail** crea una cuenta local y devuelve las notificaciones siguientes:

- **objectId**, que es el identificador de la cuenta nueva
- **newUser**, que indica si el usuario es nuevo
- **authenticationSource**, que establece la autenticación en `localAccountAuthentication`
- **userPrincipalName**, que es el nombre principal de usuario de la cuenta nueva
- **signInNames.emailAddress**, que es el nombre de inicio de sesión de la cuenta, similar a la notificación de entrada **email**

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="objectId" />
  <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
  <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
  <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
</OutputClaims>
```

## <a name="persistedclaims"></a>PersistedClaims

El elemento **PersistedClaims** contiene todos los valores que Azure AD debe conservar con la información de asignaciones posibles entre un tipo de notificación ya definido en la sección ClaimsSchema de la directiva y el nombre de atributo de Azure AD.

El perfil técnico **AAD-UserWriteUsingLogonEmail** crea una cuenta local y conserva las notificaciones siguientes:

```XML
  <PersistedClaims>
    <!-- Required claims -->
    <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
    <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password"/>
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />

    <!-- Optional claims. -->
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
```

El nombre de la notificación es el del atributo de Azure AD, a menos que se especifique el atributo **PartnerClaimType**, que contiene el nombre de atributo de Azure AD.

## <a name="requirements-of-an-operation"></a>Requisitos de una operación

- Debe haber exactamente un elemento **InputClaim** en el contenedor de notificaciones para todos los perfiles técnicos de Azure AD.
- Si la operación es `Write` o `DeleteClaims`, también debe aparecer en un elemento **PersistedClaims**.
- El valor de la notificación **userPrincipalName** debe tener el formato `user@tenant.onmicrosoft.com`.
- La notificación **displayName** es necesaria y no puede ser una cadena vacía.

## <a name="azure-ad-technical-provider-operations"></a>Operaciones de proveedores técnicos de Azure AD

### <a name="read"></a>Lectura

La operación **Read** lee los datos sobre una sola cuenta de usuario. Para leer datos del usuario, hay que proporcionar una clave como una notificación de entrada, como **objectId**, **userPrincipalName**, **signInNames** (cualquier tipo, nombre de usuario y cuenta de correo electrónico) o **alternativeSecurityId**.

El perfil técnico siguiente lee los datos sobre una cuenta de usuario mediante el valor objectId del usuario:

```XML
<TechnicalProfile Id="AAD-UserReadUsingObjectId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
  </Metadata>
  <IncludeInSso>false</IncludeInSso>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <OutputClaims>

    <!-- Required claims -->
    <OutputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />

    <!-- Optional claims -->
    <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

### <a name="write"></a>Escritura

La operación **Write** crea o actualiza una sola cuenta de usuario. Para escribir una cuenta de usuario, hay que proporcionar una clave como una notificación de entrada, como **objectId**, **userPrincipalName**, **signInNames.emailAddress** o **alternativeSecurityId**.

El perfil técnico siguiente crea la cuenta de red social:

```XML
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Write</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalAlreadyExists">You are already registered, please press the back button and sign in instead.</Item>
  </Metadata>
  <IncludeInSso>false</IncludeInSso>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="CreateOtherMailsFromEmail" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <PersistedClaims>
    <!-- Required claims -->
    <PersistedClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <PersistedClaim ClaimTypeReferenceId="userPrincipalName" />
    <PersistedClaim ClaimTypeReferenceId="mailNickName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />

    <!-- Optional claims -->
    <PersistedClaim ClaimTypeReferenceId="otherMails" />
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>
```

### <a name="deleteclaims"></a>DeleteClaims

La operación **DeleteClaims** borra la información de una lista de notificaciones proporcionada. Para eliminar información de las notificaciones, hay que proporcionar una clave como una notificación de entrada, como **objectId**, **userPrincipalName**, **signInNames.emailAddress** o **alternativeSecurityId**.

El perfil técnico siguiente elimina notificaciones:

```XML
<TechnicalProfile Id="AAD-DeleteClaimsUsingObjectId">
  <Metadata>
    <Item Key="Operation">DeleteClaims</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" PartnerClaimType="strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims />
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

### <a name="deleteclaimsprincipal"></a>DeleteClaimsPrincipal

La operación **DeleteClaimsPrincipal** elimina una sola cuenta de usuario del directorio. Para eliminar una cuenta de usuario, hay que proporcionar una clave como una notificación de entrada, como **objectId**, **userPrincipalName**, **signInNames.emailAddress** o **alternativeSecurityId**.

El perfil técnico siguiente elimina una cuenta de usuario del directorio mediante el nombre principal de usuario:

```XML
<TechnicalProfile Id="AAD-DeleteUserUsingObjectId">
  <Metadata>
    <Item Key="Operation">DeleteClaimsPrincipal</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <OutputClaims/>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

El perfil técnico siguiente elimina una cuenta de usuario de red social mediante **alternativeSecurityId**:

```XML
<TechnicalProfile Id="AAD-DeleteUserUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">DeleteClaimsPrincipal</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims/>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```
## <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Operación | Sí | La operación que se va a realizar. Valores posibles: `Read`, `Write`, `DeleteClaims` o `DeleteClaimsPrincipal`. |
| RaiseErrorIfClaimsPrincipalDoesNotExist | Sin | Genera un error si el objeto de usuario no existe en el directorio. Valores posibles: `true` o `false`. |
| UserMessageIfClaimsPrincipalDoesNotExist | Sin | Si se va a generar un error (vea la descripción del atributo RaiseErrorIfClaimsPrincipalDoesNotExist), especifique el mensaje para mostrar al usuario si el objeto de usuario no existe. El valor se puede [localizar](localization.md).|
| RaiseErrorIfClaimsPrincipalAlreadyExists | Sin | Genera un error si el objeto de usuario ya existe. Valores posibles: `true` o `false`.|
| UserMessageIfClaimsPrincipalAlreadyExists | Sin | Si se va a generar un error (vea la descripción del atributo RaiseErrorIfClaimsPrincipalAlreadyExists), especifique el mensaje para mostrar al usuario si el objeto de usuario ya existe. El valor se puede [localizar](localization.md).|
| ApplicationObjectId | Sin | El identificador de objeto de aplicación para los atributos de extensión. Valor: ObjectId de una aplicación. Para más información, vea [Uso de los atributos personalizados en una directiva de edición de perfil personalizada](custom-policy-custom-attributes.md). |
| ClientId | Sin | El identificador de cliente para acceder al inquilino como un tercero. Para más información, vea [Uso de los atributos personalizados en una directiva de edición de perfil personalizada](custom-policy-custom-attributes.md). |
| IncludeClaimResolvingInClaimsHandling  | Sin | En el caso de las notificaciones de entrada y salida, especifica si se incluye la [resolución de notificaciones](claim-resolver-overview.md) en el perfil técnico. Valores posibles: `true` o `false`  (valor predeterminado). Si desea utilizar un solucionador de notificaciones en el perfil técnico, establézcalo en `true`. |














