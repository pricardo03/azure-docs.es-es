---
title: Ejemplos de transformación de notificaciones generales para directivas personalizadas
titleSuffix: Azure AD B2C
description: Ejemplos de transformación de notificaciones generales para el esquema de Identity Experience Framework (IEF) de Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 08/27/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 639277177bf63e659e5b0ea804eca5e20f956831
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74948907"
---
# <a name="general-claims-transformations"></a>Transformaciones de notificaciones generales

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos para usar las transformaciones de notificaciones generales del esquema de Identity Experience Framework en Azure Active Directory B2C (Azure AD B2C). Para más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="doesclaimexist"></a>DoesClaimExist

Comprueba si el elemento **inputClaim** existe o no y establece **outputClaim** en true o false según corresponda.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |Any | La notificación de entrada cuya existencia debe comprobarse. |
| OutputClaim | outputClaim | boolean | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation. |

Use esta transformación de notificaciones para comprobar si una notificación existe o contiene algún valor. El valor devuelto es un valor booleano que indica si existe la notificación. En el ejemplo siguiente se comprueba si existe la dirección de correo electrónico.

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **inputClaim**: someone@contoso.com
- Notificaciones de salida:
  - **outputClaim**: true

## <a name="hash"></a>Hash

Hash del texto sin formato proporcionado con el valor salt y un secreto. El algoritmo hash utilizado es SHA-256.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | plaintext | string | La notificación de entrada que se va a cifrar. |
| InputClaim | salt | string | El parámetro salt. Puede crear un valor aleatorio, utilizando la transformación de notificaciones `CreateRandomString`. |
| InputParameter | randomizerSecret | string | Apunta a una **clave de directiva** de Azure AD B2C existente. Para crear una nueva clave de directiva: En su inquilino de Azure AD B2C, en **Administrar**, seleccione **Identity Experience Framework**. Seleccione **Claves de directiva** para ver las claves disponibles en el inquilino. Seleccione **Agregar**. En **Opciones**, seleccione **Manual**. Proporcione un nombre (es posible que se agregue automáticamente el prefijo *B2C_1A_* ). En el cuadro de texto **Secreto**, escriba el secreto que quiere usar, por ejemplo, 1234567890. En **Uso de claves**, seleccione **Firma**. Seleccione **Crear**. |
| OutputClaim | hash | string | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones. La notificación configurada en el elemento inputClaim `plaintext`. |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **plaintext**: MyPass@word1
  - **salt**: 487624568
  - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Notificaciones de salida:
  - **outputClaim**: CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=
