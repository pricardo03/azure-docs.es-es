---
title: 'Aplicación de página única (adquirir un token para llamar a una API): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación de página única (adquirir un token para llamar a una API)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4c842db8a0874d3619e0dc59b90aa12226cb984
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138818"
---
# <a name="single-page-application---acquire-a-token-to-call-an-api"></a>Aplicación de página única: adquirir un token para llamar a una API

El patrón para la adquisición de tokens de API con MSAL.js consiste en intentar realizar primero una solicitud de token silenciosa utilizando el `acquireTokenSilent` método. Cuando se llama a este método, la biblioteca comprueba primero la caché en el almacenamiento del explorador para ver si existe un token válido y la devuelve. Cuando no hay ningún token válido en la memoria caché, envía una solicitud de token silenciosa a Azure Active Directory (Azure AD) de un iframe oculto. Este método también permite que la biblioteca renovar los tokens. Para obtener más información acerca de la sesión de inicio de sesión única y los valores de duración del token de Azure AD, consulte [duración del token](active-directory-configurable-token-lifetimes.md).

Las solicitudes de token silenciosas con Azure AD pueden producir un error por algún motivo, como una sesión de AD de Azure expirada o un cambio de contraseña. En ese caso, puede invocar uno de los métodos interactivos (que se le pedirá al usuario) para adquirir tokens.

* [Adquirir el token con una ventana emergente](#acquire-token-with-a-pop-up-window) mediante `acquireTokenPopup`
* [Adquirir el token con redirección](#acquire-token-with-redirect) mediante `acquireTokenRedirect`

**Elegir entre una experiencia de elemento emergente o redireccionamiento**

 No se puede usar una combinación de métodos de la ventana emergente y el redireccionamiento en la aplicación. La elección entre una experiencia de elemento emergente o redireccionamiento depende de su flujo de la aplicación.

* Si no desea que el usuario abandona la página principal de la aplicación durante la autenticación, se recomienda utilizar los métodos emergentes. Dado que la redirección de autenticación se realiza en una ventana emergente, se conserva el estado de la aplicación principal.

* Hay algunos casos donde es posible que deba usar los métodos de redirección. Si los usuarios de la aplicación tienen restricciones del explorador o las directivas de donde se deshabilitan las ventanas de elementos emergentes, puede usar los métodos de redirección. También se recomienda usar los métodos de redirección con el Explorador de Internet Explorer ya que hay ciertos [problemas conocidos con Internet Explorer](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) al manipular las ventanas emergentes.

Puede establecer los ámbitos de API que desea que el token de acceso para incluir al crear la solicitud de token de acceso. Tenga en cuenta que todos los ámbitos solicitados no se pueden otorgar en el token de acceso y que depende el consentimiento del usuario.

## <a name="acquire-token-with-a-pop-up-window"></a>Adquirir el token con una ventana emergente

### <a name="javascript"></a>JavaScript

El patrón anterior utilizando los métodos para obtener una experiencia emergente:

```javascript
const accessTokenRequest = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, send an interactive request.
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // Acquire token interactive success
        }).catch(function(error) {
            // Acquire token interactive failure
            console.log(error);
        });
    }
    console.log(error);
});
```

### <a name="angular"></a>Angular

El contenedor MSAL Angular ofrece la comodidad de agregar el interceptor HTTP `MsalInterceptor` que adquirir tokens de acceso de forma silenciosa y adjuntarlos a las solicitudes HTTP a las API automáticamente.

Puede especificar los ámbitos de API en el `protectedResourceMap` opción de configuración que solicitará la MsalInterceptor cuando la adquisición de tokens automáticamente.

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                protectedResourceMap: {"https://graph.microsoft.com/v1.0/me", ["user.read", "mail.send"]}
            })]
         })

providers: [ ProductService, {
        provide: HTTP_INTERCEPTORS,
        useClass: MsalInterceptor,
        multi: true
    }
   ],
```

Para correctas e incorrectas de la adquisición del token silenciosa, MSAL Angular proporciona puede suscribirse a las devoluciones de llamada. También es importante recordar que cancelar la suscripción.

```javascript
// In app.component.ts
 ngOnInit() {
    this.subscription=  this.broadcastService.subscribe("msal:acquireTokenFailure", (payload) => {
    });
}

ngOnDestroy() {
   this.broadcastService.getMSALSubject().next(1);
   if(this.subscription) {
     this.subscription.unsubscribe();
   }
 }
```

Como alternativa, puede adquirir explícitamente tokens mediante los métodos de token de adquisición, como se describe en la biblioteca básica de MSAL.js.

## <a name="acquire-token-with-redirect"></a>Adquirir el token con redirección

### <a name="javascript"></a>JavaScript

El patrón es como se ha descrito anteriormente, pero se muestra con un método de redirección para adquirir el token de forma interactiva. Tenga en cuenta que necesitará registrar la devolución de llamada de redirección, como se mencionó anteriormente.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const accessTokenRequest: AuthenticationParameters = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, send an interactive request.
    console.log(error);
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenRedirect(accessTokenRequest);
    }
});
```

### <a name="angular"></a>Angular

Este es el mismo que se ha descrito anteriormente.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Una llamada a una API Web](scenario-spa-call-api.md)
