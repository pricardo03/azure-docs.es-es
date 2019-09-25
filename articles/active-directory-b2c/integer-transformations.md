---
title: Ejemplos de transformación de notificaciones de entero para el esquema del marco de experiencia de identidad de Azure Active Directory B2C | Microsoft Docs
description: Ejemplos de transformación de notificaciones de entero para el esquema del marco de experiencia de identidad de Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d82f7fecfb35c63d586993fed73a83209782a890
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064210"
---
# <a name="integer-claims-transformations"></a>Transformaciones de notificaciones de entero

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos de uso de las transformaciones de notificaciones de enteros del esquema Identity Experience Framework en Azure Active Directory B2C (Azure AD B2C). Para más información, vea [ClaimsTransformations](claimstransformations.md).

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
    - **inputClaim**: 12334 (Long)
- Notificaciones de salida:
    - **outputClaim**: "12334" (cadena)

