---
title: Configuración del inicio de sesión para un proveedor de identidades de Azure AD multiinquilino mediante directivas personalizadas en Azure Active Directory B2C | Microsoft Docs
description: 'Adición de un proveedor de identidades de Azure AD multiinquilino mediante directivas personalizadas: Azure Active Directory B2C.'
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 72d01d6927ee421d01a831244acf65c44a084354
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66508660"
---
# <a name="set-up-sign-in-for-multi-tenant-azure-active-directory-using-custom-policies-in-azure-active-directory-b2c"></a>Configuración del inicio de sesión para Azure Active Directory multiinquilino mediante directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se muestra cómo habilitar el inicio de sesión de los usuarios con el punto de conexión de multiempresa de Azure Active Directory (Azure AD) mediante [directivas personalizadas](active-directory-b2c-overview-custom.md) en Azure AD B2C. Esto permite a los usuarios de varios inquilinos de Azure AD iniciar sesión en Azure AD B2C sin tener que configurar un proveedor técnico para cada inquilino. Sin embargo, los miembros invitados en cualquiera de estos inquilinos **no** podrán iniciar sesión. Para ello, tendrá que [configurar individualmente cada inquilino](active-directory-b2c-setup-aad-custom.md).

>[!NOTE]
>En las siguientes instrucciones se usa `Contoso.com` como inquilino de Azure AD de la organización y `fabrikamb2c.onmicrosoft.com` como inquilino de Azure AD B2C.

## <a name="prerequisites"></a>Requisitos previos

Siga los pasos de [Introducción a las directivas personalizadas en Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).

## <a name="register-an-application"></a>Registro de una aplicación

Para habilitar el inicio de sesión para los usuarios de una organización específica de Azure AD, es preciso registrar una aplicación en el inquilino de Azure AD de la organización.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD de la organización (contoso.com). Para ello, haga clic en el **filtro de directorio y suscripción** que se encuentra en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Nuevo registro de aplicaciones**.
5. Escriba un nombre para la aplicación. Por ejemplo, `Azure AD B2C App`.
6. En **Tipo de aplicación**, seleccione `Web app / API`.
7. Para la **dirección URL de inicio de sesión**, escriba la siguiente dirección URL en minúscula, donde `your-tenant` se reemplaza por el nombre del inquilino de Azure AD B2C (fabrikamb2c.onmicrosoft.com):

    ```
    https://yourtenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp
    ```
    
8. Haga clic en **Create**(Crear). Copie el **Identificador de aplicación**, ya que lo usará más adelante.
9. Seleccione la aplicación y **Configuración**.
10. Seleccione **Claves**, escriba la descripción de la clave, seleccione una duración y haga clic en **Guardar**. Copie el valor de la clave que se muestra, ya que lo usará más adelante.
11. En **Configuración**, seleccione **Propiedades**, establezca **multiinquilino** en `Yes` y haga clic en **Guardar**

## <a name="create-a-policy-key"></a>Creación de una clave de directiva

Debe almacenar la clave de la aplicación que creó en el inquilino de Azure AD B2C.

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. En la página de introducción, seleccione **Marco de experiencia de identidad - VERSIÓN PRELIMINAR**.
4. Seleccione **Claves de directiva** y luego **Agregar**.
5. En **Opciones**, elija `Manual`.
6. Escriba un **nombre** para la clave de directiva. Por ejemplo, `ContosoAppSecret`.  Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
7. En **Secreto**, escriba la clave de la aplicación que guardó previamente.
8. En **Uso de claves**, seleccione `Signature`.
9. Haga clic en **Create**(Crear).

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si quiere que los usuarios inicien sesión con Azure AD, deberá definir Azure AD como proveedor de notificaciones con el que Azure AD B2C se comunique mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado. 

Para definir Azure AD como proveedor de notificaciones, agregue el elemento **ClaimsProvider** al archivo de extensión de la directiva.

1. Abra el archivo *TrustFrameworkExtensions.xml*.
2. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
3. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```XML
    <ClaimsProvider>
      <Domain>commonaad</Domain>
      <DisplayName>Common AAD</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Common-AAD">
          <DisplayName>Multi-Tenant AAD</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="METADATA">https://login.microsoftonline.com/common/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        
            <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. Update the GUIDs below for each tenant. -->
            <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/00000000-0000-0000-0000-000000000000,https://sts.windows.net/11111111-1111-1111-1111-111111111111</Item>

            <!-- The commented key below specifies that users from any tenant can sign-in. Uncomment if you would like anyone with an Azure AD account to be able to sign in. -->
            <!-- <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item> -->
          </Metadata>
          <CryptographicKeys>
            <!-- Make sure to update the reference ID of the client secret below you just created (B2C_1A_AADAppSecret) -->
            <Key Id="client_secret" StorageReferenceId="B2C_1A_AADAppSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
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

4. En el elemento **ClaimsProvider**, actualice el valor de **Domain** a un valor único que pueda usarse para distinguirlo de otros proveedores de identidades.
5. En el elemento **TechnicalProfile**, actualice el valor de **DisplayName**. Este valor se muestra en el botón de inicio de sesión de la pantalla de inicio de sesión.
6. Establezca **client_id** con el identificador de aplicación del registro de la aplicación multiinquilino de Azure AD.

### <a name="restrict-access"></a>Restricción del acceso

> [!NOTE]
> Al usar `https://sts.windows.net` como valor para **ValidTokenIssuerPrefixes**, se permite a todos los usuarios de Azure AD iniciar sesión en la aplicación.

Tiene que actualizar la lista de emisores de tokens válidos y restringir el acceso a la lista específica de usuarios inquilinos de Azure AD que pueden iniciar sesión. Para obtener los valores, debe examinar los metadatos de cada uno de los inquilinos específicos de Azure AD desde los que le gustaría que los usuarios iniciaran sesión. El formato de los datos es similar al siguiente: `https://login.windows.net/your-tenant/.well-known/openid-configuration`, donde `your-tenant` es el nombre del inquilino de Azure AD (contoso.com o cualquier otro inquilino de Azure AD).

1. Abra el explorador, vaya a la dirección URL de **METADATOS**, busque el objeto **issuer** y copie su valor. Debería ser similar a lo siguiente: `https://sts.windows.net/tenant-id/`.
2. Copie y pegue el valor de la clave **ValidTokenIssuerPrefixes**. Puede agregar varios separándolos con una coma. Un ejemplo de esto se describe en el ejemplo de XML anterior.

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Por el momento, ha configurado la directiva para que Azure AD B2C sepa cómo comunicarse con su directorio de Azure AD. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
2. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
3. Haga clic en **Cargar**.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

El proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro/inicio de sesión. Para que esté disponible, debe crear un duplicado de un recorrido del usuario de plantilla existente que modificaremos a continuación para que también tenga el proveedor de identidades de Azure AD.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
2. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
3. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
5. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInContoso`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón del proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** a Azure AD, se muestra un nuevo botón cuando un usuario llega a la página.

1. Busque el elemento **OrchestrationStep** que incluye `Order="1"` en el recorrido del usuario que ha creado.
2. En **ClaimsProviderSelects**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `AzureADExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con Azure AD para recibir un token. Para vincular un botón a una acción, vincule el perfil técnico del proveedor de notificaciones de Azure AD.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
2. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el elemento **Id** que el que usó en **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```
    
    Actualice el valor de **TechnicalProfileReferenceId** al elemento **Id** del perfil técnico que creó anteriormente. Por ejemplo, `Common-AAD`.

3. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

## <a name="create-an-azure-ad-b2c-application"></a>Creación de una aplicación de Azure AD B2C

La comunicación con Azure AD B2C se produce mediante una aplicación que se crea en el inquilino. En esta sección se enumeran los pasos opcionales que puede llevar a cabo para crear una aplicación de prueba, si aún no lo ha hecho.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y **Agregar**.
5. Escriba un nombre para la aplicación; por ejemplo, *testapp1*.
6. En **Aplicación web/API web**, seleccione `Yes` y escriba `https://jwt.ms` como **URL de respuesta**.
7. Haga clic en **Create**(Crear).

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cámbiele el nombre a *SignUpSignContoso.xml*.
2. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInContoso`.
3. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo, `http://contoso.com/B2C_1A_signup_signin_contoso`
4. Actualice el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado (SignUpSignContoso).
5. Guarde los cambios, cargue el archivo y seleccione la nueva directiva en la lista.
6. Asegúrese de que la aplicación de Azure AD B2C que creó está seleccionada en el campo **Seleccionar aplicación** y pruébela; para ello, haga clic en **Ejecutar ahora**.
