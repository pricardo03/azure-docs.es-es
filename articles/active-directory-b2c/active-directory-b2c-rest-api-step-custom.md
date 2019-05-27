---
title: 'Azure Active Directory B2C: intercambios de notificaciones de API de REST | Microsoft Docs'
description: Agregue los intercambios de notificaciones de API de REST para las directivas personalizadas de B2C de Active Directory.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: e705c12782310597ea14d5253aba8b6a1a004e6d
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65952779"
---
# <a name="add-rest-api-claims-exchanges-to-custom-policies-in-azure-active-directory-b2c"></a>Agregue los intercambios de notificaciones de API de REST para las directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Puede agregar la interacción con una API RESTful en su [directivas personalizadas](active-directory-b2c-overview-custom.md) en Azure Active Directory (Azure AD) B2C. Este artículo muestra cómo crear un recorrido del usuario de Azure AD B2C que interactúe con servicios RESTful.

La interacción incluye un intercambio de notificaciones de información entre las notificaciones de API de REST y Azure AD B2C. Intercambios de notificaciones tienen las siguientes características:

- Puede diseñarse como un paso de orquestación.
- Puede desencadenar una acción externa. Por ejemplo, puede registrar un evento en una base de datos externa.
- Puede usarse para capturar un valor y almacenarlo en la base de datos de usuario.
- Puede cambiar el flujo de ejecución. 

El escenario que se representa en este artículo incluye las siguientes acciones:

1. Buscar al usuario en un sistema externo.
2. Obtener la ciudad donde está registrado.
3. Devolver ese atributo como una notificación a la aplicación.

## <a name="prerequisites"></a>Requisitos previos

- Realice los pasos del artículo [Introducción a las directivas personalizadas](active-directory-b2c-get-started-custom.md).
- Un punto de conexión de API de REST con el que interactuar. Este artículo se utiliza un simple Azure función como ejemplo. Para crear la función de Azure, consulte [crear su primera función en Azure portal](../azure-functions/functions-create-first-azure-function.md).

## <a name="prepare-the-api"></a>Preparación de la API

En esta sección, prepare la función de Azure para recibir un valor para `email`y, a continuación, devolver el valor de `city` que puede utilizarse por Azure AD B2C como una notificación.

Cambie el archivo run.csx para la función de Azure que ha creado para usar el código siguiente: 

```
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

Abra el *TrustFrameworkExtensions.xml* archivo y agregue los siguientes elementos XML dentro de la **ClaimsProvider** elemento.

```XML
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AzureFunctions-WebHook">
      <DisplayName>Azure Function Web Hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://myfunction.azurewebsites.net/api/HttpTrigger1?code=bAZ4lLy//ZHZxmncM8rI7AgjQsrMKmVXBpP0vd9smOzdXDDUIaLljA==</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
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

El **InputClaims** elemento define las notificaciones que se envían al servicio REST. En este ejemplo, el valor de la notificación `givenName` se envía al servicio REST como la notificación `email`. El **OutputClaims** elemento define las notificaciones que esperan obtenerse desde el servicio REST.

## <a name="add-the-claim-definition"></a>Agregue la definición de notificación

Agregue una definición para `city` dentro de la **BuildingBlocks** elemento. Encontrará dicho elemento al principio del archivo TrustFrameworkExtensions.xml.

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

Agregar un paso al recorrido de usuario de edición de perfil. Después de que el usuario está autenticado (pasos 1 a 4 de orquestación en el siguiente código XML) y el usuario ha proporcionado la información de perfil actualizada (paso 5). Copie el perfil de edición de código XML del recorrido de usuario desde el *TrustFrameworkBase.xml* del archivo a su *TrustFrameworkExtensions.xml* archivo dentro de la **UserJourneys** elemento. A continuación, realizar la modificación como paso 6.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
  </ClaimsExchanges>
</OrchestrationStep>
```

El XML del recorrido del usuario final debe parecerse a este ejemplo:

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
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
  </OrchestrationSteps>
  <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="add-the-claim"></a>Agregue la notificación

Editar el *ProfileEdit.xml* archivo y agregue `<OutputClaim ClaimTypeReferenceId="city" />` a la **OutputClaims** elemento.

Después de agregar la nueva notificación, el perfil técnico se parecerá a este ejemplo:

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="upload-your-changes-and-test"></a>Carga de los cambios y prueba

1. (Opcional): Guarde la versión existente (mediante descarga) de los archivos antes de continuar.
2. Cargar el *TrustFrameworkExtensions.xml* y *ProfileEdit.xml* y seleccione esta opción para sobrescribir el archivo existente.
3. Seleccione **B2C_1A_ProfileEdit**.
4. Para **seleccione aplicación** en la página información general de la directiva personalizada, seleccione la aplicación web denominada *webapp1* que registró anteriormente. Asegúrese de que el **dirección URL de respuesta** es `https://jwt.ms`.
4. Seleccione **ejecutar ahora**. Inicie sesión con sus credenciales de cuenta y haga clic en **continuar**.

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

- También puede diseñar la interacción como un perfil de validación. Para más información, consulte [Tutorial: Integración de intercambios de notificaciones de API REST en el recorrido del usuario de Azure AD B2C como validación de la entrada del usuario](active-directory-b2c-rest-api-validation-custom.md).
- [Modificación de la edición de perfil para recopilar información adicional de sus usuarios](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
