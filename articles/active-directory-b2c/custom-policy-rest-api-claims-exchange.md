---
title: 'Intercambios de notificaciones de la API de REST: Azure Active Directory B2C'
description: Agregue los intercambios de notificaciones de la API de REST a directivas personalizadas de Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/21/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 351b41f45fb84384ec0193f8e3130347d0b19401
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/29/2020
ms.locfileid: "78189096"
---
# <a name="add-rest-api-claims-exchanges-to-custom-policies-in-azure-active-directory-b2c"></a>Agregue los intercambios de notificaciones de la API de REST a directivas personalizadas de Azure Active Directory B2C.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Puede agregar interacción con una API de RESTful a sus [directivas personalizadas](custom-policy-overview.md) en Azure Active Directory B2C (Azure AD B2C). En este artículo se muestra cómo crear un recorrido del usuario de Azure AD B2C que interactúe con servicios RESTful.

La interacción incluye un intercambio de notificaciones de información entre las notificaciones de la API de REST y Azure AD B2C. El intercambio de notificaciones tiene las siguientes características:

- Puede diseñarse como un paso de orquestación.
- Puede desencadenar una acción externa. Por ejemplo, puede registrar un evento en una base de datos externa.
- Puede usarse para capturar un valor y almacenarlo en la base de datos de usuario.
- Puede cambiar el flujo de ejecución.

El escenario que se representa en este artículo incluye las siguientes acciones:

1. Buscar al usuario en un sistema externo.
2. Obtener la ciudad donde está registrado.
3. Devolver ese atributo como una notificación a la aplicación.

## <a name="prerequisites"></a>Prerrequisitos

- Realice los pasos del artículo [Introducción a las directivas personalizadas](custom-policy-get-started.md).
- Un punto de conexión de API de REST con el que interactuar. En este artículo se usa una función simple de Azure como ejemplo. Para crear la función de Azure, consulte [Creación de su primera función en Azure Portal](../azure-functions/functions-create-first-azure-function.md).

## <a name="prepare-the-api"></a>Preparar la API

En esta sección, preparará la función de Azure para recibir un valor para `email` y, luego, devolverá el valor a `city` para que Azure AD B2C pueda usarlo como notificación.

Cambie el archivo run.csx para la función de Azure que ha creado para usar el código siguiente:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
  log.LogInformation("C# HTTP trigger function processed a request.");
  string email = req.Query["email"];
  string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
  dynamic data = JsonConvert.DeserializeObject(requestBody);
  email = email ?? data?.email;

  return email != null
    ? (ActionResult)new OkObjectResult(
      new ResponseContent
      {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        city = "Redmond"
      })
      : new BadRequestObjectResult("Please pass an email on the query string or in the request body");
}

public class ResponseContent
{
    public string version { get; set; }
    public int status { get; set; }
    public string city {get; set; }
}
```

## <a name="configure-the-claims-exchange"></a>Configurar el intercambio de notificaciones

Un perfil técnico proporciona la configuración para el intercambio de notificaciones.

Abra el archivo *TrustFrameworkExtensions.xml* y agregue el siguiente elemento XML **ClaimsProvider** dentro del elemento **ClaimsProviders**.

```XML
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AzureFunctions-WebHook">
      <DisplayName>Azure Function Web Hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://myfunction.azurewebsites.net/api/HttpTrigger1?code=bAZ4lLy//ZHZxmncM8rI7AgjQsrMKmVXBpP0vd9smOzdXDDUIaLljA==</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <!-- Set AuthenticationType to Basic or ClientCertificate in production environments -->
        <Item Key="AuthenticationType">None</Item>
        <!-- REMOVE the following line in production environments -->
        <Item Key="AllowInsecureAuthInProduction">true</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
      </OutputClaims>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

El elemento **InputClaims** define las notificaciones que se envían al servicio REST. En este ejemplo, el valor de la notificación `givenName` se envía al servicio REST como la notificación `email`. El elemento **OutputClaims** define las notificaciones que se esperan del servicio REST.

Los comentarios anteriores `AuthenticationType` y `AllowInsecureAuthInProduction` especifican los cambios que se deben realizar al pasar a un entorno de producción. Para obtener información sobre cómo proteger las API de RESTful para producción, vea [Protección de las API de RESTful con autenticación básica](secure-rest-api-dotnet-basic-auth.md) y [Protección de las API de RESTful con autenticación de certificado](secure-rest-api-dotnet-certificate-auth.md).

## <a name="add-the-claim-definition"></a>Agregar la definición de notificación

Agregue una definición para `city` dentro del elemento **BuildingBlocks**. Encontrará dicho elemento al principio del archivo TrustFrameworkExtensions.xml.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="city">
      <DisplayName>City</DisplayName>
      <DataType>string</DataType>
      <UserHelpText>Your city</UserHelpText>
      <UserInputType>TextBox</UserInputType>
    </ClaimType>
  </ClaimsSchema>
</BuildingBlocks>
```

## <a name="add-an-orchestration-step"></a>Agregar un paso de orquestación

Hay muchos casos donde la llamada de API de REST puede usarse como paso de orquestación. Como tal, se puede usar como actualización a un sistema externo una vez que un usuario haya realizado correctamente una tarea, por ejemplo, el registro por primera vez, o como actualización de perfil para mantener la información sincronizada. En este caso, se usa para aumentar la información proporcionada a la aplicación después de la edición del perfil.

Agregue un paso al recorrido del usuario de edición de perfil. Después de que el usuario se haya autenticado (pasos de orquestación del 1 al 4 en el siguiente código XML) y haya proporcionado la información de perfil actualizada (paso 5). Copie el código XML del recorrido del usuario de edición de perfil del archivo *TrustFrameworkBase.xml* en el archivo *TrustFrameworkExtensions.xml* dentro del elemento **UserJourneys**. Después, haga la modificación del paso 6.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-WebHook" />
  </ClaimsExchanges>
</OrchestrationStep>
```

El código XML final del recorrido del usuario debe tener el aspecto de este ejemplo:

```XML
<UserJourney Id="ProfileEdit">
  <OrchestrationSteps>
    <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
      <ClaimsProviderSelections>
        <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
        <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
      </ClaimsProviderSelections>
    </OrchestrationStep>
    <OrchestrationStep Order="2" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
        <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="3" Type="ClaimsExchange">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>authenticationSource</Value>
          <Value>localAccountAuthentication</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <ClaimsExchanges>
        <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="4" Type="ClaimsExchange">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>authenticationSource</Value>
          <Value>socialIdpAuthentication</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <ClaimsExchanges>
        <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="5" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <!-- Add a step 6 to the user journey before the JWT token is created-->
    <OrchestrationStep Order="6" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-WebHook" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
  </OrchestrationSteps>
  <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="add-the-claim"></a>Agregar la notificación

Edite el archivo *ProfileEdit.xml* y agregue `<OutputClaim ClaimTypeReferenceId="city" />` al elemento **OutputClaims**.

Después de agregar la nueva notificación, el perfil técnico tendrá el aspecto de este ejemplo:

```XML
<TechnicalProfile Id="PolicyProfile">
  <DisplayName>PolicyProfile</DisplayName>
  <Protocol Name="OpenIdConnect" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
    <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
    <OutputClaim ClaimTypeReferenceId="city" />
  </OutputClaims>
  <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="upload-your-changes-and-test"></a>Carga de los cambios y prueba

1. (Opcional): Guarde la versión existente (mediante descarga) de los archivos antes de continuar.
2. Cargue los archivos *TrustFrameworkExtensions.xml* y *ProfileEdit.xml* y seleccione para sobrescribir el archivo existente.
3. Seleccione **B2C_1A_ProfileEdit**.
4. Para **Seleccionar aplicación** en la página Información general de la directiva personalizada, seleccione la aplicación web denominada *webapp1* que ha registrado antes. Asegúrese de que la **URL de respuesta** sea `https://jwt.ms`.
4. Seleccione **Ejecutar ahora**. Inicie sesión con las credenciales de su cuenta y haga clic en **Continuar**.

Si todo está configurado correctamente, el token incluye la nueva notificación `city`, con el valor `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_profileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Pasos siguientes

También puede diseñar la interacción como un perfil de validación. Para más información, consulte [Tutorial: Integración de intercambios de notificaciones de API REST en el recorrido del usuario de Azure AD B2C como validación de la entrada del usuario](custom-policy-rest-api-claims-validation.md).

[Modificación de la edición de perfil para recopilar información adicional de sus usuarios](custom-policy-custom-attributes.md)

[Referencia: Perfil técnico de RESTful](restful-technical-profile.md)

Para obtener información sobre cómo proteger las API, vea los siguientes artículos:

* [Secure your RESTful API with basic authentication (username and password)](secure-rest-api-dotnet-basic-auth.md) (Protección de las API de RESTful con autenticación básica [nombre de usuario y contraseña])
* [Secure your RESTful API with client certificates](secure-rest-api-dotnet-certificate-auth.md) (Protección de las API de RESTful con certificados de cliente)
