---
title: TechnicalProfiles | Microsoft Docs
description: Especifique el elemento TechnicalProfiles de una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 45ea2a28b4b2fb3d55d7ae949152e6f51b5d3162
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566540"
---
# <a name="technicalprofiles"></a>TechnicalProfiles

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Un elemento **TechnicalProfiles** contiene un conjunto de perfiles técnicos admitidos por el proveedor de notificaciones. Cada proveedor de notificaciones necesita tener uno o más perfiles técnicos que determinen los puntos de conexión y protocolos necesarios para establecer comunicación con el proveedor de notificaciones. Un proveedor de notificaciones puede tener varios perfiles técnicos.

```XML
<ClaimsProvider>
  <DisplayName>Display name</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Technical profile identifier">
      <DisplayName>Display name of technical profile</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">URL of service</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
      </Metadata>
      <InputTokenFormat>JWT</InputTokenFormat>
      <OutputTokenFormat>JWT</OutputTokenFormat>
      <CryptographicKeys>
        <Key ID="Key identifier" StorageReferenceId="Storage key container identifier"/>
        ...
      </CryptographicKeys>
      <InputClaimsTransformations>
        <InputClaimsTransformation ReferenceId="Claims transformation identifier" />
        ...
      <InputClaimsTransformations>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" DefaultValue="givenName" PartnerClaimType="firstName" />
        ...
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="givenName" DefaultValue="givenName" PartnerClaimType="firstName" />
        ...
      </PersistedClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" DefaultValue="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="Claims transformation identifier" />
        ...
      <OutputClaimsTransformations>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="Technical profile identifier" />
        ...
      </ValidationTechnicalProfiles>
      <SubjectNamingInfo ClaimType="Claim type identifier" />
      <IncludeTechnicalProfile ReferenceId="Technical profile identifier" />
      <UseTechnicalProfileForSessionManagement ReferenceId="Technical profile identifier" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

El elemento **TechnicalProfile** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
|---------|---------|---------|
| Id | SÍ | Un identificador único del perfil técnico. Se puede hacer referencia al perfil técnico con este identificador desde otros elementos del archivo de directiva. Por ejemplo, **OrchestrationSteps** y **ValidationTechnicalProfile**. |

El elemento **TechnicalProfile** contiene los elementos siguientes:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| Dominio | 0:1 | Nombre de dominio del perfil técnico. Por ejemplo, si el perfil técnico especifica el proveedor de identidades de Facebook, el nombre de dominio será Facebook.com. |
| DisplayName | 0:1 | Nombre del perfil técnico que puede mostrarse a los usuarios. |
| DESCRIPCIÓN | 0:1 | Descripción del perfil técnico que puede mostrarse a los usuarios. |
| Protocolo | 0:1 | Protocolo usado para la comunicación con la otra parte. |
| Metadatos | 0:1 | Colección de pares de clave y valor usada por el protocolo para comunicarse con el punto de conexión durante una transacción. |
| InputTokenFormat | 0:1 | Formato del token de entrada. Valores posibles: `JSON`, `JWT`, `SAML11` o `SAML2`. El valor `JWT` representa un token de JSON Web Token según la especificación IETF. El valor `SAML11` representa un token de seguridad SAML 1.1 según la especificación OASIS.  El valor `SAML2` representa un token de seguridad SAML 2.0 según la especificación OASIS. |
| OutputTokenFormat | 0:1 | Formato del token de salida. Valores posibles: `JSON`, `JWT`, `SAML11` o `SAML2`. |
| CryptographicKeys | 0:1 | Lista de claves criptográficas que se usan en el perfil técnico. |
| InputClaimsTransformations | 0:1 | Lista de referencias (definidas anteriormente) a transformaciones de notificaciones que tienen que ejecutarse antes de enviar notificaciones al proveedor de notificaciones o al usuario de confianza. |
| InputClaims | 0:1 | Lista de referencias (definidas anteriormente) a tipos de notificación que se usan como entrada en el perfil técnico. |
| PersistedClaims | 0:1 | Lista de referencias (definidas anteriormente) a tipos de notificación que el proveedor de notificaciones hace persistir y que están relacionadas con el perfil técnico. |
| OutputClaims | 0:1 | Lista de referencias (definidas anteriormente) a tipos de notificación que se usan como salida en el perfil técnico. |
| OutputClaimsTransformations | 0:1 | Lista de referencias (definidas anteriormente) a transformaciones de notificaciones que tienen que ejecutarse cuando el proveedor de notificaciones reciba las notificaciones. |
| ValidationTechnicalProfiles | 0:n | Lista de referencias a otros perfiles técnicos que el perfil técnico usa con fines de validación. Para obtener más información, vea [Validación del perfil técnico](validation-technical-profile.md).|
| SubjectNamingInfo | 0:1 | Controla la producción del nombre de firmante en los tokens donde el nombre de firmante se especifica por separado de las notificaciones. Por ejemplo, OAuth o SAML.  |
| IncludeClaimsFromTechnicalProfile | 0:1 | Identificador de un perfil técnico cuyas notificaciones de entrada o salida quiere que se agreguen a este perfil técnico. El perfil técnico al que se haga referencia tiene que definirse en el mismo archivo de directiva. | 
| IncludeTechnicalProfile |0:1 | Identificador de un perfil técnico cuyos datos quiere agregar a este perfil técnico. El perfil técnico al que se haga referencia tiene que existir en el mismo archivo de directiva. |
| UseTechnicalProfileForSessionManagement | 0:1 | Perfil técnico distinto que se usará para la administración de sesiones. |
|EnabledForUserJourneys| 0:1 |Controla si el perfil técnico se ejecuta en un recorrido del usuario.  |

### <a name="protocol"></a>Protocolo

El elemento **Protocol** contiene los atributos siguientes:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| NOMBRE | SÍ | El nombre de un protocolo válido admitido por Azure AD B2C que se usará como parte del perfil técnico. Valores posibles: `OAuth1`, `OAuth2`, `SAML2`, `OpenIdConnect`, `WsFed`, `WsTrust`, `Proprietary`, `session management`, `self-asserted` o `None`. |
| Controlador | Sin  | Cuando el nombre del protocolo se establece en `Proprietary`, especifique el nombre completo del ensamblado que usará Azure AD B2C para determinar el controlador de protocolo. |

### <a name="metadata"></a>Metadatos

Un elemento **Metadata** contiene los elementos siguientes:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| item | 0:n | Los metadatos relacionados con el perfil técnico. Cada tipo de perfil técnico tiene un conjunto distinto de elementos de metadatos. Para obtener más información, vea la sección sobre los tipos de perfil técnico. |

#### <a name="item"></a>item

El elemento **Item** del elemento **Metadata** contiene los atributos siguientes:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| Clave | SÍ | La clave de metadatos. Puede ver la lista de elementos de metadatos en cada tipo de perfil técnico. |

### <a name="cryptographickeys"></a>CryptographicKeys

El elemento **CryptographicKeys** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| Clave | 1:n | Una clave criptográfica usada en este perfil técnico. |

#### <a name="key"></a>Clave

El elemento **Key** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| Id | Sin  | Un identificador único de un par de claves específico al que se hace referencia desde otros elementos en el archivo de directiva. |
| StorageReferenceId | SÍ | Un identificador del contenedor de claves de almacenamiento al que se hace referencia desde otros elementos en el archivo de directiva. |

### <a name="inputclaimstransformations"></a>InputClaimsTransformations

El elemento **InputClaimsTransformations** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| InputClaimsTransformation | 1:n | El identificador de una transformación de notificaciones que tiene que ejecutarse antes de enviar notificaciones al proveedor de notificaciones o al usuario de confianza. Una transformación de notificaciones puede usarse para modificar notificaciones ClaimsSchema existentes o para generar nuevas. |

#### <a name="inputclaimstransformation"></a>InputClaimsTransformation

El elemento **InputClaimsTransformation** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ReferenceId | SÍ | Un identificador de una transformación de notificaciones que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

### <a name="inputclaims"></a>InputClaims

El elemento **InputClaims** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| InputClaim | 1:n | Un tipo de notificación de entrada esperado. |

#### <a name="inputclaim"></a>InputClaim 

El elemento **InputClaim** contiene los atributos siguientes:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | SÍ | El identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva o del archivo de directiva principal. |
| DefaultValue | Sin  | Un valor predeterminado que se usará para crear una notificación si la notificación indicada por ClaimTypeReferenceId no existe, por lo que la notificación resultante puede usarse como un elemento InputClaim por el perfil técnico. |
| PartnerClaimType | Sin  | El identificador del tipo de notificación del socio externo al que se asigna el tipo de notificación de directiva especificado. Si no se especifica el atributo PartnerClaimType, el tipo de notificación de directiva especificado se asignará al tipo de notificación del socio que tenga el mismo nombre. Use esta propiedad cuando el nombre del tipo de notificación sea distinto de la otra entidad. Por ejemplo, el nombre de la primera notificación es “givenName”, mientras que el socio usa una notificación denominada “first_name”. |

### <a name="persistedclaims"></a>PersistedClaims

El elemento **PersistedClaims** contiene los elementos siguientes:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| PersistedClaim | 1:n | El tipo de notificación que persistirá. |

#### <a name="persistedclaim"></a>PersistedClaim 

El elemento **PersistedClaim** contiene los atributos siguientes:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | SÍ | El identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva o del archivo de directiva principal. |
| DefaultValue | Sin  | Un valor predeterminado que se usará para crear una notificación si la notificación indicada por ClaimTypeReferenceId no existe, por lo que la notificación resultante puede usarse como un elemento InputClaim por el perfil técnico. |
| PartnerClaimType | Sin  | El identificador del tipo de notificación del socio externo al que se asigna el tipo de notificación de directiva especificado. Si no se especifica el atributo PartnerClaimType, el tipo de notificación de directiva especificado se asignará al tipo de notificación del socio que tenga el mismo nombre. Use esta propiedad cuando el nombre del tipo de notificación sea distinto de la otra entidad. Por ejemplo, el nombre de la primera notificación es “givenName”, mientras que el socio usa una notificación denominada “first_name”. |

### <a name="outputclaims"></a>OutputClaims

El elemento **OutputClaims** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| OutputClaim | 1:n | Un tipo de notificación de salida esperado. |

#### <a name="outputclaim"></a>OutputClaim 

El elemento **OutputClaim** contiene los atributos siguientes:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | SÍ | El identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva o del archivo de directiva principal. |
| DefaultValue | Sin  | Un valor predeterminado que se usará para crear una notificación si la notificación indicada por ClaimTypeReferenceId no existe, por lo que la notificación resultante puede usarse como un elemento InputClaim por el perfil técnico. |
|AlwaysUseDefaultValue |Sin  |Obliga a usar el valor predeterminado.  |
| PartnerClaimType | Sin  | El identificador del tipo de notificación del socio externo al que se asigna el tipo de notificación de directiva especificado. Si no se especifica el atributo PartnerClaimType, el tipo de notificación de directiva especificado se asignará al tipo de notificación del socio que tenga el mismo nombre. Use esta propiedad cuando el nombre del tipo de notificación sea distinto de la otra entidad. Por ejemplo, el nombre de la primera notificación es “givenName”, mientras que el socio usa una notificación denominada “first_name”. |

### <a name="outputclaimstransformations"></a>OutputClaimsTransformations

El elemento **OutputClaimsTransformations** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| OutputClaimsTransformation | 1:n | Los identificadores de transformaciones de notificaciones que tienen que ejecutarse antes de enviar notificaciones al proveedor de notificaciones o al usuario de confianza. Una transformación de notificaciones puede usarse para modificar notificaciones ClaimsSchema existentes o para generar nuevas. |

#### <a name="outputclaimstransformation"></a>OutputClaimsTransformation

El elemento **OutputClaimsTransformation** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ReferenceId | SÍ | Un identificador de una transformación de notificaciones que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

### <a name="validationtechnicalprofiles"></a>ValidationTechnicalProfiles

El elemento **ValidationTechnicalProfiles** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| ValidationTechnicalProfile | 1:n | Los identificadores de perfiles técnicos que se usan para validar algunas o todas las notificaciones de salida del perfil técnico al que se hace referencia. Todas las notificaciones de entrada del perfil técnico al que se hace referencia tienen que aparecer en las notificaciones de salida del perfil técnico al que se hace referencia. |

#### <a name="validationtechnicalprofile"></a>ValidationTechnicalProfile

El elemento **ValidationTechnicalProfile** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ReferenceId | SÍ | Un identificador de un perfil técnico que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

###  <a name="subjectnaminginfo"></a>SubjectNamingInfo

El elemento **SubjectNamingInfo** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ClaimType | SÍ | Un identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva. |

### <a name="includetechnicalprofile"></a>IncludeTechnicalProfile

El elemento **IncludeTechnicalProfile** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ReferenceId | SÍ | Un identificador de un perfil técnico que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

### <a name="usetechnicalprofileforsessionmanagement"></a>UseTechnicalProfileForSessionManagement

El elemento **UseTechnicalProfileForSessionManagement** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| ReferenceId | SÍ | Un identificador de un perfil técnico que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

### <a name="enabledforuserjourneys"></a>EnabledForUserJourneys
El elemento **ClaimsProviderSelections** en un recorrido del usuario define la lista de opciones de selección de proveedor de notificaciones y el orden en que se mostrarán. Con el elemento **EnabledForUserJourneys**, puede filtrar los proveedores de notificaciones que estarán disponibles para el usuario. El elemento **EnabledForUserJourneys** contiene uno de los valores siguientes:

- **Always**: ejecuta el perfil técnico.
- **Never**: omite el perfil técnico. 
- **OnClaimsExistence**: solo se ejecuta cuando existe una notificación específica (indicada en el perfil técnico). 
- **OnItemExistenceInStringCollectionClaim**: solo se ejecuta cuando existe un elemento en una notificación de colección de cadenas. 
- **OnItemAbsenceInStringCollectionClaim**: solo se ejecuta cuando no existe un elemento en una notificación de colección de cadenas.

Para usar **OnClaimsExistence**, **OnItemExistenceInStringCollectionClaim**, o **OnItemAbsenceInStringCollectionClaim**, necesita especificar los metadatos siguientes: **ClaimTypeOnWhichToEnable** especifica el tipo de notificación que se evaluará y **ClaimValueOnWhichToEnable** especifica el valor con el que se comparará.

El siguiente perfil técnico solo se ejecuta si la colección de cadenas **identityProviders** contiene el valor de `facebook.com`:

```XML
<TechnicalProfile Id="UnLink-Facebook-OAUTH">
  <DisplayName>Unlink Facebook</DisplayName>
...
    <Metadata>
        <Item Key="ClaimTypeOnWhichToEnable">identityProviders</Item>
        <Item Key="ClaimValueOnWhichToEnable">facebook.com</Item>
    </Metadata>        
...
  <EnabledForUserJourneys>OnItemExistenceInStringCollectionClaim</EnabledForUserJourneys>
</TechnicalProfile>  
```












