---
title: Definición de un perfil técnico autoafirmado en una directiva personalizada en Azure Active Directory B2C | Microsoft Docs
description: Defina un perfil técnico autoafirmado en una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: dcc94daeb19174b85fface05222f8842e9544adf
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188871"
---
# <a name="define-a-self-asserted-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Definición de un perfil técnico autoafirmado en una directiva personalizada en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Todas las interacciones realizadas en Azure Active Directory (Azure AD) B2C en las que se espera que el usuario proporcione una entrada son perfiles técnicos autoafirmados. Por ejemplo, una página de registro, de inicio de sesión o de restablecimiento de la contraseña.

## <a name="protocol"></a>Protocolo

El atributo **Name** del elemento **Protocol** tiene que establecerse en `Proprietary`. El atributo **handler** debe contener el nombre completo del ensamblado de controlador de protocolo que usa Azure AD B2C para la autoafirmación: `Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

En el ejemplo siguiente se muestra un perfil técnico autoafirmado para el registro de un correo electrónico:

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
```
 
## <a name="input-claims"></a>Notificaciones de entrada

En un perfil técnico autoafirmado, puede usar los elementos **InputClaims** y **InputClaimsTransformations** para rellenar previamente el valor de las notificaciones que aparecen en la página autoafirmada (notificaciones de salida). Por ejemplo, en la directiva del perfil de edición, el recorrido del usuario lee primero el perfil de usuario desde el servicio de directorio de Azure AD B2C y, después, el perfil técnico autoafirmado establece las notificaciones de entrada con los datos de usuario almacenados en el perfil de usuario. Estas notificaciones se recopilan desde el perfil de usuario y se presentan al usuario, que después puede editar los datos existentes.

```XML
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <InputClaim ClaimTypeReferenceId="userPrincipalName" />
    <InputClaim ClaimTypeReferenceId="givenName" />
    <InputClaim ClaimTypeReferenceId="surname" />
  </InputClaims>
```


## <a name="output-claims"></a>Notificaciones de salida

El elemento **OutputClaims** contiene una lista de las notificaciones que se van a presentar para recopilar datos del usuario. Para rellenar previamente las notificaciones de salida con algunos valores, use las notificaciones de entrada que se describieron anteriormente. El elemento también puede incluir un valor predeterminado. El orden de las notificaciones en **OutputClaims** controla el orden en que Azure AD B2C presenta las notificaciones en la pantalla. El atributo **DefaultValue** solo tiene efecto si la notificación no se ha establecido antes. En cambio, si se ha establecido antes en un paso de orquestación anterior, incluso si el usuario deja el valor vacío, el valor predeterminado no surtirá efecto. Para forzar el uso de un valor predeterminado, establezca el atributo **AlwaysUseDefaultValue** en `true`. Para forzar que el usuario proporcione un valor para una notificación de salida específica, establezca el atributo **Required** del elemento **OutputClaims** en `true`.

El elemento **ClaimType** de la colección **OutputClaims** necesita establecer el elemento **UserInputType** en cualquier tipo de entrada de usuario admitido por Azure AD B2C, como `TextBox`o `DropdownSingleSelect`. O el elemento **OutputClaim** debe establecer un **DefaultValue**.  

El elemento **OutputClaimsTransformations** puede contener una colección de elementos **OutputClaimsTransformation** que se usan para modificar las notificaciones de salida o generar otras nuevas.

La siguiente notificación de salida siempre se establece en `live.com`:

```XML
<OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" AlwaysUseDefaultValue="true" />
```

### <a name="use-case"></a>Caso de uso

Hay cuatro escenarios para las notificaciones de salida:

- **Recopilar las notificaciones de salida del usuario**: cuando tenga que recopilar información del usuario, como la fecha de nacimiento, debe agregar la notificación a la colección **OutputClaims**. Las notificaciones que se presentan al usuario deben especificar el **UserInputType**, como `TextBox` o `DropdownSingleSelect`. Si el perfil técnico autoafirmado contiene un perfil técnico de validación que da como resultado la misma notificación, Azure AD B2C no presenta la notificación al usuario. Si no hay ninguna solicitud de salida más para presentar al usuario, Azure AD B2C omite el perfil técnico.
- **Establecer un valor predeterminado en una notificación de salida**: sin recopilar datos del usuario ni devolver los datos desde el perfil técnico de validación. El perfil técnico autoafirmado `LocalAccountSignUpWithLogonEmail` establece la notificación **executed-SelfAsserted-Input** en `true`.
- **Un perfil técnico de validación devuelve las notificaciones de salida**: su perfil técnico puede llamar a un perfil técnico de validación que devuelve algunas notificaciones. Es posible que desee propagar las notificaciones y devolverlas a los siguientes pasos de orquestación en el recorrido del usuario. Por ejemplo, al iniciar sesión con una cuenta local, el perfil técnico autoafirmado denominado `SelfAsserted-LocalAccountSignin-Email` llama al perfil técnico de validación denominado `login-NonInteractive`. Este perfil técnico valida las credenciales de usuario y también devuelve el perfil de usuario. Por ejemplo, “userPrincipalName”, “displayName”, “givenName” y “surName”.
- **Salida de las notificaciones a través de la transformación de notificaciones de salida**

En el ejemplo siguiente, el perfil técnico autoafirmado `LocalAccountSignUpWithLogonEmail` muestra el uso de notificaciones de salida y establece **executed-SelfAsserted-Input** en `true`. Las notificaciones `objectId`, `authenticationSource` y `newUser` son el resultado del perfil de validación técnica `AAD-UserWriteUsingLogonEmail` y no se muestran al usuario.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    <Item Key="language.button_continue">Create</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
  </CryptographicKeys>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
    <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
    <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
    <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
    <OutputClaim ClaimTypeReferenceId="authenticationSource" />
    <OutputClaim ClaimTypeReferenceId="newUser" />

    <!-- Optional claims, to be collected from the user -->
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surName" />
  </OutputClaims>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
  </ValidationTechnicalProfiles>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>

```

## <a name="persist-claims"></a>Conservar las notificaciones

Si el elemento **PersistedClaims** no está presente, el perfil técnico autoafirmado no conserva los datos en Azure AD B2C. En cambio, se realiza una llamada a un perfil técnico de validación que es responsable de conservar los datos. Por ejemplo, la directiva de registro usa el perfil técnico autoafirmado `LocalAccountSignUpWithLogonEmail` para recopilar el nuevo perfil de usuario. El perfil técnico `LocalAccountSignUpWithLogonEmail` llama al perfil técnico de validación para crear la cuenta en Azure AD B2C.

## <a name="validation-technical-profiles"></a>Perfiles técnicos de validación

Los perfiles técnicos de validación se usan para validar algunas o todas las notificaciones de salida del perfil técnico al que hace referencia. Las notificaciones de entrada del perfil técnico de validación de entrada deben aparecer en las notificaciones de salida del perfil técnico autoafirmado. El perfil técnico de validación valida la entrada del usuario y puede devolver un error al usuario. 

El perfil técnico de validación puede ser cualquier perfil técnico de la directiva, como perfiles técnicos de [Azure Active Directory](active-directory-technical-profile.md) o de una [API de REST](restful-technical-profile.md). En el ejemplo anterior, el perfil técnico `LocalAccountSignUpWithLogonEmail` valida que signinName no exista en el directorio. En caso contrario, el perfil técnico de validación crea una cuenta local y devuelve los valores objectId, authenticationSource y newUser. El perfil técnico `SelfAsserted-LocalAccountSignin-Email` llama al perfil técnico de validación `login-NonInteractive` para validar las credenciales del usuario.

También puede llamar a un perfil técnico de la API de REST con la lógica de negocios, sobrescribir notificaciones de entrada o enriquecer los datos del usuario mediante una integración adicional de una aplicación de línea de negocio corporativa. Para obtener más información, vea [Perfil técnico de validación](validation-technical-profile.md).

## <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| setting.showContinueButton | Sin  | Muestra el botón para continuar. Valores posibles: `true` (opción predeterminada) o `false` |
| setting.showCancelButton | Sin  | Muestra el botón para cancelar. Valores posibles: `true` (opción predeterminada) o `false` |
| setting.operatingMode | Sin  | En una página de inicio de sesión, esta propiedad controla el comportamiento del campo de nombre de usuario, como la validación de entrada y los mensajes de error. Valores esperados: `Username` o `Email`. |
| ContentDefinitionReferenceId | SÍ | El identificador de la [definición de contenido](contentdefinitions.md) asociada a este perfil técnico. |
| EnforceEmailVerification | Sin  | Para registrarse o editar el perfil, exige la comprobación del correo electrónico. Valores posibles: `true` (opción predeterminada) o `false`. | 
| setting.showSignupLink | Sin  | Muestra el botón para registrarse. Valores posibles: `true` (opción predeterminada) o `false` |
| setting.retryLimit | Sin  | Controla el número de veces que un usuario puede intentar proporcionar los datos que se comprueban con un perfil técnico de validación. Por ejemplo, si un usuario intenta registrarse con una cuenta que ya existe y sigue intentándolo hasta que alcance el límite.
| SignUpTarget | Sin  | Identificador de intercambio de destinos del registro. Cuando el usuario hace clic en el botón de registro, Azure AD B2C ejecuta el identificador de intercambio especificado. |

## <a name="cryptographic-keys"></a>Claves de cifrado

El elemento **CryptographicKeys** no se usa.













