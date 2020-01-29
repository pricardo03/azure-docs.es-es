---
title: Definición de un perfil técnico de RESTful en una directiva personalizada
titleSuffix: Azure AD B2C
description: Defina un perfil técnico de RESTful en una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 00d5ba6fd86ea722270dfbe73324323bd831a529
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76263377"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Definición de un perfil técnico de RESTful en una directiva personalizada en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) proporciona compatibilidad con un servicio RESTful propio. Azure AD B2C envía datos al servicio RESTful en una colección de notificaciones de entrada y recibe los datos en una colección de notificaciones de salida. Con la integración del servicio RESTful, puede hacer lo siguiente:

- **Validar datos de entrada de usuario**: impide que se conserven datos con formato incorrecto en Azure AD B2C. Si el valor del usuario no es válido, el servicio REST devuelve el mensaje de error que indica al usuario que proporcione una entrada. Por ejemplo, puede comprobar que la dirección de correo electrónico que proporciona el usuario existe en la base de datos del cliente.
- **Sobrescribir notificaciones de entrada**: permite volver a formatear los valores de las notificaciones de entrada. Por ejemplo, si un usuario escribe el nombre con mayúsculas o minúsculas, se le puede dar un formato en el que solo la primera letra esté en mayúscula.
- **Enriquecer datos de usuario**: permite integración adicional con aplicaciones de línea de negocio corporativas. Por ejemplo, el servicio RESTful puede recibir la dirección de correo electrónico del usuario, consultar la base de datos del cliente y devolver el número de fidelidad del usuario a Azure AD B2C. Las notificaciones de devolución se pueden almacenar, evaluar en los siguientes pasos de orquestación, o bien incluir en el token de acceso.
- **Ejecutar lógica de negocios personalizada**: permite enviar notificaciones push, actualizar las bases de datos corporativas, ejecutar un proceso de migración de usuarios, administrar permisos, auditar bases de datos y realizar otras acciones.

La directiva puede enviar notificaciones de entrada a la API REST. La API REST también puede devolver notificaciones de salida que más adelante puede usar en la directiva, o bien puede generar un mensaje de error. La integración con los servicios REST se pueden diseñar de las siguientes maneras:

- **Perfil técnico de validación**: un perfil técnico de validación llama al servicio RESTful. El perfil técnico de validación valida los datos que proporciona el usuario antes de que continúe el recorrido del usuario. Con el perfil técnico de validación, se muestra un mensaje de error en una página autoafirmada y se devuelve en las notificaciones de salida.
- **Intercambio de notificaciones**: se realiza una llamada al servicio RESTful a través de un paso de orquestación. En este escenario, no hay ninguna interfaz de usuario para representar el mensaje de error. Si la API REST devuelve un error, se redirige al usuario a la aplicación de usuario de confianza con el mensaje de error.

## <a name="protocol"></a>Protocolo

El atributo **Name** del elemento **Protocol** tiene que establecerse en `Proprietary`. El atributo **handler** debe contener el nombre completo del ensamblado de controlador de protocolo que usa Azure AD B2C: `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

En el ejemplo siguiente se muestra un perfil técnico de RESTful:

```XML
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="input-claims"></a>Notificaciones de entrada

El elemento **InputClaims** contiene una lista de notificaciones para enviar a la API REST. También se puede asignar el nombre de la notificación al nombre definido en la API REST. En el ejemplo siguiente se muestra la asignación entre la directiva y la API REST. La notificación **givenName** se envía a la API REST como **firstName**, mientras que **surname** se envía como **lastName**. La notificación **email** se establece tal cual.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="email" />
  <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
  <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
</InputClaims>
```

El elemento **InputClaimsTransformations** puede contener una colección de elementos **InputClaimsTransformation** que se usan para modificar las notificaciones de entrada o generar otras nuevas antes del envío a la API REST.

## <a name="send-a-json-payload"></a>Envío de una carga de JSON

El perfil técnico de la API REST permite enviar una carga de JSON compleja a un punto de conexión.

Para enviar una carga de JSON compleja:

1. Compile la carga de JSON con la transformación de notificaciones [GenerateJson](json-transformations.md).
1. En el perfil técnico de la API REST:
    1. Agregue una transformación de notificaciones de entrada con una referencia a la transformación de notificaciones `GenerateJson`.
    1. Establezca la opción de metadatos `SendClaimsIn` en `body`.
    1. Establezca la opción de metadatos `ClaimUsedForRequestPayload` en el nombre de la notificación que contiene la carga de JSON.
    1. En la notificación de entrada, agregue una referencia a la notificación de entrada que contiene la carga de JSON.

En el siguiente ejemplo, `TechnicalProfile` envía un correo electrónico de verificación mediante un servicio de correo electrónico de terceros (en este caso, SendGrid).

```XML
<TechnicalProfile Id="SendGrid">
  <DisplayName>Use SendGrid's email API to send the code the the user</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://api.sendgrid.com/v3/mail/send</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="ClaimUsedForRequestPayload">sendGridReqBody</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_SendGridApiKey" />
  </CryptographicKeys>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="GenerateSendGridRequestBody" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="sendGridReqBody" />
  </InputClaims>
</TechnicalProfile>
```

## <a name="output-claims"></a>Notificaciones de salida

El elemento **OutputClaims** contiene una lista de notificaciones devuelta por la API REST. Es posible que tenga que asignar el nombre de la notificación definida en la directiva al nombre definido en la API REST. También puede incluir las notificaciones que la API REST no devuelve, siempre y cuando establezca el atributo `DefaultValue`.

El elemento **OutputClaimsTransformations** puede contener una colección de elementos **OutputClaimsTransformation** que se usan para modificar las notificaciones de salida o para generar nuevas.

En el ejemplo siguiente se muestra la notificación devuelta por la API REST:

- La notificación **MembershipId** que se asigna al nombre de notificación **loyaltyNumber**.

El perfil técnico también devuelve notificaciones, que no son devueltas por el proveedor de identidades:

- La notificación **loyaltyNumberIsNew** que tiene un valor predeterminado establecido en `true`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="MembershipId" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumberIsNew" DefaultValue="true" />
</OutputClaims>
```

## <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ServiceUrl | Sí | La dirección URL del punto de conexión de la API REST. |
| AuthenticationType | Sí | El tipo de autenticación realizada por el proveedor de notificaciones RESTful. Valores posibles: `None`, `Basic`, `Bearer` o `ClientCertificate`. El valor `None` indica que la API REST no es anónima. El valor `Basic` indica que la API REST se protege con autenticación básica HTTP. Solo los usuarios verificados, incluido Azure AD B2C, pueden acceder a la API. El valor `ClientCertificate` (recomendado) indica que la API REST restringe el acceso mediante la autenticación de certificado de cliente. Solo pueden acceder a la API los servicios que tengan los certificados adecuados; por ejemplo, Azure AD B2C. El valor `Bearer` indica que la API REST restringe el acceso mediante el token de portador de OAuth2 de cliente. |
| SendClaimsIn | No | Especifica cómo se envían las notificaciones de entrada al proveedor de notificaciones RESTful. Valores posibles: `Body` (predeterminado), `Form`, `Header` o `QueryString`. El valor `Body` es la notificación de entrada que se envía en el cuerpo de la solicitud en formato JSON. El valor `Form` es la notificación de entrada que se envía en el cuerpo de la solicitud en un formato de valor de clave separado por "&" (Y comercial). El valor `Header` es la notificación de entrada que se envía en el cuerpo de la solicitud. El valor `QueryString` es la notificación de entrada que se envía en la cadena de consulta de la solicitud. Los verbos HTTP invocados por cada uno de ellos son los siguientes:<br /><ul><li>`Body`: POST</li><li>`Form`: POST</li><li>`Header`: GET</li><li>`QueryString`: GET</li></ul> |
| ClaimsFormat | No | Especifica el formato de las notificaciones de salida. Valores posibles: `Body` (predeterminado), `Form`, `Header` o `QueryString`. El valor `Body` es la notificación de salida que se envía en el cuerpo de la solicitud en formato JSON. El valor `Form` es la notificación de salida que se envía en el cuerpo de la solicitud en un formato de valor de clave separado por "&" (Y comercial). El valor `Header` es la notificación de salida que se envía en el cuerpo de la solicitud. El valor `QueryString` es la notificación de salida que se envía en la cadena de consulta de la solicitud. |
| ClaimUsedForRequestPayload| No | Nombre de una notificación de cadena que contiene la carga que se va a enviar a la API REST. |
| DebugMode | No | Ejecuta el perfil técnico en modo de depuración. Valores posibles: `true` o `false` (valor predeterminado). En el modo de depuración, la API REST puede devolver más información. consulte la sección [Devolución de mensajes de error](#returning-error-message). |

## <a name="cryptographic-keys"></a>Claves de cifrado

Si el tipo de autenticación se establece en `None`, no se usa el elemento **CryptographicKeys**.

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
</TechnicalProfile>
```

Si el tipo de autenticación se establece en `Basic`, el elemento **CryptographicKeys** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| BasicAuthenticationUsername | Sí | Nombre de usuario que se usa para la autenticación. |
| BasicAuthenticationPassword | Sí | Contraseña que se usa para la autenticación. |

En el ejemplo siguiente se muestra un perfil técnico con autenticación básica:

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Basic</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
  </CryptographicKeys>
</TechnicalProfile>
```

Si el tipo de autenticación se establece en `ClientCertificate`, el elemento **CryptographicKeys** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ClientCertificate | Sí | El certificado X509 (conjunto de claves RSA) que se va a usar para la autenticación. |

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">ClientCertificate</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
  </CryptographicKeys>
</TechnicalProfile>
```

Si el tipo de autenticación se establece en `Bearer`, el elemento **CryptographicKeys** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| BearerAuthenticationToken | No | El token de portador de OAuth 2.0. |

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_B2cRestClientAccessToken" />
  </CryptographicKeys>
</TechnicalProfile>
```

## <a name="returning-error-message"></a>Devolución de mensajes de error

Es posible la API REST tenga que devolver un mensaje de error, como "No se encuentra el usuario en el sistema CRM". Si se produce un error, la API REST debe devolver un mensaje de error HTTP 409 (código de estado de respuesta de conflicto) con los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| version | Sí | 1.0.0 |
| status | Sí | 409 |
| código | No | Código de error del proveedor de punto de conexión RESTful, que se muestra cuando `DebugMode` está habilitado. |
| requestId | No | Identificador de solicitud del proveedor de punto de conexión RESTful, que se muestra cuando `DebugMode` está habilitado. |
| userMessage | Sí | Mensaje de error que se muestra al usuario. |
| developerMessage | No | Descripción detallada del problema y cómo corregirlo, que se muestra cuando `DebugMode` está habilitado. |
| moreInfo | No | URI que señala a información adicional, que se muestra cuando `DebugMode` está habilitado. |

En el ejemplo siguiente se muestra una API REST que devuelve un mensaje de error con formato JSON:

```JSON
{
  "version": "1.0.0",
  "status": 409,
  "code": "API12345",
  "requestId": "50f0bd91-2ff4-4b8f-828f-00f170519ddb",
  "userMessage": "Message for the user",
  "developerMessage": "Verbose description of problem and how to fix it.",
  "moreInfo": "https://restapi/error/API12345/moreinfo"
}
```

En el ejemplo siguiente se muestra una clase de C# que devuelve un mensaje de error:

```csharp
public class ResponseContent
{
  public string version { get; set; }
  public int status { get; set; }
  public string code { get; set; }
  public string userMessage { get; set; }
  public string developerMessage { get; set; }
  public string requestId { get; set; }
  public string moreInfo { get; set; }
}
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener ejemplos del uso de un perfil técnico de RESTful:

- [Integración de intercambios de notificaciones de API REST en el recorrido del usuario de Azure AD B2C como validación de la entrada del usuario](active-directory-b2c-custom-rest-api-netfw.md)
- [Protección de los servicios RESTful mediante la autenticación HTTP básica](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
- [Protección de los servicios RESTful mediante certificados de cliente](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
- [Tutorial: Integración de intercambios de notificaciones de API REST en el recorrido del usuario de Azure AD B2C como validación de la entrada del usuario](active-directory-b2c-rest-api-validation-custom.md)
