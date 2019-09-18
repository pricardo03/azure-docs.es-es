---
title: Errores y excepciones (MSAL) | Azure
description: Aprenda a controlar errores y excepciones, obtenga más información sobre el acceso condicional y el desafío de notificaciones en las aplicaciones de MSAL.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/08/2019
ms.author: negoe
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 280746281fd45b3286cc76be5d3483f0cc65f90f
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872792"
---
# <a name="handling-exceptions-and-errors-using-msal"></a>Control de excepciones y errores con MSAL

Las excepciones de la biblioteca de autenticación de Microsoft (MSAL) están diseñadas para que los desarrolladores de aplicaciones solucionen problemas y no para mostrarse a los usuarios finales. Los mensajes de excepción no están traducidos.

Cuando procesa excepciones y errores, puede usar el propio tipo de excepción y el código de error para distinguir entre las excepciones.  Para ver una lista con los códigos de error, consulte [Códigos de error de autenticación y autorización](reference-aadsts-error-codes.md).

## <a name="net-exceptions"></a>Excepciones .NET
Cuando procesa excepciones puede usar el propio tipo de excepción y el miembro `ErrorCode` para distinguir entre las excepciones. Los valores de `ErrorCode` son constantes del tipo [MsalError](/dotnet/api/microsoft.identity.client.msalerror?view=azure-dotnet).

También puede echar un vistazo a los campos de [MsalClientException](/dotnet/api/microsoft.identity.client.msalexception?view=azure-dotnet), [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) y [MsalUIRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet).

Si se produce una excepción [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet), el código de error podría contener algún código que puede consultar en [códigos de error de autenticación y autorización](reference-aadsts-error-codes.md).

### <a name="common-exceptions"></a>Excepciones comunes
Estas son las excepciones comunes que pueden producirse y algunas de las posibles mitigaciones.

| Excepción | Código de error | Mitigación|
| --- | --- | --- |
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS65001: The user or administrator has not consented to use the application with ID '{appId}' named '{appName}'. Send an interactive authorization request for this user and resource (Ni el usuario ni el administrador han dado consentimiento para usar la aplicación con ID "{appId}" llamada "{appName}". Envíe una solicitud de autorización interactiva para este usuario y este recurso).| Debe obtener primero el consentimiento del usuario. Si no está utilizando .NET Core (que no tiene ninguna interfaz de usuario web), llame a `AcquireTokeninteractive` (solo una vez). Si lo utiliza o si no quiere realizar una operación `AcquireTokenInteractive`, el usuario puede ir a una dirección URL para dar su consentimiento: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read. Para llamar a `AcquireTokenInteractive`: `app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS50079: The user is required to use multi-factor authentication (Es necesario que el usuario utilice la autenticación multifactor).| No hay ninguna mitigación. Si la autenticación multifactor está configurada para el inquilino y AAD decide aplicarla, deberá recurrir a un flujo interactivo como `AcquireTokenInteractive` o `AcquireTokenByDeviceCode`.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) |AADSTS90010: The grant type is not supported over the */common* or */consumers* endpoints. Use the */organizations* or tenant-specific endpoint. You used */common*. (El tipo de concesión no es compatible con los puntos de conexión /common o /consumers. Use /organizations o el punto de conexión específico del inquilino. Usó /common).| Como se ha explicado en el mensaje de Azure AD, la autoridad debe tener un inquilino o, en caso contrario, */organizations*.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) | AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion' (El cuerpo de la solicitud debe contener el siguiente parámetro: "client_secret o client_assertion").| Esta excepción puede ocurrir si la aplicación no estaba registrada como una aplicación cliente pública en Azure AD. En Azure Portal, edite el manifiesto de la aplicación y establezca `allowPublicClient` en `true`. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| unknown_user Message: Could not identify logged in user (No se pudo identificar al usuario que inició sesión).| La biblioteca no pudo consultar el usuario actual de Windows que ha iniciado sesión o este usuario no está unido a AD o AAD (los usuarios unidos a un lugar de trabajo no son compatibles). Mitigación 1: en UWP, compruebe que la aplicación tiene las siguientes funcionalidades: Autenticación empresarial, Redes privadas (cliente y servidor), Información de la cuenta de usuario. Mitigación2: Implemente su propia lógica para capturar el nombre de usuario (por ejemplo, john@contoso.com) y use el formulario `AcquireTokenByIntegratedWindowsAuth` que toma el nombre de usuario.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Este método se basa en un protocolo que Active Directory (AD) expone. Si un usuario se creó en Azure Active Directory sin respaldo de AD (usuario "administrado"), se producirá un error en este método. Los usuarios creados en AD y con respaldo de AAD (usuarios "federados") pueden beneficiarse de este método de autenticación no interactiva. Mitigación: Use la autenticación interactiva.|

### `MsalUiRequiredException`

Uno de los códigos de estado comunes que devuelve MSAL.NET al llamar a `AcquireTokenSilent()` es `MsalError.InvalidGrantError`. Este código de estado implica que la aplicación debe volver a llamar a la biblioteca de autenticación, aunque en modo interactivo (AcquireTokenInteractive o AcquireTokenByDeviceCodeFlow para las aplicaciones cliente públicas y realizar un desafío en las aplicaciones web). Esto se debe a que se requiere interacción adicional del usuario para emitir el token de autenticación.

La mayoría de las veces en que `AcquireTokenSilent` produce un error, se debe a que la memoria caché del token no tiene tokens que coincidan con la solicitud. Los tokens de acceso expiran en 1 hora y `AcquireTokenSilent` intentará capturar uno nuevo en función de un token de actualización (en términos de OAuth2, este es el flujo de "actualización de token"). Este flujo también puede producir un error por diversos motivos; por ejemplo, si un administrador de inquilinos configura directivas de inicio de sesión más rigurosas. 

La interacción tiene el objetivo de hacer que el usuario realice una acción. Algunas de esas condiciones son fáciles de solucionar para los usuarios (por ejemplo, aceptar las condiciones de uso con un solo clic) y otras no se pueden solucionar con la configuración actual (por ejemplo, el equipo en cuestión necesita conectarse a una red corporativa específica). Algunas ayudan al usuario a configurar la autenticación multifactor o a instalar Microsoft Authenticator en su dispositivo.

### <a name="msaluirequiredexception-classification-enumeration"></a>Enumeración de la clasificación `MsalUiRequiredException`

MSAL expone un campo `Classification` que puede leer para ofrecer una mejor experiencia de usuario. Por ejemplo, puede indicar al usuario que su contraseña expiró o que necesitará dar su consentimiento para usar ciertos recursos. Los valores admitidos son parte de la enumeración `UiRequiredExceptionClassification`:

| clasificación    | Significado           | Uso recomendado |
|-------------------|-------------------|----------------------|
| BasicAction | La interacción del usuario puede resolver la condición durante el flujo de autenticación interactiva. | Llame a AcquireTokenInteractively(). |
| AdditionalAction | La interacción de corrección con el sistema puede resolver la condición, independientemente del flujo de autenticación interactiva. | Llame a AcquireTokenInteractively() para mostrar un mensaje que explique la acción correctiva. La aplicación que realiza la llamada puede ocultar los flujos que requieren acciones adicionales si es poco probable que el usuario complete la acción correctiva. |
| MessageOnly      | No se puede resolver la condición en este momento. Al iniciar el flujo de autenticación interactiva, se mostrará un mensaje que explica la condición. | Llame a AcquireTokenInteractively() para mostrar un mensaje que explique la condición. AcquireTokenInteractively() devolverá el error UserCanceled después de que el usuario lea el mensaje y cierre la ventana. La aplicación que realiza la llamada puede optar por ocultar los flujos que dan como resultado solo un mensaje si es poco probable que el usuario se beneficie del mensaje.|
| ConsentRequired  | Falta el consentimiento del usuario o se ha revocado. | Llame a AcquireTokenInteractively() para que el usuario dé su consentimiento. |
| UserPasswordExpired | La contraseña del usuario ha expirado. | Llame a AcquireTokenInteractively() para que el usuario pueda restablecer su contraseña. |
| PromptNeverFailed| Se llamó a la autenticación interactiva con el parámetro prompt=never; esto obliga a MSAL a basarse en las cookies del explorador y no mostrar el explorador. Se produjo un error. | Llame a AcquireTokenInteractively() sin Prompt.None. |
| AcquireTokenSilentFailed | El SDK de MSAL no tiene suficiente información para capturar un token de la memoria caché. Esto puede deberse a que no hay tokens en la memoria caché o a que no se encontró una cuenta. El mensaje de error contiene más detalles.  | Llame a AcquireTokenInteractively(). |
| None    | No se proporcionan más detalles. La interacción del usuario puede resolver la condición durante el flujo de autenticación interactiva. | Llame a AcquireTokenInteractively(). |

## <a name="code-example"></a>Ejemplo de código

```csharp
AuthenticationResult res;
try
{
 res = await application.AcquireTokenSilent(scopes, account)
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex) when (ex.ErrorCode == MsalError.InvalidGrantError)
{
 switch (ex.Classification)
 {
  case UiRequiredExceptionClassification.None:
   break;
  case UiRequiredExceptionClassification.MessageOnly:
  // You might want to call AcquireTokenInteractive(). Azure AD will show a message
  // that explains the condition. AcquireTokenInteractively() will return UserCanceled error
  // after the user reads the message and closes the window. The calling application may choose
  // to hide features or data that result in message_only if the user is unlikely to benefit 
  // from the message
  try
  {
   res = await application.AcquireTokenInteractive(scopes)
                          .ExecuteAsync();
  }
  catch (MsalClientException ex2) when (ex2.ErrorCode == MsalError.AuthenticationCanceledError)
  {
   // Do nothing. The user has seen the message
  }
  break;

  case UiRequiredExceptionClassification.BasicAction:
  // Call AcquireTokenInteractive() so that the user can, for instance accept terms
  // and conditions

  case UiRequiredExceptionClassification.AdditionalAction:
  // You might want to call AcquireTokenInteractive() to show a message that explains the remedial action. 
  // The calling application may choose to hide flows that require additional_action if the user 
  // is unlikely to complete the remedial action (even if this means a degraded experience)

  case UiRequiredExceptionClassification.ConsentRequired:
  // Call AcquireTokenInteractive() for user to give consent.
  
  case UiRequiredExceptionClassification.UserPasswordExpired:
  // Call AcquireTokenInteractive() so that user can reset their password
  
  case UiRequiredExceptionClassification.PromptNeverFailed:
  // You used WithPrompt(Prompt.Never) and this failed
  
  case UiRequiredExceptionClassification.AcquireTokenSilentFailed:
  default:
  // May be resolved by user interaction during the interactive authentication flow.
  res = await application.AcquireTokenInteractive(scopes)
                         .ExecuteAsync(); break;
 }
}
```


## <a name="javascript-errors"></a>Errores de JavaScript

MSAL.js proporciona objetos de error que abstraen y clasifican los distintos tipos de errores comunes. También proporciona la interfaz para tener acceso a detalles específicos de los errores, como los mensajes de error, para controlarlos de manera adecuada.

**Objeto Error**

```javascript
export class AuthError extends Error {
    // This is a short code describing the error
    errorCode: string;
    // This is a descriptive string of the error,
    // and may also contain the mitigation strategy
    errorMessage: string;
    // Name of the error class
    this.name = "AuthError";
}
```

Si extiende la clase de error, tendrá acceso a las siguientes propiedades:
* **AuthError.message:**  igual que errorMessage.
* **AuthError.stack:** Seguimiento de la pila de los errores que se produjeron. Permite el seguimiento hasta dar con el punto de origen del error.

**Tipos de errores**

Están disponibles los siguientes tipos de errores:

* *AuthError:* Clase de error básica para la biblioteca MSAL.js. También se usa para los errores inesperados.

* *ClientAuthError:* clase de error que indica un problema con la autenticación de cliente. La mayoría de los errores que proceden de la biblioteca serán del tipo ClientAuthErrors. Puede tratarse de errores como llamar a un método de inicio de sesión cuando el inicio de sesión ya está en curso, usuarios que cancelan un inicio de sesión, etc. 

* *ClientConfigurationError:* clase de error que extiende el tipo de error ClientAuthError que se produce antes de que se realicen las solicitudes si los parámetros de configuración de un usuario determinado faltan o tienen un formato incorrecto.

* *ServerError:* Clase de error para representar las cadenas de error que envía el servidor de autenticación. Puede tratarse de errores como formatos o parámetros de solicitud no válidos o cualquier otro error que impida al servidor autenticar o autorizar al usuario.

* *InteractionRequiredAuthError:* clase de error que extiende ServerError para representar errores de servidor que requieren una llamada interactiva. `acquireTokenSilent` genera este error si se solicita al usuario que interactúe con el servidor para proporcionar credenciales o consentimiento para la autenticación o autorización. Entre los códigos de error se incluyen estos: "interaction_required", "login_required", "consent_required".

Para el control de errores en flujos de autenticación con métodos de redireccionamiento (`loginRedirect`, `acquireTokenRedirect`), deberá registrar la devolución de llamada a la que se llama correctamente o con error después del redireccionamiento mediante el método `handleRedirectCallback()` de la siguiente manera:

```javascript
function authCallback(error, response) {
    //handle redirect response
}


var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);

myMSALObj.acquireTokenRedirect(request);
```

Los métodos de la experiencia emergente (`loginPopup`, `acquireTokenPopup`) devuelven promesas, de forma que puede usar el patrón de promesa (.then y .catch) para controlarlas de la siguiente manera:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="interaction-required-errors"></a>Errores al requerir una interacción

Se devuelve un error cuando intenta usar un método no interactivo para adquirir un token (por ejemplo, `acquireTokenSilent`) y MSAL no pudo hacerlo de manera silenciosa.

Los posibles motivos son:

* el usuario debe iniciar sesión.
* el usuario debe dar su consentimiento.
* el usuario debe pasar por una experiencia de autenticación multifactor.

La solución consiste en llamar a un método interactivo como `acquireTokenPopup` o `acquireTokenRedirect`:

```javascript
// Request for Access Token
myMSALObj.acquireTokenSilent(request).then(function (response) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    // due to consent or interaction required
    if (error.errorCode === "consent_required"
    || error.errorCode === "interaction_required"
    || error.errorCode === "login_required") {
        myMSALObj.acquireTokenPopup(request).then(
            function (response) {
                // call API
            }).catch(function (error) {
                console.log(error);
            });
    }
});
```

## <a name="conditional-access-and-claims-challenges"></a>Acceso condicional y desafíos de notificaciones
Al obtener los tokens de forma automática, la aplicación puede recibir errores si una API a la que está intentando acceder requiere un [desafío de notificaciones de acceso condicional](conditional-access-dev-guide.md) como, por ejemplo, una directiva de autenticación multifactor.

El patrón para controlar este error consiste en adquirir interactivamente un token mediante MSAL. La adquisición interactiva de un token avisa al usuario y le da la oportunidad de cumplir con la directiva de acceso condicional requerida.

En algunos casos, al llamar a una API que requiere acceso condicional, puede recibir un desafío de notificaciones en el error de la API. Por ejemplo, si la directiva de acceso condicional consiste en tener un dispositivo administrado (Intune) el error puede ser parecido a este: [AADSTS53000: Your device is required to be managed to access this resource](reference-aadsts-error-codes.md) (Se requiere que su dispositivo sea administrado para acceder a este recurso) o algo parecido. En este caso, puede pasar las notificaciones de la llamada de adquisición del token de manera que se le pida al usuario que cumpla con la directiva adecuada.

### <a name="net"></a>.NET
Cuando llama a una API que requiere acceso condicional desde MSAL.NET, la aplicación debe controlar las excepciones del desafío de notificaciones. Esto aparecerá como [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) donde la propiedad [Claims](/dotnet/api/microsoft.identity.client.msalserviceexception.claims?view=azure-dotnet) no estará vacía.

Para controlar el desafío de notificaciones, deberá usar el método `.WithClaim()` de la clase `PublicClientApplicationBuilder`.

### <a name="javascript"></a>JavaScript
Al obtener los tokens de forma automática (mediante `acquireTokenSilent`) con MSAL.js, la aplicación puede recibir errores si una API a la que está intentando acceder requiere un [desafío de notificaciones de acceso condicional](conditional-access-dev-guide.md) como, por ejemplo, una directiva de autenticación multifactor.

El patrón para controlar este error consiste en realizar una llamada interactiva para adquirir el token en MSAL.js como, por ejemplo, `acquireTokenPopup` o `acquireTokenRedirect` como se puede ver en el ejemplo siguiente:

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function (accessTokenResponse) {
    // call API
}).catch( function (error) {
    if (error instanceof InteractionRequiredAuthError) {
        // Extract claims from error message
        accessTokenRequest.claimsRequest = extractClaims(error.errorMessage);
        // call acquireTokenPopup in case of InteractionRequiredAuthError failure
        myMSALObj.acquireTokenPopup(accessTokenRequest).then(function (accessTokenResponse) {
            // call API
        }).catch(function (error) {
            console.log(error);
        });
    }
});
```

La adquisición interactiva del token avisa al usuario y le da la oportunidad de cumplir con la directiva de acceso condicional requerida.

Al llamar a una API que requiere acceso condicional, puede recibir un desafío de notificaciones en el error de la API. En este caso, puede pasar las notificaciones devueltas en el error al campo `claimsRequest` de la clase `AuthenticationParameters.ts` para satisfacer la directiva adecuada. 

Consulte cómo [solicitar notificaciones adicionales](active-directory-optional-claims.md) para más información.

## <a name="retrying-after-errors-and-exceptions"></a>Nuevos intentos después de errores y excepciones

Se espera que implemente sus propias directivas de reintentos al llamar a MSAL. MSAL realiza llamadas HTTP al servicio AAD y pueden producirse errores ocasionales; por ejemplo, la red puede dejar de funcionar o el servidor puede estar sobrecargado.  

### <a name="http-error-codes-500-600"></a>Códigos de error HTTP 500-600

MSAL.NET implementa un único mecanismo de un solo reintento para aquellos errores con códigos de error 500-600.

### <a name="http-429"></a>HTTP 429

Cuando el servidor de token de servicio (STS) se sobrecarga con demasiadas solicitudes, devuelve un error HTTP 429 con una sugerencia sobre cuándo puede intentarlo de nuevo en términos de tiempo. El error se puede leer en el campo de respuesta `Retry-After`.

#### <a name="net"></a>.NET

La excepción [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) muestra `System.Net.Http.Headers.HttpResponseHeaders` como una propiedad `namedHeaders`. Por lo tanto, puede aprovechar la información adicional del código de error para mejorar la confiabilidad de las aplicaciones. En el caso que acabamos de describir, puede usar `RetryAfterproperty` (del tipo `RetryConditionHeaderValue`) y calcular cuándo se debe volver a intentar.

Este es un ejemplo para una aplicación de demonio (que usa el flujo de credenciales de cliente), pero se puede adaptar a cualquiera de los métodos de adquisición de un token.

```csharp
do
{
 retry = false;
 TimeSpan? delay;
 try
 {
  result = await publicClientApplication.AcquireTokenForClient(scopes, account)
                                        .ExecuteAsync();
 }
 catch (MsalServiceException serviceException)
 {
  if (ex.ErrorCode == "temporarily_unavailable")
  {
   RetryConditionHeaderValue retryAfter = serviceException.Headers.RetryAfter;
   if (retryAfter.Delta.HasValue)
   {
    delay = retryAfter.Delta;
   }
   else if (retryAfter.Date.HasValue)
   {
    delay = retryAfter.Date.Value.Offset;
   }
  }
 }

    …
 if (delay.HasValue)
 {
  Thread.Sleep((int)delay.Value.TotalMilliseconds); // sleep or other
  retry = true;
 }
} while (retry);
```
