---
title: 'Aplicación de página única (adquirir un token para llamar a una API): plataforma de identidad de Microsoft'
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65138818"
---
# <a name="single-page-application---acquire-a-token-to-call-an-api"></a>Aplicación de página única: adquirir un token para llamar a una API

El patrón para la adquisición de tokens de API con MSAL.js consiste en intentar realizar primero una solicitud de token silenciosa utilizando el método `acquireTokenSilent`. Cuando se llama a este método, la biblioteca comprueba primero la caché en el almacenamiento del explorador para ver si existe un token válido y, a continuación, la devuelve. Cuando no hay ningún token válido en la memoria caché, envía una solicitud de token silenciosa a Azure Active Directory (Azure AD) desde un iframe oculto. Este método también permite que la biblioteca renueve los tokens. Para obtener más información sobre los valores de inicio de sesión único y la duración del token en Azure AD, consule la [vigencia de los token](active-directory-configurable-token-lifetimes.md).

Las solicitudes de token silenciosas a Azure AD pueden producir un error por algún motivo, como una sesión de Azure AD que haya expirado o un cambio de contraseña. En ese caso, puede invocar uno de los métodos interactivos (que se le pedirán al usuario) para adquirir tokens.

* [Adquirir tokens con una ventana emergente](#acquire-token-with-a-pop-up-window) mediante `acquireTokenPopup`
* [Adquirir tokens con redirección](#acquire-token-with-redirect) mediante `acquireTokenRedirect`

**Elegir entre una experiencia de elemento emergente o de redireccionamiento**

 No se puede usar una combinación de métodos de elemento emergente y redireccionamiento en la aplicación. La elección entre una experiencia de elemento emergente o redireccionamiento depende de su flujo de la aplicación.

* Si no quiere que el usuario abandone la página principal de la aplicación durante la autenticación, se recomienda usar los métodos emergentes. Dado que la redirección de la autenticación se realiza en una ventana emergente, se conserva el estado de la aplicación principal.

* Hay algunos casos donde es posible que deba usar los métodos de redirección. Si los usuarios de la aplicación tienen restricciones del explorador o directivas donde se deshabilitan las ventanas de elementos emergentes, puede usar los métodos de redirección. También es recomendable que use los métodos de redirección con el explorador de Internet Explorer, ya que hay ciertos [problemas conocidos con Internet Explorer](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) al manipular las ventana emergentes.

Puede establecer los ámbitos de API que quiere que incluya el token de acceso, al crear la solicitud de token de acceso. Tenga en cuenta que no todos los ámbitos solicitados se pueden otorgar en el token de acceso y que dependen del consentimiento del usuario.

## <a name="acquire-token-with-a-pop-up-window"></a>Adquirir tokens con una ventana emergente

### <a name="javascript"></a>JavaScript

El patrón anterior usa los métodos de una experiencia emergente:

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

El contenedor de MSAL Angular le resultará cómodo para agregar el interceptor HTTP `MsalInterceptor`, el cual adquirirá automáticamente los tokens de acceso en silencio y los adjuntará a las solicitudes HTTP de las API.

Puede especificar los ámbitos de las API en la opción de configuración `protectedResourceMap` que MsalInterceptor solicitará cuando adquiera tokens automáticamente.

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

En cuanto al posible éxito o fracaso de la adquisición de tokens silenciosa, MSAL Angular proporciona devoluciones de llamada a las que puede suscribirse. También es importante recordar esta información si quiere cancelar la suscripción.

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

Como alternativa, también puede adquirir los tokens de forma explícita mediante los métodos de token de adquisición, tal como se describe en la biblioteca principal MSAL.js.

## <a name="acquire-token-with-redirect"></a>Adquirir tokens con redirección

### <a name="javascript"></a>JavaScript

El patrón es tal como se describió anteriormente, pero se muestra con un método de redireccionamiento para adquirir el token de manera interactiva. Tenga en cuenta que necesitará registrar la devolución de llamada de redirección, tal como se mencionó anteriormente.

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

Este es el mismo elemento que se ha descrito anteriormente.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamada a una API web](scenario-spa-call-api.md)
