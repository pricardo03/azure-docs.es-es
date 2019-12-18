---
title: Ejemplos de transformación de notificaciones de JSON para directivas personalizadas
titleSuffix: Azure AD B2C
description: Ejemplos de transformación de notificaciones de JSON para el esquema de Identity Experience Framework (IEF) de Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0ff6f24e30febd57a3a9740ec72a927225b37933
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74948915"
---
# <a name="json-claims-transformations"></a>Transformaciones de notificaciones de JSON

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos de uso de las transformaciones de notificaciones de JSON del esquema Identity Experience Framework en Azure Active Directory B2C (Azure AD B2C). Para más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="getclaimfromjson"></a>GetClaimFromJson

Obtener un elemento especificado de datos JSON.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputJson | string | ClaimTypes que usa la transformación de notificaciones para obtener el elemento. |
| InputParameter | claimToExtract | string | El nombre del elemento JSON que se va a extraer. |
| OutputClaim | extractedClaim | string | ClaimType que se genera después de que se haya invocado esta transformación de notificaciones, con el valor especificado en el parámetro de entrada _claimToExtract_. |

En el ejemplo siguiente, se muestra que la transformación de notificaciones extrajo el elemento `emailAddress` a partir de los datos JSON: `{"emailAddress": "someone@example.com", "displayName": "Someone"}`

```XML
<ClaimsTransformation Id="GetEmailClaimFromJson" TransformationMethod="GetClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="customUserData" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="emailAddress" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="extractedEmail" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Someone"}
- Parámetro de entrada:
    - **claimToExtract**: emailAddress
- Notificaciones de salida:
  - **extractedClaim**: someone@example.com


## <a name="getclaimsfromjsonarray"></a>GetClaimsFromJsonArray

Obtener una lista de los elementos especificados a partir de datos JSON.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | jsonSourceClaim | string | ClaimTypes que usa la transformación de notificaciones para obtener las notificaciones. |
| InputParameter | errorOnMissingClaims | boolean | Especifica si se emite un error cuando falta una de las notificaciones. |
| InputParameter | includeEmptyClaims | string | Especifica si se incluyen notificaciones vacías. |
| InputParameter | jsonSourceKeyName | string | Nombre de la clave del elemento. |
| InputParameter | jsonSourceValueName | string | Nombre del valor del elemento. |
| OutputClaim | Colección | string, int, boolean y datetime |Lista de las notificaciones que se van a extraer. El nombre de la notificación debe ser igual al especificado en la notificación de entrada _jsonSourceClaim_. |

En el ejemplo siguiente, la transformación de notificaciones extrae estas notificaciones: email (string), displayName (string), membershipNum (int), active (boolean) y birthdate (datetime) a partir de los datos JSON.

```JSON
[{"key":"email","value":"someone@example.com"}, {"key":"displayName","value":"Someone"}, {"key":"membershipNum","value":6353399}, {"key":"active","value":true}, {"key":"birthdate","value":"1980-09-23T00:00:00Z"}]
```

```XML
<ClaimsTransformation Id="GetClaimsFromJson" TransformationMethod="GetClaimsFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="jsonSourceClaim" TransformationClaimType="jsonSource" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="errorOnMissingClaims" DataType="boolean" Value="false" />
    <InputParameter Id="includeEmptyClaims" DataType="boolean" Value="false" />
    <InputParameter Id="jsonSourceKeyName" DataType="string" Value="key" />
    <InputParameter Id="jsonSourceValueName" DataType="string" Value="value" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="membershipNum" />
    <OutputClaim ClaimTypeReferenceId="active" />
    <OutputClaim ClaimTypeReferenceId="birthdate" />
  </OutputClaims>
</ClaimsTransformation>
```

- Notificaciones de entrada:
  - **jsonSourceClaim**: [{"key":"email","value":"someone@example.com"}, {"key":"displayName","value":"Someone"}, {"key":"membershipNum","value":6353399}, {"key":"active","value": true}, {"key":"birthdate","value":"1980-09-23T00:00:00Z"}]
- Parámetros de entrada:
    - **errorOnMissingClaims**: false
    - **includeEmptyClaims**: false
    - **jsonSourceKeyName**: key
    - **jsonSourceValueName**: value
- Notificaciones de salida:
  - **email**: "someone@example.com"
  - **displayName**: "Someone"
  - **membershipNum**: 6353399
  - **active**: true
  - **birthdate**: 1980-09-23T00:00:00Z

## <a name="getnumericclaimfromjson"></a>GetNumericClaimFromJson

Obtiene un elemento numérico especificado (long) a partir de datos JSON.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputJson | string | ClaimTypes usadas por la transformación de notificaciones para obtener la notificación. |
| InputParameter | claimToExtract | string | El nombre del elemento JSON que se va a extraer. |
| OutputClaim | extractedClaim | long | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation, con el valor del elemento especificado en los parámetros de entrada _claimToExtract_. |

En el ejemplo siguiente, la transformación de notificaciones extrae el elemento `id` a partir de datos JSON.

```JSON
{
    "emailAddress": "someone@example.com",
    "displayName": "Someone",
    "id" : 6353399
}
```

```XML
<ClaimsTransformation Id="GetIdFromResponse" TransformationMethod="GetNumericClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="exampleInputClaim" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="id" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="membershipId" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Someone", "id" : 6353399}
- Parámetros de entrada
    - **claimToExtract**: id
- Notificaciones de salida:
    - **extractedClaim**: 6353399

## <a name="getsinglevaluefromjsonarray"></a>GetSingleValueFromJsonArray

Obtiene el primer elemento de una matriz de datos JSON.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputJsonClaim | string | ClaimTypes usados por la transformación de notificaciones para obtener el elemento de la matriz JSON. |
| OutputClaim | extractedClaim | string | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation, con el primer elemento de la matriz JSON. |

En el ejemplo siguiente, la transformación de notificaciones extrae el primer elemento (dirección de correo electrónico) de la matriz JSON `["someone@example.com", "Someone", 6353399]`.

```XML
<ClaimsTransformation Id="GetEmailFromJson" TransformationMethod="GetSingleValueFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userData" TransformationClaimType="inputJsonClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **inputJsonClaim**: ["someone@example.com", "Someone", 6353399]
- Notificaciones de salida:
  - **extractedClaim**: someone@example.com

## <a name="xmlstringtojsonstring"></a>XmlStringToJsonString

Convierte los datos XML en formato JSON.

| item | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | Xml | string | ClaimTypes usados por la transformación de notificaciones para convertir los datos XML en formato JSON. |
| OutputClaim | json | string | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation, con los datos en formato JSON. |

```XML
<ClaimsTransformation Id="ConvertXmlToJson" TransformationMethod="XmlStringToJsonString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="intpuXML" TransformationClaimType="xml" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="outputJson" TransformationClaimType="json" />
  </OutputClaims>
</ClaimsTransformation>
```

En el ejemplo siguiente, la transformación de notificaciones convierte los siguientes datos XML en formato JSON.

#### <a name="example"></a>Ejemplo
Notificación de entrada:

```XML
<user>
  <name>Someone</name>
  <email>someone@example.com</email>
</user>
```

Notificación de salida:

```JSON
{
  "user": {
    "name":"Someone",
    "email":"someone@example.com"
  }
}
```

