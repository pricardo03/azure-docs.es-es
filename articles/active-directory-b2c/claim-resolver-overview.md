---
title: Acerca de los solucionadores de notificaciones en las directivas personalizadas de Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre cómo se usan los solucionadores de notificaciones en una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 10/08/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: dab6b87c2785d3331817d6c191be64d406683a51
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49312247"
---
# <a name="about-claim-resolvers-in-azure-active-directory-b2c-custom-policies"></a>Acerca de los solucionadores de notificaciones en las directivas personalizadas de Azure Active Directory B2C

Los solucionadores de notificaciones en [directivas personalizadas](active-directory-b2c-overview-custom.md) de Azure Active Directory (Azure AD) B2C proporcionan información de contexto sobre una solicitud de autorización, como el nombre de la directiva, el identificador de correlación de la solicitud, el idioma de la interfaz de usuario, etc.

Para utilizar un solucionador de notificaciones en una notificación de entrada o salida, se define una cadena **ClaimType** en el elemento [ClaimsSchema](claimsschema.md) y, a continuación, se establece **DefaultValue** en el solucionador de notificaciones del elemento de notificación de entrada o salida. Azure AD B2C lee el valor del solucionador de notificaciones y usa el valor en el perfil técnico. 

En el ejemplo siguiente, un tipo de notificación llamada `correlationId` se define con un **DataType** de `string`.  

```XML
<ClaimType Id="correlationId">
  <DisplayName>correlationId</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Request correlation Id</UserHelpText>
</ClaimType>
```

En el perfil técnico, asigne el solucionador de notificaciones al tipo de notificación. Azure AD B2C rellena el valor del solucionador de notificaciones `{context:corelationId}` en la notificación `correlationId` y envía la notificación al perfil técnico.

```XML
<InputClaim ClaimTypeReferenceId="correlationId" DefaultValue="{context:corelationId}" />
```

## <a name="claim-resolver-types"></a>Tipos de solucionadores de notificaciones

Las secciones siguientes enumeran los solucionadores de notificaciones disponibles.

### <a name="culture"></a>Referencia cultural

| Notificación | DESCRIPCIÓN | Ejemplo |
| ----- | ----------- | --------|
| {Culture:LanguageName} | Código ISO de dos letras para el idioma. | en |
| {Culture:LCID}   | El identificador de configuración regional del código de idioma. | 1033 |
| {Culture:RegionName} | Código ISO de dos letras para la región. | US |
| {Culture:RFC5646} | Código de idioma RFC5646. | es-ES |

### <a name="policy"></a>Directiva

| Notificación | DESCRIPCIÓN | Ejemplo |
| ----- | ----------- | --------|
| {Policy:PolicyId} | Nombre de la directiva del usuario de confianza. | B2C_1A_signup_signin |
| {Policy:RelyingPartyTenantId} | El identificador de inquilino de la directiva del usuario de confianza. | your-tenant.onmicrosoft.com |
| {Policy:TenantObjectId} | El identificador de objeto de inquilino de la directiva del usuario de confianza. | 00000000-0000-0000-0000-000000000000 |
| {Policy:TrustFrameworkTenantId} | El identificador de inquilino del marco de confianza. | your-tenant.onmicrosoft.com |

### <a name="openid-connect"></a>OpenID Connect

| Notificación | DESCRIPCIÓN | Ejemplo |
| ----- | ----------- | --------|
| {OIDC:AuthenticationContextReferences} |El parámetro de cadena de consulta `acr_values`. | N/D |
| {OIDC:ClientId} |El parámetro de cadena de consulta `client_id`. | 00000000-0000-0000-0000-000000000000 |
| {OIDC:DomainHint} |El parámetro de cadena de consulta `domain_hint`. | facebook.com |
| {OIDC:LoginHint} |  El parámetro de cadena de consulta `login_hint`. | someone@contoso.com |
| {OIDC:MaxAge} | El parámetro de cadena de consulta `max_age`. | N/D |
| {OIDC:Nonce} |El parámetro de cadena de consulta `Nonce`. | defaultNonce |
| {OIDC:Prompt} | El parámetro de cadena de consulta `prompt`. | inicio de sesión |
| {OIDC:Resource} |El parámetro de cadena de consulta `resource`. | N/D |
| {OIDC:scope} |El parámetro de cadena de consulta `scope`. | openid |

### <a name="context"></a>Context

| Notificación | DESCRIPCIÓN | Ejemplo |
| ----- | ----------- | --------|
| {Context:BuildNumber} | Versión del marco de experiencia de identidad (número de compilación).  | 1.0.507.0 |
| {Context:CorrelationId} | Identificador de correlación.  | 00000000-0000-0000-0000-000000000000 |
| {Context:DateTimeInUtc} |Hora y fecha en UTC.  | 10/10/2018 12:00:00 p. m. |
| {Context:DeploymentMode} |Modo de implementación de la directiva.  | Producción |
| {Context:IPAddress} | Dirección IP del usuario. | 11.111.111.11 |


### <a name="non-protocol-parameters"></a>Parámetros sin protocolo

Cualquier nombre de parámetro incluido como parte de una solicitud OIDC u OAuth2 se puede asignar a una notificación en el recorrido del usuario. Por ejemplo, la solicitud de la aplicación puede incluir un parámetro de cadena de consulta con el nombre de `app_session`, `loyalty_number` o cualquier cadena de consulta personalizada.

| Notificación | DESCRIPCIÓN | Ejemplo |
| ----- | ----------------------- | --------|
| {OAUTH-KV:campaignId} | Parámetro de cadena de consulta. | hawaii |
| {OAUTH-KV:app_session} | Parámetro de cadena de consulta. | A3C5R |
| {OAUTH-KV:loyalty_number} | Parámetro de cadena de consulta. | 1234 |
| {OAUTH-KV:any custom query string} | Parámetro de cadena de consulta. | N/D |


## <a name="how-to-use-claim-resolvers"></a>Uso de los solucionadores de notificaciones

### <a name="restful-technical-profile"></a>Perfil técnico de RESTful

En un perfil técnico de [RESTful](restful-technical-profile.md), es posible que desee enviar el idioma del usuario, el nombre de la directiva, el ámbito y el identificador de cliente. Según estas notificaciones, la API REST puede ejecutar la lógica de negocios personalizada y, si es necesario, generar un mensaje de error localizado. 

En el ejemplo siguiente se muestra un perfil técnico de RESTful:

```XML
<TechnicalProfile Id="REST">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app.azurewebsites.net/api/identity</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userLanguage" DefaultValue="{Culture:LCID}" />
    <InputClaim ClaimTypeReferenceId="policyName" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="scope" DefaultValue="{OIDC:scope}" />
    <InputClaim ClaimTypeReferenceId="clientId" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

### <a name="direct-sign-in"></a>Inicio de sesión directo

Al usar solucionadores de notificaciones, puede rellenar previamente el nombre de inicio de sesión o puede iniciar sesión directamente en un proveedor de identidades sociales específico, como Facebook, LinkedIn o una cuenta de Microsoft. Para más información, consulte [Configuración de inicio de sesión directo con Azure Active Directory B2C](direct-signin.md).

### <a name="dynamic-ui-customization"></a>Personalización de la interfaz de usuario dinámica

Azure AD B2C le permite pasar parámetros de cadena de consulta a los puntos de conexión de la definición de contenido HTML, lo que permite representar dinámicamente el contenido de la página. Por ejemplo, puede cambiar la imagen de fondo en la página de inicio de sesión o de registro de Azure AD B2C en función de un parámetro personalizado que se pasa desde la aplicación web o dispositivo móvil. Para más información, consulte [Azure Active Directory B2C: configuración de la interfaz de usuario con contenido dinámico utilizando directivas personalizadas](active-directory-b2c-ui-customization-custom-dynamic.md). También puede localizar la página HTML basándose en un parámetro de idioma, o bien puede cambiar el contenido basándose en el identificador de cliente.

El ejemplo siguiente pasa en la cadena de consulta un parámetro denominado **campaignId** con un valor de `hawaii`, un código de **idioma** de `en-US` y una **aplicación** que representa el identificador de cliente:

```XML
<UserJourneyBehaviors>
  <ContentDefinitionParameters>
    <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
    <Parameter Name="language">{Culture:RFC5646}</Parameter>
    <Parameter Name="app">{OIDC:ClientId}</Parameter>
  </ContentDefinitionParameters>
</UserJourneyBehaviors>
```

Como resultado, Azure AD B2C envía los parámetros anteriores a la página de contenido HTML:

```
/selfAsserted.aspx?campaignId=hawaii&language=en-US&app=0239a9cc-309c-4d41-87f1-31288feb2e82
```

### <a name="application-insights-technical-profile"></a>Perfil técnico de Application Insights

Con Azure Application Insights y los solucionadores de notificaciones, puede obtener información sobre el comportamiento del usuario. En el perfil técnico de Application Insights, envía notificaciones de entrada que se conservan en Azure Application Insights. Para más información, consulte [Seguimiento del comportamiento del usuario dentro de los recorridos de Azure AD B2C mediante Application Insights](active-directory-b2c-custom-guide-eventlogger-appins.md). El ejemplo siguiente envía el identificador de directiva, el identificador de correlación, el idioma y el identificador de cliente a Azure Application Insights.

```XML
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Alternate Email</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
    <InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
    <InputClaim ClaimTypeReferenceId="AppId" PartnerClaimType="{property:App}" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
</TechnicalProfile>
```
