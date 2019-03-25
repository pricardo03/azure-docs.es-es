---
title: Pase un token de acceso a través de una directiva personalizada a la aplicación en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre cómo se puede pasar un token de acceso para proveedores de identidades de OAuth2.0 como una notificación a través de una directiva personalizada a la aplicación en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 784387b119bff6445015419adfd3bc0e52eee43f
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402650"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Pase de un token de acceso a través de una directiva personalizada a la aplicación en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Un [directiva personalizada](active-directory-b2c-get-started-custom.md) en Azure Active Directory (Azure AD) B2C proporciona una oportunidad a los usuarios para registrarse o iniciar sesión con un proveedor de identidades. Cuando esto sucede, Azure AD B2C recibe un [token de acceso](active-directory-b2c-reference-tokens.md) del proveedor de identidades. Azure AD B2C usa ese token para recuperar información sobre el usuario. Agregue un tipo de notificación y envíe dicha notificación a la directiva personalizada para pasar el token a través de las aplicaciones que registre en Azure AD B2C. 

Azure AD B2C admite pasar el token de acceso de los proveedores de identidades de [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) y [OpenID Connect](active-directory-b2c-reference-oidc.md). Para todos los demás proveedores de identidades, la notificación se devuelve en blanco.

## <a name="prerequisites"></a>Requisitos previos

- La directiva personalizada se configura con un proveedor de identidades de OAuth 2.0. o de OpenID Connect.

## <a name="add-the-claim-elements"></a>Incorporación de elementos de notificación 

1. Abra el archivo *TrustframeworkExtensions.xml* y agregue el siguiente elemento **ClaimType** con un identificador de `identityProviderAccessToken` al elemento **ClaimsSchema**:

    ```XML
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="identityProviderAccessToken">
          <DisplayName>Identity Provider Access Token</DisplayName>
          <DataType>string</DataType>
          <AdminHelpText>Stores the access token of the identity provider.</AdminHelpText>
        </ClaimType>
        ...
      </ClaimsSchema>
    </BuildingBlocks>
    ```

2. Agregue el elemento **OutputClaim** al elemento **TechnicalProfile** para cada proveedor de identidades de OAuth 2.0 para el que le gustaría el token de acceso. El ejemplo siguiente muestra el elemento agregado en el perfil técnico de Facebook:

    ```XML
    <ClaimsProvider>
      <DisplayName>Facebook</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Facebook-OAUTH">
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="{oauth2:access_token}" />
          </OutputClaims>
          ...
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

3. Guarde el archivo *TrustFrameworkExtensions.xml*.
4. Abra el archivo de directiva del usuario de confianza, como *SignUpOrSignIn.xml* y agregue el elemento **OutputClaim** a **TechnicalProfile**:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
      <TechnicalProfile Id="PolicyProfile">
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="idp_access_token"/>
        </OutputClaims>
        ...
      </TechnicalProfile>
    </RelyingParty>
    ```

5. Guarde el archivo de directiva.

## <a name="test-your-policy"></a>Prueba de la directiva

Al probar las aplicaciones en Azure AD B2C, puede ser útil que el token de Azure AD B2C se devuelva a `https://jwt.ms` para poder revisar las notificaciones en él.

### <a name="upload-the-files"></a>Carga de los archivos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Marco de experiencia de identidad**.
5. En la página Directivas personalizadas, haga clic en **Cargar directiva**.
6. Seleccione **Sobrescribir la directiva, si existe** y busque y seleccione el archivo *TrustframeworkExtensions.xml*.
7. Haga clic en **Cargar**.
8. Repita los pasos 5 a 7 para el archivo del usuario de confianza, como *SignUpOrSignIn.xml*.

### <a name="run-the-policy"></a>Ejecución de la directiva

1. Abra la directiva que ha cambiado. Por ejemplo, *B2C_1A_signup_signin*.
2. Para **Aplicación**, seleccione la aplicación que registró anteriormente. Para ver el token en el ejemplo siguiente, **URL de respuesta** debe mostrar `https://jwt.ms`.
3. Haga clic en **Ejecutar ahora**.

    Debería ver algo parecido al siguiente ejemplo:

    ![Token descodificado](./media/idp-pass-through-custom/idp-pass-through-custom-token.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los tokens en la [referencia del token de Azure Active Directory](active-directory-b2c-reference-tokens.md).





