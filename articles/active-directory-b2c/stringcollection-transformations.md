---
title: Ejemplos de transformación de notificaciones StringCollection para el esquema del marco de experiencia de identidad de Azure Active Directory B2C | Microsoft Docs
description: Ejemplos de transformación de notificaciones StringCollection para el esquema del marco de experiencia de identidad de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 924da8c92bad1c5345d5d1833723a0c4e8ad7d29
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47432671"
---
# <a name="stringcollection-claims-transformations"></a>Transformaciones de notificaciones StringCollection

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos para usar las transformaciones de notificaciones de la colección de cadenas del esquema del marco de experiencia de identidad en Azure Active Directory (Azure AD) B2C. Para más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Agrega una notificación de cadena a una nueva notificación stringCollection. 

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | item | string | Elemento ClaimType que se agregará a la notificación de salida. |
| InputClaim | collection | stringCollection | [Opcional] Si se especifica, la transformación de notificaciones copia los elementos de esta colección y agrega el elemento al final de la notificación de la colección de salida. |
| OutputClaim | collection | stringCollection | Elementos ClaimTypes que se producen después de que se invoque este elemento ClaimsTransformation. |

Use esta transformación de notificaciones para agregar una cadena a una clase stringCollection nueva o existente. Se usa normalmente en un perfil técnico **AAD-UserWriteUsingAlternativeSecurityId**. Antes de crear una nueva cuenta de redes sociales, la transformación de notificaciones **CreateOtherMailsFromEmail** lee el ClaimType y agrega el valor al ClaimType **otherMails**. 

La siguiente transformación de notificaciones agrega el ClaimType **email** al ClaimType **otherMails**.

```XML
<ClaimsTransformation Id="CreateOtherMailsFromEmail" TransformationMethod="AddItemToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **collection**: ["someone@outlook.com"]
    - **item**: "admin@contoso.com"
- Notificaciones de salida: 
    - **collection**: ["someone@outlook.com", "admin@contoso.com"]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

Agrega un parámetro de cadena a una nueva notificación stringCollection. 

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | collection | stringCollection | [Opcional] Si se especifica, la transformación de notificaciones copia los elementos de esta colección y agrega el elemento al final de la notificación de la colección de salida. |
| InputParameter | item | string | El valor que se agregará a la notificación de salida. |
| OutputClaim | collection | stringCollection | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation. |

Use esta transformación de notificaciones para agregar un valor de cadena a una clase stringCollection nueva o existente. En el ejemplo siguiente se agrega una dirección de correo electrónico constante (admin@contoso.com) a la notificación **otherMails**. 

```XML
<ClaimsTransformation Id="SetCompanyEmail" TransformationMethod="AddParameterToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="item" DataType="string" Value="admin@contoso.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **collection**: ["someone@outlook.com"]
- Parámetros de entrada 
    - **item**: "admin@contoso.com"
- Notificaciones de salida:
    - **collection**: ["someone@outlook.com", "admin@contoso.com"]

## <a name="getsingleitemfromstringcollection"></a>GetSingleItemFromStringCollection

Obtiene el primer elemento de la colección de la cadena proporcionada. 

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | collection | stringCollection | Los ClaimTypes que la transformación de notificaciones usa para obtener el elemento. |
| OutputClaim | extractedItem | string | Elementos ClaimTypes que se producen después de que se invoque este elemento ClaimsTransformation. El primer elemento de la colección. |

En el ejemplo siguiente se lee la notificación **otherMails** y se devuelve el primer elemento de la notificación de **correo electrónico**. 

```XML
<ClaimsTransformation Id="CreateEmailFromOtherMails" TransformationMethod="GetSingleItemFromStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedItem" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **collection**: ["someone@outlook.com", "someone@contoso.com"]
- Notificaciones de salida: 
    - **extractedItem**: "someone@outlook.com"

