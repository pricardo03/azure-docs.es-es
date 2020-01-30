---
title: Intercambios de notificaciones de API REST como validación
titleSuffix: Azure AD B2C
description: Tutorial para la creación de un recorrido del usuario de Azure AD B2C que interactúe con servicios RESTful.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/21/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 209cddcc8648f7d396f048057bb4367e2ae9f66c
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850363"
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Tutorial: Integración de intercambios de notificaciones de API REST en el recorrido del usuario de Azure AD B2C como validación de la entrada del usuario

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

El marco de experiencia de identidad (IEF) subyacente a Azure Active Directory B2C (Azure AD B2C) permite al desarrollador de identidades integrar una interacción con una API de RESTful en un recorrido del usuario.

Al final de este tutorial podrá crear un recorrido del usuario de Azure AD B2C que interactúe con servicios RESTful.

El IEF envía datos en notificaciones y recibe los datos en notificaciones. La interacción con la API:

- Se puede diseñar como un intercambio de notificaciones de API de REST, a modo de un perfil de validación, que tiene lugar dentro de un paso de orquestación.
- Normalmente valida la entrada del usuario. Si se rechaza el valor del usuario, el usuario puede intentar escribir de nuevo un valor válido con la posibilidad de que reciba un mensaje de error.

La interacción también se puede diseñar como un paso de orquestación. Para más información, consulte [Tutorial: Integración de intercambios de notificaciones de API REST en los recorridos de usuario de Azure AD B2C como un paso de orquestación](custom-policy-rest-api-claims-exchange.md).

En el ejemplo de perfil de validación, usaremos el recorrido de usuario de edición de perfil del archivo del paquete de inicio ProfileEdit.xml.

Podemos comprobar que el nombre proporcionado por el usuario en la edición del perfil no es parte de una lista de exclusión.

## <a name="prerequisites"></a>Prerequisites

- Configuración de un inquilino de Azure AD B2C para completar el registro o inicio de sesión de una cuenta local como se describe en [Introducción](custom-policy-get-started.md).
- Un punto de conexión de API de REST con el que interactuar. En este tutorial, hemos configurado un sitio de demostración denominado [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) con un servicio de API de REST.

## <a name="step-1-prepare-the-rest-api-function"></a>Paso 1: Preparación de la función de API REST

> [!NOTE]
> La configuración de funciones de la API de REST está fuera del ámbito de este artículo. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) proporciona un excelente conjunto de herramientas para crear servicios de RESTful en la nube.

Hemos creado una función de Azure que recibe una notificación que espera como `playerTag`. La función valida si existe esta notificación. Puede acceder al código completo de la función de Azure en [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"The player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

El IEF espera la notificación `userMessage` que devuelve la función de Azure. Esta notificación se le presentará al usuario como una cadena si se produce un error en la validación, por ejemplo, si se devuelve un estado de conflicto 409 en el ejemplo anterior.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>Paso 2: Configuración del intercambio de notificaciones de API RESTful como perfil técnico en el archivo TrustFrameworkExtensions.xml

Un perfil técnico es la configuración completa del intercambio deseado con el servicio RESTful. Abra el archivo TrustFrameworkExtensions.xml y agregue el siguiente fragmento de código XML dentro del elemento `<ClaimsProviders>`.

> [!NOTE]
> En el siguiente código XML, el proveedor de RESTful `Version=1.0.0.0` se describe como el protocolo. Considérelo como la función que interactuará con el servicio externo. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="SendClaimsIn">Body</Item>
                <!-- Set AuthenticationType to Basic or ClientCertificate in production environments -->
                <Item Key="AuthenticationType">None</Item>
                <!-- REMOVE the following line in production environments -->
                <Item Key="AllowInsecureAuthInProduction">true</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

El elemento `InputClaims` define las notificaciones que se enviarán desde el IEF al servicio REST. En este ejemplo, el contenido de la notificación `givenName` se enviará al servicio REST como `playerTag`. En este ejemplo, el IEF no espera que se devuelvan notificaciones. En su lugar, espera una respuesta del servicio REST y actúa en función de los códigos de estado recibidos.

Los comentarios anteriores `AuthenticationType` y `AllowInsecureAuthInProduction` especifican los cambios que se deben realizar al pasar a un entorno de producción. Para obtener información sobre cómo proteger las API de RESTful para producción, vea [Protección de las API de RESTful con autenticación básica](secure-rest-api-dotnet-basic-auth.md) y [Protección de las API de RESTful con autenticación de certificado](secure-rest-api-dotnet-certificate-auth.md).

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a>Paso 3: Inclusión del intercambio de notificaciones del servicio RESTful en un perfil técnico autoafirmado donde quiere validar la entrada del usuario

El uso más común del paso de validación es en la interacción con un usuario. Todas las interacciones en las que se espera que el usuario proporcione una entrada son *perfiles técnicos autoafirmados*. En este ejemplo, agregaremos esta validación al perfil técnico Self-Asserted-ProfileUpdate. Se trata del perfil técnico que usa el archivo de directiva de usuario de confianza (RP) `Profile Edit`.

Para agregar el intercambio de notificaciones al perfil técnico autoafirmado:

1. Abra el archivo TrustFrameworkBase.xml y busque `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Revise la configuración de este perfil técnico. Observe cómo el intercambio con el usuario se define como notificaciones que se pedirán al usuario (notificaciones de entrada) y notificaciones que se espera que devuelva el proveedor autoafirmado (notificaciones de salida).
3. Busque `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`; observe que este perfil se invoca como el paso de orquestación n.º 5 de `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a>Paso 4: Carga y prueba del archivo de directiva RP de edición de perfil

1. Cargue la nueva versión del archivo TrustFrameworkExtensions.xml.
2. Use **Ejecutar ahora** para probar el archivo de directiva RP de edición de perfil.
3. Para probar la validación, proporcione uno de los nombres existentes (por ejemplo: mcvinny) en el campo **Nombre propio**. Si todo está configurado correctamente, debería recibir un mensaje que informa al usuario de que la etiqueta player ya está en uso.

## <a name="next-steps"></a>Pasos siguientes

[Modificación de la edición de perfil y el registro de usuario para recopilar información adicional de sus usuarios](custom-policy-custom-attributes.md)

[Tutorial: Integración de intercambios de notificaciones de API REST en los recorridos de usuario de Azure AD B2C como un paso de orquestación](custom-policy-rest-api-claims-exchange.md)

[Referencia: Perfil técnico de RESTful](restful-technical-profile.md)

Para obtener información sobre cómo proteger las API, vea los siguientes artículos:

* [Secure your RESTful API with basic authentication (username and password)](secure-rest-api-dotnet-basic-auth.md) (Protección de las API de RESTful con autenticación básica [nombre de usuario y contraseña])
* [Secure your RESTful API with client certificates](secure-rest-api-dotnet-certificate-auth.md) (Protección de las API de RESTful con certificados de cliente)
