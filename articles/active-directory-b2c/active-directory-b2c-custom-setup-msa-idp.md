---
title: Adición de una cuenta Microsoft (MSA) como proveedor de identidades mediante directivas personalizadas en Azure Active Directory B2C | Microsoft Docs
description: Ejemplo en el que se usa Microsoft como proveedor de identidades mediante el protocolo OpenID Connect (OIDC).
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d4b1f65daf271de88ce7167da6866e1890602fd4
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66510334"
---
# <a name="set-up-sign-in-with-a-microsoft-account-using-custom-policies-in-azure-active-directory-b2c"></a>Configuración del inicio de sesión con una cuenta Microsoft mediante directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se muestra cómo habilitar el inicio de sesión para los usuarios desde una cuenta Microsoft mediante [directivas personalizadas](active-directory-b2c-overview-custom.md) en Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Requisitos previos

- Siga los pasos de [Introducción a las directivas personalizadas en Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).
- Si aún no tiene una cuenta Microsoft, cree una en [https://www.live.com/](https://www.live.com/).

## <a name="add-an-application"></a>Adición de una aplicación

Para usar una cuenta Microsoft como proveedor de identidades en Azure AD B2C, deberá agregar una aplicación de la cuenta Microsoft.

1. Inicie sesión en el [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) con las credenciales de su cuenta Microsoft.
2. En la esquina superior derecha, seleccione **Agregar una aplicación**.
3. Escriba un **nombre de aplicación** y haga clic en **Crear** 
4. Seleccione **Generar nueva contraseña** y asegúrese de copiar la contraseña que se va a usar al configurar el proveedor de identidades. Copie también el identificador de aplicación. 
5. Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **Direcciones URL de redireccionamiento**. Reemplace `your-tenant-name` por el nombre del inquilino.
6. Seleccione **Guardar**.

## <a name="create-a-policy-key"></a>Creación de una clave de directiva

Debe almacenar la contraseña que haya generado y registrado previamente en el inquilino de Azure AD B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. Asegúrese de que usa el directorio que contiene al inquilino de Azure AD B2C. Seleccione el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. En la página de introducción, seleccione **Marco de experiencia de identidad - VERSIÓN PRELIMINAR**.
5. Seleccione **Claves de directiva** y luego **Agregar**.
6. En **Opciones**, elija `Manual`.
7. Escriba un **nombre** para la clave de directiva. Por ejemplo, `MSASecret`. Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
8. En **Secreto**, escriba la contraseña que haya registrado previamente.
9. En **Uso de claves**, seleccione `Signature`.
10. Haga clic en **Create**(Crear).

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si desea que los usuarios inicien sesión con una cuenta Microsoft, deberá definir la cuenta como un proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado. 

Puede definir Azure AD como proveedor de notificaciones. Para ello, agregue el elemento **ClaimsProvider** en el archivo de extensión de la directiva.

1. Abra el archivo *TrustFrameworkExtensions.xml*.
2. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
3. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```xml
    <ClaimsProvider>
      <Domain>live.com</Domain>
      <DisplayName>Microsoft Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="MSA-OIDC">
          <DisplayName>Microsoft Account</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="ProviderName">https://login.live.com</Item>
            <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your Microsoft application client id</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="email" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4.  Reemplace el valor de **client_id** con el identificador de aplicación que haya registrado previamente.
5.  Guarde el archivo.

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Por el momento, ha configurado la directiva para que Azure AD B2C sepa cómo comunicarse con su cuenta Microsoft. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
2. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
3. Haga clic en **Cargar**.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

En este punto, el proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro o de inicio de sesión. Para que esté disponible, creará un duplicado de un recorrido del usuario de plantilla existente y, después, lo modificará para que también tenga el proveedor de identidades de la cuenta Microsoft.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
2. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
3. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
5. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInMSA`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón de proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta Microsoft, se muestra un nuevo botón cuando un usuario llega a la página.

1. En el archivo *TrustFrameworkExtensions.xml*, busque el elemento **OrchestrationStep** que incluya `Order="1"` en el recorrido de usuario que creó.
2. En **ClaimsProviderSelects**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `MicrosoftAccountExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con la cuenta de Twitter para recibir un token.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
2. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el identificador que usó para **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    ```
    
    Cambie el valor de **TechnicalProfileReferenceId** para el identificador del perfil técnico que creó anteriormente. Por ejemplo, `MSA-OIDC`.

3. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

## <a name="create-an-azure-ad-b2c-application"></a>Creación de una aplicación de Azure AD B2C

La comunicación con Azure AD B2C se produce mediante una aplicación que se crea en el inquilino. En esta sección se enumeran los pasos opcionales que puede llevar a cabo para crear una aplicación de prueba, si aún no lo ha hecho.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene al inquilino de Azure AD B2C. Seleccione el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y **Agregar**.
5. Escriba un nombre para la aplicación; por ejemplo, *testapp1*.
6. En **Aplicación web/API web**, seleccione `Yes` y escriba `https://jwt.ms` como **URL de respuesta**.
7. Haga clic en **Create**(Crear).

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie su nombre a *SignUpSignInMSA.xml*.
2. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInMSA`.
3. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo, `http://contoso.com/B2C_1A_signup_signin_msa`
4. Cambie el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado (SignUpSignInMSA).
5. Guarde los cambios, cargue el archivo y, a continuación, seleccione la nueva directiva en la lista.
6. Asegúrese de que la aplicación de Azure AD B2C que creó está seleccionada en el campo **Seleccionar aplicación** y pruébela; para ello, haga clic en **Ejecutar ahora**.
