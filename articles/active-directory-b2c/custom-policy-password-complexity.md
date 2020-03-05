---
title: Configuración de la complejidad de contraseñas mediante directivas personalizadas
titleSuffix: Azure AD B2C
description: Cómo configurar los requisitos de complejidad de las contraseñas con una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 2de1130e28b5071913e4cf3632c3fe4407597a98
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/29/2020
ms.locfileid: "78189147"
---
# <a name="configure-password-complexity-using-custom-policies-in-azure-active-directory-b2c"></a>Configuración de la complejidad de las contraseñas con directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En Azure Active Directory B2C (Azure AD B2C), puede configurar los requisitos de complejidad de las contraseñas que proporciona un usuario al crear una cuenta. De forma predeterminada, Azure AD B2C utiliza contraseñas **seguras**. En este artículo se muestra cómo configurar la complejidad de las contraseñas en [directivas personalizadas](custom-policy-overview.md). También es posible configurar la complejidad de las contraseñas en [flujos de usuario](user-flow-password-complexity.md).

## <a name="prerequisites"></a>Prerrequisitos

Siga los pasos de [Introducción a las directivas personalizadas en Active Directory B2C](custom-policy-get-started.md).

## <a name="add-the-elements"></a>Adición de los elementos

1. Copie el archivo *SignUpOrSignIn.xml* que descargó con el paquete de inicio y asígnele el nombre de archivo *SingUpOrSignInPasswordComplexity.xml*.
2. Abra el archivo *SingUpOrSignInPasswordComplexity.xml* y cambie el valor de **PolicyId** y **PublicPolicyUri** a un nuevo nombre de directiva. Por ejemplo, *B2C_1A_signup_signin_password_complexity*.
3. Agregue los siguientes elementos **ClaimType** con los identificadores de `newPassword` y `reenterPassword`:

    ```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    ```

4. Los elementos [Predicates](predicates.md) tienen tipos de método de `IsLengthRange` o `MatchesRegex`. El tipo `MatchesRegex` se usa para hacer coincidir con una expresión regular. El tipo `IsLengthRange` toma una longitud de cadena mínima y máxima. Agregue un elemento **Predicates** al elemento **BuildingBlocks**, si no existe, con los siguientes elementos **Predicate**:

    ```XML
    <Predicates>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    ```

5. Cada elemento **InputValidation** se construye con los elementos **Predicate** definidos. Este elemento le permite realizar agregaciones booleanas similares a `and` y `or`. Agregue un elemento **InputValidations** al elemento **BuildingBlocks**, si no existe, con el siguiente elemento **InputValidation**:

    ```XML
    <InputValidations>
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
    </InputValidations>
    ```

6. Asegúrese de que el perfil técnico **PolicyProfile** contiene los siguientes elementos:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn"/>
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect"/>
        <InputClaims>
          <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
        </InputClaims>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="displayName"/>
          <OutputClaim ClaimTypeReferenceId="givenName"/>
          <OutputClaim ClaimTypeReferenceId="surname"/>
          <OutputClaim ClaimTypeReferenceId="email"/>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub"/>
      </TechnicalProfile>
    </RelyingParty>
    ```

7. Guarde el archivo de directiva.

## <a name="test-your-policy"></a>Prueba de la directiva

Al probar las aplicaciones en Azure AD B2C, puede ser útil que el token de Azure AD B2C se devuelva a `https://jwt.ms` para poder revisar las notificaciones en él.

### <a name="upload-the-files"></a>Carga de los archivos

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Marco de experiencia de identidad**.
5. En la página Directivas personalizadas, haga clic en **Cargar directiva**.
6. Seleccione **Sobrescribir la directiva, si existe** y busque y seleccione el archivo *SingUpOrSignInPasswordComplexity.xml*.
7. Haga clic en **Cargar**.

### <a name="run-the-policy"></a>Ejecución de la directiva

1. Abra la directiva que ha cambiado. Por ejemplo, *B2C_1A_signup_signin_password_complexity*.
2. En **Aplicación**, seleccione la aplicación que registró anteriormente. Para ver el token, **URL de respuesta** debe mostrar `https://jwt.ms`.
3. Haga clic en **Ejecutar ahora**.
4. Seleccione **Registrarse ahora**, escriba una dirección de correo electrónico y escriba una contraseña nueva. Se presenta una guía sobre las restricciones de las contraseñas. Termine de especificar la información de usuario y, a continuación, haga clic en **Crear**. Debería ver el contenido del token que se devolvió.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Configuración del cambio de contraseñas mediante directivas personalizadas en Azure Active Directory B2C](custom-policy-password-change.md).


