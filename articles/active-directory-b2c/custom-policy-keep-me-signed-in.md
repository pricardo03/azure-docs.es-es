---
title: Mantener la sesión iniciada en Azure Active Directory B2C
description: Aprenda a configurar Mantener la sesión iniciada (KMSI) en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 052e1bc4e9a14b34e21b0bfeb4193fbd0b2b1a22
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76851067"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Habilitación de Mantener la sesión iniciada (KMSI) en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Puede habilitar la funcionalidad Mantener la sesión iniciada (KMSI) para los usuarios y las aplicaciones nativas que tienen cuentas locales en el directorio de Azure Active Directory B2C (Azure AD B2C). Esta característica concede acceso a los usuarios que vuelven a la aplicación sin pedirles que escriban de nuevo el nombre de usuario y la contraseña. Este acceso se revocará cuando el usuario cierre la sesión.

Los usuarios no deben habilitar esta opción en equipos públicos.

![Ejemplo de página de inicio de sesión de registro que muestra la casilla Mantener la sesión iniciada](./media/custom-policy-keep-me-signed-in/kmsi.PNG)

## <a name="prerequisites"></a>Prerequisites

Un inquilino de Azure AD B2C configurado para permitir el inicio de sesión en la cuenta local. KMSI no es compatible con cuentas de proveedor de identidades externo.

Si no dispone de un inquilino, puede crear uno con los pasos de [Tutorial: Creación de un inquilino de Azure Active Directory B2C](tutorial-create-tenant.md).

## <a name="add-a-content-definition-element"></a>Incorporación de un elemento de la definición de contenido

En el elemento **BuildingBlocks** del archivo de extensión, agregue un elemento **ContentDefinitions**.

1. En el elemento **ContentDefinitions**, agregue un elemento **ContentDefinition** con un identificador de `api.signuporsigninwithkmsi`.
2. En el elemento **ContentDefinition**, agregue los elementos **LoadUri**, **RecoveryUri** y **DataUri**. El valor `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` del elemento **DataUri** es un identificador comprensible para la máquina que abrirá una casilla para KMSI en las páginas de inicio de sesión. Este valor no se puede cambiar.

    ```XML
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsigninwithkmsi">
          <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

## <a name="add-a-sign-in-claims-provider-for-a-local-account"></a>Incorporación de un proveedor de notificaciones de inicio de sesión de la cuenta local

Puede definir el inicio de sesión de la cuenta local como proveedor de notificaciones con el elemento **ClaimsProvider** en el archivo de extensión de la directiva:

1. Abra el archivo *TrustFrameworkExtensions.xml* desde el directorio de trabajo.
2. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz. El [paquete de inicio](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) incluye un proveedor de notificaciones de inicio de sesión de la cuenta local.
3. Agregue un elemento **ClaimsProvider** con los elementos **DisplayName** y **TechnicalProfile** tal y como se muestra en el ejemplo siguiente:

    ```XML
    <ClaimsProviders>
      <ClaimsProvider>
        <DisplayName>Local Account SignIn</DisplayName>
        <TechnicalProfiles>
          <TechnicalProfile Id="login-NonInteractive">
            <Metadata>
              <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
              <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
            </Metadata>
            <InputClaims>
              <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
              <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
            </InputClaims>
          </TechnicalProfile>
        </TechnicalProfiles>
      </ClaimsProvider>
    </ClaimsProviders>
    ```

### <a name="add-the-application-identifiers-to-your-custom-policy"></a>Incorporación de identificadores de aplicación a la directiva personalizada

Agregue los identificadores de aplicación al archivo *TrustFrameworkExtensions.xml*.

1. En el archivo *TrustFrameworkExtensions.xml*, busque el elemento **TechnicalProfile** con el identificador de `login-NonInteractive` y el elemento **TechnicalProfile** con un identificador de `login-NonInteractive-PasswordChange` y reemplace todos los valores de `IdentityExperienceFrameworkAppId` por el identificador de aplicación de la aplicación Marco de experiencia de identidad tal y como se describe en [Introducción](custom-policy-get-started.md).

    ```XML
    <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
    ```

2. Reemplace todos los valores de `ProxyIdentityExperienceFrameworkAppId` por el identificador de aplicación de la aplicación Marco de experiencia de identidad del proxy tal y como se describe en la [Introducción](custom-policy-get-started.md).
3. Guarde el archivo de extensiones.

## <a name="create-a-kmsi-enabled-user-journey"></a>Creación de un recorrido del usuario habilitado para KMSI

Agregue el proveedor de notificaciones de inicio de sesión de una cuenta local al recorrido del usuario.

1. Abra el archivo base de la directiva. Por ejemplo, *TrustFrameworkBase.xml*.
2. Busque el elemento **UserJourneys** y copie todo el contenido del elemento **UserJourney** que usa el identificador de `SignUpOrSignIn`.
3. Abra el archivo de extensión. Por ejemplo, *TrustFrameworkExtensions.xml*, y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Pegue todo el elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
5. Cambie el valor del identificador del nuevo recorrido del usuario. Por ejemplo, `SignUpOrSignInWithKmsi`.
6. Por último, en el primer paso de orquestación, cambie el valor de **ContentDefinitionReferenceId** a `api.signuporsigninwithkmsi`. El establecimiento de este valor habilita la casilla en el recorrido del usuario.
7. Guarde y cargue el archivo de extensión y asegúrese de que el resultado de todas las comprobaciones es satisfactorio.

    ```XML
    <UserJourneys>
      <UserJourney Id="SignUpOrSignInWithKmsi">
        <OrchestrationSteps>
          <OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsigninwithkmsi">
            <ClaimsProviderSelections>
              <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
            <ClaimsExchanges>
              <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="2" Type="ClaimsExchange">
            <Preconditions>
              <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
                <Value>objectId</Value>
                <Action>SkipThisOrchestrationStep</Action>
              </Precondition>
            </Preconditions>
            <ClaimsExchanges>
              <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <!-- This step reads any user attributes that we may not have received when in the token. -->
          <OrchestrationStep Order="3" Type="ClaimsExchange">
            <ClaimsExchanges>
              <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
        </OrchestrationSteps>
        <ClientDefinition ReferenceId="DefaultWeb" />
      </UserJourney>
    </UserJourneys>
    ```

## <a name="create-a-relying-party-file"></a>Creación de un archivo de usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, *SignUpOrSignInWithKmsi.xml*.
2. Abra el nuevo archivo y actualice el atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Este es el nombre de la directiva. Por ejemplo, `SignUpOrSignInWithKmsi`.
3. Cambie el atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado. Por ejemplo, `SignUpOrSignInWithKmsi`.

    KMSI se configura mediante el elemento **UserJourneyBehaviors** con **SingleSignOn**, **SessionExpiryType** y **SessionExpiryInSeconds** como sus primeros elementos secundarios. El atributo **KeepAliveInDays** controla el tiempo que permanece conectado el usuario. En el ejemplo siguiente, la sesión de KMSI expira automáticamente tras `7` días con independencia de la frecuencia con la que el usuario realiza la autenticación en modo silencioso. El establecimiento del valor **KeepAliveInDays** en `0` desactiva la funcionalidad KMSI. De manera predeterminada, este valor es `0`. Si el valor de **SessionExpiryType** es `Rolling`, la sesión de KMSI se amplía en `7` días cada vez que el usuario realiza la autenticación silenciosa.  Si `Rolling` está seleccionado, debe mantener el número de días al mínimo.

    El valor de **SessionExpiryInSeconds** representa la hora de expiración de una sesión de inicio de sesión único (SSO). Azure AD B2C lo usa internamente para comprobar si la sesión de KMSI ha expirado o no. El valor de **KeepAliveInDays** determina el valor de Max-Age y Expires de la cookie de SSO en el explorador web. A diferencia de **SessionExpiryInSeconds**, **KeepAliveInDays** se usa para evitar que el explorador borre la cookie cuando se cierre. Un usuario solo puede iniciar sesión en modo silencioso si existe la cookie de sesión de SSO, lo que controla **KeepAliveInDays**, y no ha expirado, lo que controla **SessionExpiryInSeconds**.

    Si un usuario no habilita **Keep me signed in** (Mantener la sesión iniciada) en la página de registro e inicio de sesión, la sesión expira una vez transcurrido el tiempo indicado por **SessionExpiryInSeconds** o cuando se cierra el explorador. Si un usuario habilita **Keep me signed in** (Mantener la sesión iniciada), el valor de **KeepAliveInDays** reemplaza el valor de **SessionExpiryInSeconds** y dicta el tiempo de expiración de la sesión. Aunque los usuarios cierren el explorador y vuelvan a abrirlo, pueden seguir con la sesión iniciada en modo silencioso siempre que el tiempo esté dentro del establecido por **KeepAliveInDays**. Se recomienda que establezca el valor de **SessionExpiryInSeconds** para un período breve (1200 segundos), mientras que el valor de **KeepAliveInDays** se puede establecer en un período relativamente largo (7 días), como se muestra en el ejemplo siguiente:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
      <UserJourneyBehaviors>
        <SingleSignOn Scope="Tenant" KeepAliveInDays="7" />
        <SessionExpiryType>Absolute</SessionExpiryType>
        <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
      </UserJourneyBehaviors>
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="displayName" />
          <OutputClaim ClaimTypeReferenceId="givenName" />
          <OutputClaim ClaimTypeReferenceId="surname" />
          <OutputClaim ClaimTypeReferenceId="email" />
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
      </TechnicalProfile>
    </RelyingParty>
    ```

4. Guarde los cambios y después cargue el archivo.
5. Para probar la directiva personalizada que ha cargado, vaya a la página de la directiva en Azure Portal y seleccione **Ejecutar ahora**.

Puede encontrar la directiva de ejemplo [aquí](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).
