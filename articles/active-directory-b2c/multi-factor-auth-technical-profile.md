---
title: Perfiles técnicos de Azure MFA en las directivas personalizadas
titleSuffix: Azure AD B2C
description: Referencia de directivas personalizadas para perfiles técnicos de Azure Multi-Factor Authentication (MFA) en Azure AD B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 05851dba9de06b5dfba2da4f455fbaf5e9376d08
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/29/2020
ms.locfileid: "78184288"
---
# <a name="define-an-azure-mfa-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Definición de un perfil técnico de Azure AD en una directiva personalizada de Azure AD B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) proporciona compatibilidad para verificar un número de teléfono mediante Azure Multi-Factor Authentication (MFA). Use este perfil técnico para generar y enviar un código a un número de teléfono y, a continuación, verifique el código.

El perfil técnico de Azure MFA también puede devolver un mensaje de error. Puede diseñar la integración con Azure MFA mediante el uso de un **perfil técnico de validación**. El perfil técnico de validación llama al servicio Azure MFA. El perfil técnico de validación valida los datos que proporciona el usuario antes de que continúe el recorrido del usuario. Con el perfil técnico de validación, se muestra un mensaje de error en una página autoafirmada.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="protocol"></a>Protocolo

El atributo **Name** del elemento **Protocol** tiene que establecerse en `Proprietary`. El atributo **handler** debe contener el nombre completo del ensamblado del controlador de protocolo que usa Azure AD B2C:

```
Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

En el ejemplo siguiente se muestra un perfil técnico de Azure MFA:

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    ...
```

## <a name="send-sms"></a>Envío de SMS

El primer modo de este perfil técnico es generar un código y enviarlo. Pueden configurarse las opciones siguientes para este modo.

### <a name="input-claims"></a>Notificaciones de entrada

El elemento **InputClaims** contiene una lista de notificaciones para enviar a Azure MFA. También puede asignar el nombre de la notificación al nombre definido en el perfil técnico de MFA.

| ClaimReferenceId | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| userPrincipalName | Sí | Identificador del usuario que posee el número de teléfono. |
| phoneNumber | Sí | Número de teléfono al que se va a enviar un código de SMS. |
| companyName | Sin |Nombre de la empresa en el SMS. Si no se proporciona, se usa el nombre de la aplicación. |
| locale | Sin | Configuración regional del SMS. Si no se proporciona, se usa la configuración regional del explorador del usuario. |

El elemento **InputClaimsTransformations** puede contener una colección de elementos **InputClaimsTransformation** que se usan para modificar las notificaciones de entrada o generar otras nuevas antes del envío al servicio Azure MFA.

### <a name="output-claims"></a>Notificaciones de salida

El proveedor del protocolo de Azure MFA no devuelve ningún **OutputClaims**, por lo que no es necesario especificar notificaciones de salida. Sin embargo, puede incluir notificaciones que no devuelve el proveedor de identidades de Azure MFA, siempre que establezca el atributo `DefaultValue`.

El elemento **OutputClaimsTransformations** puede contener una colección de elementos **OutputClaimsTransformation** que se usan para modificar las notificaciones de salida o para generar nuevas.

### <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Operación | Sí | Debe ser **OneWaySMS**.  |
| UserMessageIfInvalidFormat | Sin | Mensaje de error personalizado si el número de teléfono proporcionado no es un número de teléfono válido |
| UserMessageIfCouldntSendSms | Sin | Mensaje de error personalizado si el número de teléfono proporcionado no acepta SMS |
| UserMessageIfServerError | Sin | Mensaje de error personalizado si el servidor ha encontrado un error interno |

### <a name="return-an-error-message"></a>Devolución de un mensaje de error

Tal y como se describe en la sección [Metadatos](#metadata), puede personalizar el mensaje de error que se muestra al usuario para los distintos casos de errores. También puede localizar esos mensajes si agrega la configuración regional como prefijo. Por ejemplo:

```XML
<Item Key="en.UserMessageIfInvalidFormat">Invalid phone number.</Item>
```

### <a name="example-send-an-sms"></a>Ejemplo: envío de un SMS

En el ejemplo siguiente se muestra un perfil técnico de Azure MFA que se usa para enviar un código a través de SMS.

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">OneWaySMS</Item>
    </Metadata>
    <InputClaimsTransformations>
        <InputClaimsTransformation ReferenceId="CombinePhoneAndCountryCode" />
        <InputClaimsTransformation ReferenceId="ConvertStringToPhoneNumber" />
    </InputClaimsTransformations>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="userPrincipalName" />
        <InputClaim ClaimTypeReferenceId="fullPhoneNumber" PartnerClaimType="phoneNumber" />
    </InputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>Compruebe el código.

El segundo modo de este perfil técnico es verificar un código. Pueden configurarse las opciones siguientes para este modo.

### <a name="input-claims"></a>Notificaciones de entrada

El elemento **InputClaims** contiene una lista de notificaciones para enviar a Azure MFA. También puede asignar el nombre de la notificación al nombre definido en el perfil técnico de MFA.

| ClaimReferenceId | Obligatorio | Descripción |
| --------- | -------- | ----------- | ----------- |
| phoneNumber| Sí | El mismo número de teléfono que se usó anteriormente para enviar un código. También se usa para buscar una sesión de verificación del teléfono. |
| verificationCode  | Sí | Código de verificación que proporciona el usuario y que se va a verificar |

El elemento **InputClaimsTransformations** puede contener una colección de elementos **InputClaimsTransformation** que se usan para modificar las notificaciones de entrada o generar otras nuevas antes de llamar al servicio Azure MFA.

### <a name="output-claims"></a>Notificaciones de salida

El proveedor del protocolo de Azure MFA no devuelve ningún **OutputClaims**, por lo que no es necesario especificar notificaciones de salida. Sin embargo, puede incluir notificaciones que no devuelve el proveedor de identidades de Azure MFA, siempre que establezca el atributo `DefaultValue`.

El elemento **OutputClaimsTransformations** puede contener una colección de elementos **OutputClaimsTransformation** que se usan para modificar las notificaciones de salida o para generar nuevas.

## <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Operación | Sí | Debe ser **Verify**. |
| UserMessageIfInvalidFormat | Sin | Mensaje de error personalizado si el número de teléfono proporcionado no es un número de teléfono válido |
| UserMessageIfWrongCodeEntered | Sin | Mensaje de error personalizado si el código especificado para la verificación es incorrecto |
| UserMessageIfMaxAllowedCodeRetryReached | Sin | Mensaje de error personalizado si el usuario ha intentado un código de verificación demasiadas veces |
| UserMessageIfThrottled | Sin | Mensaje de error personalizado si el usuario está limitado |
| UserMessageIfServerError | Sin | Mensaje de error personalizado si el servidor ha encontrado un error interno |

### <a name="return-an-error-message"></a>Devolución de un mensaje de error

Tal y como se describe en la sección [Metadatos](#metadata), puede personalizar el mensaje de error que se muestra al usuario para los distintos casos de errores. También puede localizar esos mensajes si agrega la configuración regional como prefijo. Por ejemplo:

```XML
<Item Key="en.UserMessageIfWrongCodeEntered">Wrong code has been entered.</Item>
```

### <a name="example-verify-a-code"></a>Ejemplo: verificación de un código

En el ejemplo siguiente se muestra un perfil técnico de Azure MFA que se usa para verificar el código.

```XML
<TechnicalProfile Id="AzureMfa-VerifySms">
    <DisplayName>Verify Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">Verify</Item>
    </Metadata>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="phoneNumber" PartnerClaimType="phoneNumber" />
        <InputClaim ClaimTypeReferenceId="verificationCode" />
    </InputClaims>
</TechnicalProfile>
```
