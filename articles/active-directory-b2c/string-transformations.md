---
title: Ejemplos de transformación de notificaciones de cadena para directivas personalizadas
titleSuffix: Azure AD B2C
description: Ejemplos de transformación de notificaciones de cadena para el esquema de Identity Experience Framework (IEF) de Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 72b3349e0ad4fd86b91a7a02f70b2bcf1efbc271
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76712856"
---
# <a name="string-claims-transformations"></a>Transformaciones de notificaciones de cadena

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos para usar las transformaciones de notificaciones de cadena del esquema Identity Experience Framework en Azure Active Directory B2C (Azure AD B2C). Para más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="assertstringclaimsareequal"></a>AssertStringClaimsAreEqual

Comparar dos notificaciones y emitir una excepción si no son iguales según la comparación especificada inputClaim1, inputClaim2 y stringComparison.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | Tipo de la primera notificación, que se va a comparar. |
| InputClaim | inputClaim2 | string | Tipo de la segunda notificación, que se va a comparar. |
| InputParameter | stringComparison | string | comparación de cadenas, uno de los valores: Ordinal, OrdinalIgnoreCase. |

La transformación de notificaciones **AssertStringClaimsAreEqual** siempre se ejecuta desde un [perfil técnico de validación](validation-technical-profile.md) llamado por un [perfil técnico autofirmado](self-asserted-technical-profile.md). Los metadatos de un perfil técnico autoafirmado **UserMessageIfClaimsTransformationStringsAreNotEqual** controlan el mensaje de error que se presenta al usuario.

![Ejecución de AssertStringClaimsAreEqual](./media/string-transformations/assert-execution.png)

Puede usar esta transformación de notificaciones para asegurarse de que dos argumentos ClaimType tienen el mismo valor. Si no es así, se emite un mensaje de error. En el ejemplo siguiente se comprueba que los argumentos ClaimType **strongAuthenticationEmailAddress** e **email** son iguales. De lo contrario, se emite un mensaje de error.

```XML
<ClaimsTransformation Id="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="strongAuthenticationEmailAddress" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
  </InputParameters>
</ClaimsTransformation>
```


El perfil técnico de validación **login-NonInteractive** llama a la transformación de notificaciones **AssertEmailAndStrongAuthenticationEmailAddressAreEqual**.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

El perfil técnico autoafirmado llama al perfil técnico **login-NonInteractive** de validación.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">Custom error message the email addresses you provided are not the same.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
    - Parámetros de entrada:
  - **stringComparison**: ordinalIgnoreCase
- Resultado: aparece un error

## <a name="changecase"></a>ChangeCase

Cambia el uso de mayúsculas y minúsculas en la notificación proporcionada, dependiendo del operador.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | El tipo ClaimType que se va cambiar. |
| InputParameter | toCase | string | Puede ser uno de los siguientes valores: `LOWER` o `UPPER`. |
| OutputClaim | outputClaim | string | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones. |

Use esta transformación de notificaciones para cambiar cualquier ClaimType de cadena a minúsculas o mayúsculas.

```XML
<ClaimsTransformation Id="ChangeToLower" TransformationMethod="ChangeCase">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
  </InputClaims>
<InputParameters>
  <InputParameter Id="toCase" DataType="string" Value="LOWER" />
</InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **email**: SomeOne@contoso.com
- Parámetros de entrada:
    - **toCase**: LOWER
- Notificaciones de salida:
  - **email**: someone@contoso.com

## <a name="createstringclaim"></a>CreateStringClaim

Crea una notificación de cadena a partir del parámetro de entrada proporcionado en la directiva.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
|----- | ----------------------- | --------- | ----- |
| InputParameter | value | string | La cadena que va a establecerse. |
| OutputClaim | createdClaim | string | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones, con el valor especificado en el parámetro de entrada. |

Use que esta transformación de notificaciones para establecer un valor de ClaimType de cadena.

```XML
<ClaimsTransformation Id="CreateTermsOfService" TransformationMethod="CreateStringClaim">
  <InputParameters>
    <InputParameter Id="value" DataType="string" Value="Contoso terms of service..." />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="TOS" TransformationClaimType="createdClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Parámetro de entrada:
    - **value**: Contoso terms of service...
- Notificaciones de salida:
    - **createdClaim**: el tipo ClaimType TOS contiene el valor "Contoso terms of service…".

## <a name="compareclaims"></a>CompareClaims

Determine si una notificación de cadena es igual a otra. El resultado es un nuevo ClaimType booleano con un valor de `true` o `false`.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | Tipo de la primera notificación, que se va a comparar. |
| InputClaim | inputClaim2 | string | Tipo de la segunda notificación, que se va a comparar. |
| InputParameter | operator | string | Valores posibles: `EQUAL` o `NOT EQUAL`. |
| InputParameter | ignoreCase | boolean | Especifica si la comparación distingue entre mayúsculas y minúsculas en las cadenas que se están comparando. |
| OutputClaim | outputClaim | boolean | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones. |

Use esta transformación de notificaciones para comprobar si una notificación es igual a otra notificación. Por ejemplo, la siguiente transformación de notificaciones comprueba si el valor de la notificación **email** es igual al de la notificación **Verified.Email**.

```XML
<ClaimsTransformation Id="CheckEmail" TransformationMethod="CompareClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="Email" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="Verified.Email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="SameEmailAddress" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
- Parámetros de entrada:
    - **operator**:  NOT EQUAL
    - **ignoreCase**: true
- Notificaciones de salida:
    - **outputClaim**: true

## <a name="compareclaimtovalue"></a>CompareClaimToValue

Determina si un valor de notificación es igual al valor del parámetro de entrada.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | Tipo de la notificación, que se va a comparar. |
| InputParameter | operator | string | Valores posibles: `EQUAL` o `NOT EQUAL`. |
| InputParameter | compareTo | string | comparación de cadenas, uno de los valores: Ordinal, OrdinalIgnoreCase. |
| InputParameter | ignoreCase | boolean | Especifica si la comparación distingue entre mayúsculas y minúsculas en las cadenas que se están comparando. |
| OutputClaim | outputClaim | boolean | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones. |

Puede usar esta transformación de notificaciones para comprobar si una notificación es igual a un valor que haya especificado. Por ejemplo, la siguiente transformación de notificaciones comprueba si el valor de la notificación **termsOfUseConsentVersion** es igual a `v1`.

```XML
<ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="compareTo" DataType="string" Value="V1" />
    <InputParameter Id="operator" DataType="string" Value="not equal" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo
- Notificaciones de entrada:
    - **inputClaim1**: v1
- Parámetros de entrada:
    - **compareTo**: V1
    - **operator**: EQUAL
    - **ignoreCase**: true
- Notificaciones de salida:
    - **outputClaim**: true

## <a name="createrandomstring"></a>CreateRandomString

Crea una cadena aleatoria mediante el generador de números aleatorios. Si el generador de números aleatorios es de tipo `integer`, opcionalmente se pueden proporcionar un parámetro de inicialización y un número máximo. Un parámetro de formato de cadena opcional permite que la salida tenga ese formato, y un parámetro de base64 opcional especifica si la salida randomGeneratorType [guid, integer] outputClaim (String) está codificada en base64 .

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputParameter | randomGeneratorType | string | Especifica el valor aleatorio que se genera, `GUID` (identificador único global) o `INTEGER` (un número). |
| InputParameter | stringFormat | string | [Opcional] Formato del valor aleatorio. |
| InputParameter | base64 | boolean | [Opcional] Convertir el valor aleatorio en base64. Si se aplica el formato de cadena, el valor después del formato de cadena está codificado en base64. |
| InputParameter | maximumNumber | int | [Opcional] Solo para randomGeneratorType `INTEGER`. Especifique el número máximo. |
| InputParameter | seed  | int | [Opcional] Solo para randomGeneratorType `INTEGER`. Especifique el valor de inicialización del valor aleatorio. Nota: El mismo valor de inicialización genera la misma secuencia de números aleatorios. |
| OutputClaim | outputClaim | string | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones. El valor aleatorio. |

El ejemplo siguiente genera un identificador único global. Esta transformación de notificaciones se usa para crear el UPN (nombre principal del usuario) aleatorio.

```XML
<ClaimsTransformation Id="CreateRandomUPNUserName" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="GUID" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Ejemplo

- Parámetros de entrada:
    - **randomGeneratorType**: GUID
- Notificaciones de salida:
    - **outputClaim**: bc8bedd2-aaa3-411e-bdee-2f1810b73dfc

El ejemplo siguiente genera un valor entero aleatorio entre 0 y 1000. El valor pasa a tener el formato OTP_ {valor_aleatorio}.

```XML
<ClaimsTransformation Id="SetRandomNumber" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="INTEGER" />
    <InputParameter Id="maximumNumber" DataType="int" Value="1000" />
    <InputParameter Id="stringFormat" DataType="string" Value="OTP_{0}" />
    <InputParameter Id="base64" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="randomNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Parámetros de entrada:
    - **randomGeneratorType**: INTEGER
    - **maximumNumber**: 1000
    - **stringFormat**: OTP_{0}
    - **base64**: false
- Notificaciones de salida:
    - **outputClaim**: OTP_853


## <a name="formatstringclaim"></a>FormatStringClaim

Da formato a una notificación de acuerdo con la cadena de formato proporcionada. Esta transformación usa el método `String.Format` de C#.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |string |ClaimType que actúa como el parámetro {0} de formato de cadena. |
| InputParameter | stringFormat | string | El formato de cadena, incluido el parámetro {0}. |
| OutputClaim | outputClaim | string | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones. |

Use esta transformación de notificaciones para dar formato a cualquier cadena con un parámetro {0}. El ejemplo siguiente crea un **userPrincipalName**. Todos los perfiles técnicos de proveedores de las identidades de redes sociales, como `Facebook-OAUTH` llaman a **CreateUserPrincipalName** para generar un **userPrincipalName**.

```XML
<ClaimsTransformation Id="CreateUserPrincipalName" TransformationMethod="FormatStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="cpim_{0}@{RelyingPartyTenantId}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim**: 5164db16-3eee-4629-bfda-dcc3326790e9
- Parámetros de entrada:
    - **stringFormat**: cpim_{0}@{RelyingPartyTenantId}
- Notificaciones de salida:
  - **outputClaim**: cpim_5164db16-3eee-4629-bfda-dcc3326790e9@b2cdemo.onmicrosoft.com

## <a name="formatstringmultipleclaims"></a>FormatStringMultipleClaims

Da formato a dos notificaciones de acuerdo con la cadena de formato proporcionada. Esta transformación usa el método de C# **String.Format**.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |string | ClaimType que actúa como el parámetro {0} de formato de cadena. |
| InputClaim | inputClaim | string | ClaimType que actúa como el parámetro {1} de formato de cadena. |
| InputParameter | stringFormat | string | El formato de cadena, incluidos los parámetros {0} y {1}. |
| OutputClaim | outputClaim | string | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones. |

Use esta transformación de notificaciones para dar formato a cualquier cadena con dos parámetros, {0} y {1}. En el ejemplo siguiente se crea un **displayName** con el formato especificado:

```XML
<ClaimsTransformation Id="CreateDisplayNameFromFirstNameAndLastName" TransformationMethod="FormatStringMultipleClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="surName" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim1**: Joe
    - **inputClaim2**: Fernando
- Parámetros de entrada:
    - **stringFormat**: {0} {1}
- Notificaciones de salida:
    - **outputClaim**: Joe Fernando

## <a name="getmappedvaluefromlocalizedcollection"></a>GetMappedValueFromLocalizedCollection

Buscar un elemento en una colección **Restriction** de notificaciones.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | mapFromClaim | string | La notificación que contiene el texto que se va a buscar en las notificaciones **restrictionValueClaim** con la colección **Restriction**.  |
| OutputClaim | restrictionValueClaim | string | La notificación que contiene la colección **Restriction**. Una vez que se ha invocado la transformación de notificaciones, el valor de esta notificación contiene el valor del elemento seleccionado. |

En el ejemplo siguiente se busca la descripción del mensaje de error según la clave de error. La notificación **responseMsg** contiene una colección de mensajes de error para presentar al usuario final o a enviar al usuario de confianza.

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cannot sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```
La transformación de notificaciones busca el texto del elemento y devuelve su valor. Si la restricción se localiza mediante `<LocalizedCollection>`, la transformación de notificaciones devuelve el valor localizado.

```XML
<ClaimsTransformation Id="GetResponseMsgMappedToResponseCode" TransformationMethod="GetMappedValueFromLocalizedCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="responseCode" TransformationClaimType="mapFromClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="responseMsg" TransformationClaimType="restrictionValueClaim" />        
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **mapFromClaim**: B2C_V1_90001
- Notificaciones de salida:
    - **restrictionValueClaim**: No puede iniciar sesión porque es menor de edad.

## <a name="lookupvalue"></a>LookupValue

Buscar un valor de notificación en una lista de valores en función del valor de otra notificación.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputParameterId | string | La notificación que contiene el valor de búsqueda. |
| InputParameter | |string | Colección de inputParameters. |
| InputParameter | errorOnFailedLookup | boolean | Controlar si se devuelve un error cuando una búsqueda no tiene resultados. |
| OutputClaim | inputParameterId | string | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones. El valor del identificador coincidente. |

En el ejemplo siguiente se busca el nombre de dominio en una de las colecciones inputParameters. La transformación de notificaciones busca el nombre de dominio en el identificador y devuelve su valor (un identificador de aplicación).

```XML
 <ClaimsTransformation Id="DomainToClientId" TransformationMethod="LookupValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="inputParameterId" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="contoso.com" DataType="string" Value="13c15f79-8fb1-4e29-a6c9-be0d36ff19f1" />
    <InputParameter Id="microsoft.com" DataType="string" Value="0213308f-17cb-4398-b97e-01da7bd4804e" />
    <InputParameter Id="test.com" DataType="string" Value="c7026f88-4299-4cdb-965d-3f166464b8a9" />
    <InputParameter Id="errorOnFailedLookup" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainAppId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputParameterId**: test.com
- Parámetros de entrada:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **microsoft.com**: 0213308f-17cb-4398-b97e-01da7bd4804e
    - **test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **errorOnFailedLookup**: false
- Notificaciones de salida:
    - **outputClaim**: c7026f88-4299-4cdb-965d-3f166464b8a9

## <a name="nullclaim"></a>NullClaim

Limpiar el valor de una notificación determinada.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | claim_to_null | string | La notificación cambia su valor a null. |

Use esta transformación de notificaciones para quitar datos innecesarios de la bolsa de propiedades de notificaciones. De este modo, la cookie de sesión será menor. En el ejemplo siguiente se quita el valor del tipo de notificación `TermsOfService`.

```XML
<ClaimsTransformation Id="SetTOSToNull" TransformationMethod="NullClaim">
  <OutputClaims>
  <OutputClaim ClaimTypeReferenceId="TermsOfService" TransformationClaimType="claim_to_null" />
  </OutputClaims>
</ClaimsTransformation>
```

- Notificaciones de entrada:
    - **outputClaim**: Bienvenidos a Contoso App. Si continúa explorando y usando este sitio web, acepta las siguientes condiciones generales, que regirán su uso del sitio web...
- Notificaciones de salida:
    - **outputClaim**: NULL

## <a name="parsedomain"></a>ParseDomain

Obtiene la parte de dominio de una dirección de correo electrónico.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | emailAddress | string | ClaimType que contiene la dirección de correo electrónico. |
| OutputClaim | dominio | string | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones: el dominio. |

Use esta transformación de notificaciones para analizar el nombre de dominio después el símbolo @ del usuario. Esto puede ser útil para quitar información de identificación personal (PII) de los datos de auditoría. La siguiente transformación de notificaciones muestra cómo analizar el nombre de dominio desde una notificación de **email**.

```XML
<ClaimsTransformation Id="SetDomainName" TransformationMethod="ParseDomain">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="emailAddress" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="domain" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **emailAddress**: joe@outlook.com
- Notificaciones de salida:
    - **domain**: outlook.com

## <a name="setclaimsifstringsareequal"></a>SetClaimsIfStringsAreEqual

Comprueba que una notificación de cadena y el parámetro de entrada `matchTo` son iguales, y establece las notificaciones de salida con el valor presente en los parámetros de entrada `stringMatchMsg` y `stringMatchMsgCode`, junto con la notificación de salida del resultado de la comparación, que debe establecerse en `true` o `false` según el resultado de la comparación.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | string | Tipo de la notificación que se va a comparar. |
| InputParameter | matchTo | string | Cadena que se va a comparar con `inputClaim`. |
| InputParameter | stringComparison | string | Valores posibles: `Ordinal` o `OrdinalIgnoreCase`. |
| InputParameter | stringMatchMsg | string | Primer valor que debe establecerse si las cadenas son iguales. |
| InputParameter | stringMatchMsgCode | string | Segundo valor que debe establecerse si las cadenas son iguales. |
| OutputClaim | outputClaim1 | string | Si las cadenas son iguales, esta notificación de salida contiene el valor del parámetro de entrada `stringMatchMsg`. |
| OutputClaim | outputClaim2 | string | Si las cadenas son iguales, esta notificación de salida contiene el valor del parámetro de entrada `stringMatchMsgCode`. |
| OutputClaim | stringCompareResultClaim | boolean | Tipo de la notificación de salida del resultado de la comparación, que debe establecerse en `true` o `false` en función del resultado de la comparación. |

Puede usar esta transformación de notificaciones para comprobar si una notificación es igual a un valor que haya especificado. Por ejemplo, la siguiente transformación de notificaciones comprueba si el valor de la notificación **termsOfUseConsentVersion** es igual a `v1`. En caso afirmativo, cambie el valor a `v2`.

```XML
<ClaimsTransformation Id="CheckTheTOS" TransformationMethod="SetClaimsIfStringsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="v1" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="stringMatchMsg" DataType="string" Value="B2C_V1_90005" />
    <InputParameter Id="stringMatchMsgCode" DataType="string" Value="The TOS is upgraded to v2" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="outputClaim1" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeCode" TransformationClaimType="outputClaim2" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeResult" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim**: v1
- Parámetros de entrada:
    - **matchTo**: V1
    - **stringComparison**: ordinalIgnoreCase
    - **stringMatchMsg**:  B2C_V1_90005
    - **stringMatchMsgCode**:  The TOS is upgraded to v2
- Notificaciones de salida:
    - **outputClaim1**: B2C_V1_90005
    - **outputClaim2**: The TOS is upgraded to v2
    - **stringCompareResultClaim**: true

## <a name="setclaimsifstringsmatch"></a>SetClaimsIfStringsMatch

Comprueba que una notificación de cadena y el parámetro de entrada `matchTo` son iguales, y establece las notificaciones de salida con el valor presente en el parámetro de entrada `outputClaimIfMatched`, junto con la notificación de salida del resultado de la comparación, que debe establecerse en `true` o `false` según el resultado de la comparación.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | claimToMatch | string | Tipo de la notificación que se va a comparar. |
| InputParameter | matchTo | string | Cadena que se va a comparar con inputClaim. |
| InputParameter | stringComparison | string | Valores posibles: `Ordinal` o `OrdinalIgnoreCase`. |
| InputParameter | outputClaimIfMatched | string | Valor que debe establecerse si las cadenas son iguales. |
| OutputClaim | outputClaim | string | Si las cadenas son iguales, esta notificación de salida contiene el valor del parámetro de entrada `outputClaimIfMatched`. O null, si las cadenas no coinciden. |
| OutputClaim | stringCompareResultClaim | boolean | Tipo de la notificación de salida del resultado de la comparación, que debe establecerse en `true` o `false` en función del resultado de la comparación. |

Por ejemplo, la siguiente transformación de notificaciones comprueba si el valor de la notificación **ageGroup** es igual a `Minor`. En caso afirmativo, devuelve el valor para `B2C_V1_90001`.

```XML
<ClaimsTransformation Id="SetIsMinor" TransformationMethod="SetClaimsIfStringsMatch">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="ageGroup" TransformationClaimType="claimToMatch" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="Minor" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="outputClaimIfMatched" DataType="string" Value="B2C_V1_90001" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isMinor" TransformationClaimType="outputClaim" />
    <OutputClaim ClaimTypeReferenceId="isMinorResponseCode" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **claimToMatch**: Minor
- Parámetros de entrada:
    - **matchTo**: Minor
    - **stringComparison**: ordinalIgnoreCase
    - **outputClaimIfMatched**:  B2C_V1_90001
- Notificaciones de salida:
    - **isMinorResponseCode**: B2C_V1_90001
    - **isMinor**: true

