---
title: Ejemplos de transformación de notificaciones booleanas para el esquema del marco de experiencia de identidad de Azure Active Directory B2C | Microsoft Docs
description: Ejemplos de transformación de notificaciones booleanas para el esquema del marco de experiencia de identidad de Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0a08849340d19055a03f85ca401757a81cd2c95d
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511736"
---
# <a name="boolean-claims-transformations"></a>Transformaciones de notificaciones booleanas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos para usar las transformaciones de notificaciones booleanas del esquema del marco de experiencia de identidad en Azure Active Directory (Azure AD) B2C. Para más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="andclaims"></a>AndClaims

Realiza una operación And de dos inputClaims booleanos y establece el elemento outputClaim con el resultado de la operación.

| item  | TransformationClaimType  | Tipo de datos  | Notas |
|-------| ------------------------ | ---------- | ----- |
| InputClaim | inputClaim1 | boolean | El primer ClaimType que se va a evaluar. |
| InputClaim | inputClaim2  | boolean | El segundo ClaimType que se va a evaluar. |
|OutputClaim | outputClaim | boolean | Los ClaimTypes que se generan después de que se haya invocado esta transformación de notificaciones (true o false). |

La siguiente transformación de notificaciones explica cómo aplicar And a dos argumentos ClaimType booleanos: `isEmailNotExist` y `isSocialAccount`. La notificación de salida `presentEmailSelfAsserted` está establecida en `true` si el valor de ambas notificaciones de entrada es `true`. En un paso de orquestación, puede usar una condición previa para preestablecer una página autoafirmada solo si la dirección de correo electrónico de la cuenta de redes sociales está vacía.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="AndClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isEmailNotExist" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isSocialAccount" TransformationClaimType="inputClaim2" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentEmailSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Notificaciones de salida:
    - **outputClaim**: false


## <a name="assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue

Comprueba que los valores booleanos de dos notificaciones son iguales y lanza una excepción si no lo son.

| item | TransformationClaimType  | Tipo de datos  | Notas |
| ---- | ------------------------ | ---------- | ----- |
| inputClaim | inputClaim | boolean | ClaimType que se va a afirmar. |
| InputParameter |valueToCompareTo | boolean | El valor que se va a comparar (true o false). |

La transformación de notificaciones **AssertBooleanClaimIsEqualToValue** siempre se ejecuta desde un [perfil técnico de validación](validation-technical-profile.md) llamado por un [perfil técnico autoafirmado](self-asserted-technical-profile.md). Los metadatos de un perfil técnico autoafirmado **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** controlan el mensaje de error que el perfil técnico presenta al usuario.

![Ejecución de AssertStringClaimsAreEqual](./media/boolean-transformations/assert-execution.png)

La siguiente transformación de notificaciones explica cómo comprobar el valor de un argumento ClaimType booleano con un valor `true`. Si el valor del ClaimType `accountEnabled` es false, se desencadena un mensaje de error.

```XML
<ClaimsTransformation Id="AssertAccountEnabledIsTrue" TransformationMethod="AssertBooleanClaimIsEqualToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="accountEnabled" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
</ClaimsTransformation>
```


El perfil técnico de validación `login-NonInteractive` llama a la transformación de notificaciones `AssertAccountEnabledIsTrue`.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

El perfil técnico autoafirmado llama al perfil técnico **login-NonInteractive** de validación.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Custom error message if account is disabled.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim**: false
    - **valueToCompareTo**: true
- Resultado: aparece un error

## <a name="notclaims"></a>NotClaims

Realiza una operación Not del inputClaim booleano y establece el elemento outputClaim con el resultado de la operación.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | boolean | La notificación que se va a realizar. |
| OutputClaim | outputClaim | boolean | Elementos ClaimTypes que se producen después de que se invoque este elemento ClaimsTransformation (true o false). |

Use esta transformación de notificaciones para realizar la negación lógica en una notificación.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="NotClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="inputClaim" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim**: false
- Notificaciones de salida:
    - **outputClaim**: true

## <a name="orclaims"></a>OrClaims 

Procesa una operación Or de dos inputClaims booleanos y establece el elemento outputClaim con el resultado de la operación.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | boolean | El primer ClaimType que se va a evaluar. |
| InputClaim | inputClaim2 | boolean | El segundo ClaimType que se va a evaluar. |
| OutputClaim | outputClaim | boolean | Los ClaimTypes que se generan después de que se haya invocado esta ClaimsTransformation (true o false). |

La siguiente transformación de notificaciones explica cómo aplicar `Or` a dos argumentos ClaimType booleanos. En el paso de orquestación, puede usar una condición previa para preestablecer una página autoafirmada si el valor de una de las notificaciones es `true`.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="OrClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedNotExists" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="inputClaim2" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentTOSSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Notificaciones de salida:
    - **outputClaim**: true

