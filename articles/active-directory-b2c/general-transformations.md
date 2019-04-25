---
title: Ejemplos de transformación de notificaciones generales para el esquema del marco de experiencia de identidad de Azure Active Directory B2C | Microsoft Docs
description: Ejemplos de transformación de notificaciones generales para el esquema del marco de experiencia de identidad de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 6a9a819e75e487999a2b50ae758b8d9c6c716a4f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60397165"
---
# <a name="general-claims-transformations"></a>Transformaciones de notificaciones generales

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos para usar las transformaciones de notificaciones generales del esquema del marco de experiencia de identidad en Azure Active Directory (Azure AD) B2C. Para obtener más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="doesclaimexist"></a>DoesClaimExist

Comprueba si el elemento **inputClaim** existe o no y establece **outputClaim** en true o false según corresponda.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |Cualquiera | La notificación de entrada cuya existencia debe comprobarse. |
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

Hash del texto sin formato proporcionado con el valor salt y un secreto.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | plaintext | string | La notificación de entrada que se va a cifrar. |
| InputClaim | salt | string | El parámetro salt. Puede crear un valor aleatorio, utilizando la transformación de notificaciones `CreateRandomString`. |
| InputParameter | randomizerSecret | string | Apunta a un **claves de directiva** de Azure AD B2C existentes. Para crear una nueva: En su inquilino de Azure AD B2C, seleccione **B2C Settings (Configuración de B2C) > Marco de experiencia de identidad**. Seleccione **Policy Keys** (Claves de directiva) para ver las claves disponibles en el inquilino. Seleccione **Agregar**. En **Opciones**, seleccione **Manual**. Proporcione un nombre (es posible que se agregue automáticamente el prefijo B2C_1A_). En el cuadro de secreto, escriba el secreto que quiere usar, por ejemplo, 1234567890. Para usar una clave, seleccione **Secreto**. Seleccione **Crear**. |
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



