---
title: 'Aplicación de página única (inicio de sesión): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación de página única (inicio de sesión)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc9c46ae28960387e6f8efc1ade20afa1c77ef55
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138802"
---
# <a name="single-page-application---sign-in"></a>Aplicación de página única - inicio de sesión

Obtenga información sobre cómo agregar el inicio de sesión al código de la aplicación de página única.

Antes de obtener tokens para tener acceso a las API en la aplicación, necesita un contexto de usuario autenticado. Puede iniciar sesión a los usuarios a la aplicación en MSAL.js de dos maneras:

* [Inicie sesión con una ventana emergente](#sign-in-with-a-pop-up-window) mediante `loginPopup` (método)
* [Inicie sesión con redirección](#sign-in-with-redirect) mediante `loginRedirect` (método)

Opcionalmente, también puede pasar los ámbitos de las API para los que necesita el usuario dé su consentimiento en el momento de inicio de sesión.

> [!NOTE]
> Si la aplicación ya tiene acceso a un contexto de usuario autenticado o el identificador de token, puede omitir el paso de inicio de sesión y adquirir directamente los tokens. Para obtener más información, consulte [sso sin inicio de sesión msal.js](msal-js-sso.md#sso-without-msaljs-login).

## <a name="choosing-between-a-pop-up-or-redirect-experience"></a>Elegir entre una experiencia de elemento emergente o redireccionamiento

No se puede usar una combinación de métodos de la ventana emergente y el redireccionamiento en la aplicación. La elección entre una experiencia de elemento emergente o redireccionamiento depende de su flujo de la aplicación.

* Si no desea que el usuario abandona la página principal de la aplicación durante la autenticación, se recomienda utilizar los métodos emergentes. Dado que la redirección de autenticación se realiza en una ventana emergente, se conserva el estado de la aplicación principal.

* Hay algunos casos donde es posible que deba usar los métodos de redirección. Si los usuarios de la aplicación tienen restricciones del explorador o las directivas de donde se deshabilitan las ventanas de elementos emergentes, puede usar los métodos de redirección. Utilice los métodos de redirección con el Explorador de Internet Explorer ya que hay ciertos [problemas conocidos con Internet Explorer](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) al manipular las ventanas emergentes.

## <a name="sign-in-with-a-pop-up-window"></a>Inicie sesión con una ventana emergente

### <a name="javascript"></a>JavaScript

```javascript
const loginRequest = {
    scopes: ["user.read", "user.write"]
}

userAgentApplication.loginPopup(loginRequest).then(function (loginResponse) {
    //login success
    let idToken = loginResponse.idToken;
}).catch(function (error) {
    //login failure
    console.log(error);
});
```

### <a name="angular"></a>Angular

El contenedor MSAL Angular permite proteger rutas específicas en la aplicación agregando simplemente la `MsalGuard` a la definición de ruta. Esta protección invocará al método para iniciar sesión cuando se tiene acceso a esa ruta.

```javascript
// In app.routes.ts
{ path: 'product', component: ProductComponent, canActivate : [MsalGuard],
    children: [
      { path: 'detail/:id', component: ProductDetailComponent  }
    ]
   },
  { path: 'myProfile' ,component: MsGraphComponent, canActivate : [MsalGuard] },
```

Para una experiencia de la ventana emergente, habilitar la `popUp` opción de configuración. También puede pasar los ámbitos que requieren el consentimiento como sigue:

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                popUp: true,
                consentScopes: ["user.read", "user.write"]
            })]
         })
```

## <a name="sign-in-with-redirect"></a>Iniciar sesión con redirección

### <a name="javascript"></a>JavaScript

Los métodos de redirección no devuelva una promesa debido a la navegación fuera de la aplicación principal. Para procesar y obtener acceso a los tokens devueltos, deberá registrar devoluciones de llamada success y error antes de llamar a los métodos de redirección.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const loginRequest = {
    scopes: ["user.read", "user.write"]
}

userAgentApplication.loginRedirect(loginRequest);
```

### <a name="angular"></a>Angular

Este código es el mismo que se ha descrito anteriormente en el inicio de sesión con una sección de la ventana emergente. El flujo predeterminado es de redirección.

> [!NOTE]
> El token de identificador no contiene los ámbitos con consentimiento y solo representa al usuario autenticado. Se devuelven los ámbitos con consentimiento en el token de acceso que va a adquirir en el paso siguiente.

## <a name="sign-out"></a>Cerrar sesión

La biblioteca MSAL proporciona un `logout` método que se borrará la memoria caché en el almacenamiento del explorador y envía una solicitud de cierre de sesión a Azure AD. Después de cierre de sesión, redirige a la página de inicio de la aplicación de forma predeterminada.

Puede configurar el URI al que debe redirigir después del inicio de sesión fuera estableciendo el `postLogoutRedirectUri`. Esta URI también se debe registrar como Logout URI en el registro de la aplicación.

### <a name="javascript"></a>JavaScript

```javascript
const config = {

    auth: {
        clientID: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const userAgentApplication = new UserAgentApplication(config);
userAgentApplication.logout();

```

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                postLogoutRedirectUri: "your_app_logout_redirect_uri"
            })]
         })

// In app.component.ts
this.authService.logout();
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquirir un token para la aplicación](scenario-spa-acquire-token.md)
