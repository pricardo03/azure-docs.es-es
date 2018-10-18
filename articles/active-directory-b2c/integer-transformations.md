---
title: Ejemplos de transformación de notificaciones de entero para el esquema del marco de experiencia de identidad de Azure Active Directory B2C | Microsoft Docs
description: Ejemplos de transformación de notificaciones de entero para el esquema del marco de experiencia de identidad de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 988e25b6a5ef3f99ae7df9076a40e06b403bb029
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44383035"
---
# <a name="integer-claims-transformations"></a>Transformaciones de notificaciones de entero

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos para usar las transformaciones de notificaciones de entero del esquema del marco de experiencia de identidad en Azure Active Directory (Azure AD) B2C. Para obtener más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim 

Convierte a un tipo de datos Long en un tipo de datos String.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | long | El valor ClaimType que se va a convertir en cadena. |
| OutputClaim | outputClaim | string | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation. |

En este ejemplo, la notificación `numericUserId` con un tipo de valor de Long se convierte en una notificación `UserId` con un tipo de valor de String.

```XML
<ClaimsTransformation Id="CreateUserId" TransformationMethod="ConvertNumberToStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="UserId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim**: 12334 (long)
- Notificaciones de salida: 
    - **outputClaim**: "12334" (string)

