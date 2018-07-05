---
title: Administración de SSO y personalización de tokens con directivas personalizadas en Azure Active Directory B2C | Microsoft Docs
description: Más información sobre la administración de SSO y personalización de token con directivas personalizadas.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/02/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 811fb8b2de59c9d324ab4acb8b0f51b4cec80aee
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441804"
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Azure Active Directory B2C: administración de SSO y personalización de token con directivas personalizadas
El uso de directivas personalizadas proporciona el mismo control sobre las configuraciones de token, sesión e inicio de sesión único (SSO) que las directivas integradas.  Para conocer la función de cada configuración, consulte la documentación [aquí](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Configuración de notificaciones y duración del token
Para cambiar la configuración de la duración del token, debe agregar un elemento `<ClaimsProviders>` en el archivo de usuario de confianza de la directiva que quiere modificar.  El elemento `<ClaimsProviders>` es secundario de `<TrustFrameworkPolicy>`.  En el parámetro, deberá colocar la información que afecta a la duración del token.  El XML es similar al de este ejemplo:

```XML
<ClaimsProviders>
   <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="JwtIssuer">
            <Metadata>
               <Item Key="token_lifetime_secs">3600</Item>
               <Item Key="id_token_lifetime_secs">3600</Item>
               <Item Key="refresh_token_lifetime_secs">1209600</Item>
               <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
               <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
               <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
            </Metadata>
         </TechnicalProfile>
      </TechnicalProfiles>
   </ClaimsProvider>
</ClaimsProviders>
```

**Duración del token de acceso**: la duración del token de acceso se puede cambiar mediante la modificación del valor `<Item>` con la clave = "token_lifetime_secs" en segundos.  El valor predeterminado en el elemento integrado es 3600 (60 minutos).

**Duración del token del identificador**: la duración del token del identificador se puede cambiar mediante la modificación del valor `<Item>` con la clave = "id_token_lifetime_secs" en segundos.  El valor predeterminado en el elemento integrado es 3600 (60 minutos).

**Duración del token de actualización**: la duración del token de actualización se puede cambiar mediante la modificación del valor `<Item>` con la clave = "refresh_token_lifetime_secs" en segundos.  El valor predeterminado en el elemento integrado es 1209600 segundos (14 días).

**Duración de la ventana deslizante del token de actualización**: si quiere establecer una duración de ventana deslizante para su token de actualización, modifique el valor `<Item>` con la clave Key="rolling_refresh_token_lifetime_secs" en segundos.  El valor predeterminado del elemento integrado es 7776000 (90 días).  Si no quiere exigir una duración de ventana deslizante, reemplace esta línea por:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Notificación de emisor (iss)**: si quiere cambiar la notificación del emisor (iss), modifique el valor `<Item>` con la clave = "IssuanceClaimPattern".  Los valores aplicables son `AuthorityAndTenantGuid` y `AuthorityWithTfp`.

**Configuración de notificación que representa el identificador de directiva**: las opciones para configurar este valor son TFTP (directiva de marco de confianza) y ACR (referencia de contexto de autenticación).  
Se recomienda establecer este valor en TFTP; para ello, asegúrese de que exista `<Item>` con la clave = "AuthenticationContextReferenceClaimPattern" y que el valor sea `None`.
En su elemento `<OutputClaims>`, agregue este elemento:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
Para ACR, quite `<Item>` con la clave= "AuthenticationContextReferenceClaimPattern".

**Notificación de asunto (sub)**: esta opción se establece de forma predeterminada en ObjectID. Si quiere cambiarla a `Not Supported`, haga lo siguiente:

Reemplace esta línea 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
por esta otra:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Comportamiento de sesión y SSO

Para cambiar las configuraciones de comportamiento de sesión y SSO, debe agregar un elemento `<UserJourneyBehaviors>` dentro del elemento `<RelyingParty>`.  El elemento `<UserJourneyBehaviors>` debe seguir inmediatamente a `<DefaultUserJourney>`.  El interior de su elemento `<UserJourneyBehavors>` debe ser similar al siguiente:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Configuración de inicio de sesión único (SSO)**: para cambiar la configuración del inicio de sesión único, debe modificar el valor de `<SingleSignOn>`.  Los valores aplicables son `Tenant`, `Application`, `Policy` y `Disabled`. 

**Duración de sesión de aplicación web (minutos)**: para cambiar la duración de la sesión de aplicación web, debe modificar el valor del elemento `<SessionExpiryInSeconds>`.  El valor predeterminado en las directivas integradas es 86400 segundos (1440 minutos).

**Tiempo de espera de sesión de aplicación web**: para cambiar el tiempo de espera de sesión de una aplicación web, debe modificar el valor de `<SessionExpiryType>`.  Los valores aplicables son `Absolute` y `Rolling`.
