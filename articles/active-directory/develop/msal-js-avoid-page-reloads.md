---
title: Evitar las recargas de página (biblioteca de autenticación de Microsoft para JavaScript) | Azure
description: Aprenda a evitar las recargas de página cuando la adquisición y renovación de tokens en modo silencioso utilizando la biblioteca de autenticación de Microsoft para JavaScript (MSAL.js).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/29/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 162811221e6dde89ad11f358b2ec8f32f3c82522
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420474"
---
# <a name="avoid-page-reloads-when-acquiring-and-renewing-tokens-silently-using-msaljs"></a>Evitar las recargas de página cuando la adquisición y renovación de tokens en modo silencioso utilizando MSAL.js
Biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) usa ocultados `iframe` elementos para adquirir y renovar los tokens silenciosamente en segundo plano. Azure AD devuelve el token de vuelta a los redirect_uri registrados especificado en la solicitud de token (de forma predeterminada es la página de aplicación raíz). Puesto que la respuesta es 302, da como resultado el HTML correspondiente a la `redirect_uri` carga en el `iframe`. Normalmente, la aplicación `redirect_uri` es la página raíz y esto provoca que se vuelva a cargar.

En otros casos, si navegar a la página raíz de la aplicación requiere autenticación, se podría producir anidada a `iframe` elementos o `X-Frame-Options: deny` error.

Dado que MSAL.js no se puede descartar el 302 emitido por Azure AD y es necesaria para procesar el token devuelto, no puede impedir que el `redirect_uri` de carga en el `iframe`.

Para evitar volver a cargar de nuevo toda la aplicación u otros errores debidos a esto, siga estas soluciones.

## <a name="specify-different-html-for-the-iframe"></a>Especificar diferentes HTML de iframe

Establecer el `redirect_uri` propiedad de configuración a una página simple, que no requiere autenticación. Tiene que asegurarse de que coincide con el `redirect_uri` registrado en Azure portal. Esto no afectará experiencia de inicio de sesión del usuario como MSAL guarda la página de inicio cuando comienza el proceso de inicio de sesión de usuario y redirige a la ubicación exacta de una vez completado el inicio de sesión.

## <a name="initialization-in-your-main-app-file"></a>Inicialización en el archivo de aplicación principal

Si la aplicación está estructurada de forma que hay un archivo Javascript central que define la inicialización de la aplicación, enrutamiento y otras cosas, puede cargar condicionalmente los módulos de aplicación en función de si la aplicación se está cargando en un `iframe` o no. Por ejemplo:

En AngularJS: app.js

```javascript
// Check that the window is an iframe and not popup
if (window !== window.parent && !window.opener) {
angular.module('todoApp', ['ui.router', 'MsalAngular'])
    .config(['$httpProvider', 'msalAuthenticationServiceProvider','$locationProvider', function ($httpProvider, msalProvider,$locationProvider) {
        msalProvider.init(
            // msal configuration
        );

        $locationProvider.html5Mode(false).hashPrefix('');
    }]);
}
else {
    angular.module('todoApp', ['ui.router', 'MsalAngular'])
        .config(['$stateProvider', '$httpProvider', 'msalAuthenticationServiceProvider', '$locationProvider', function ($stateProvider, $httpProvider, msalProvider, $locationProvider) {
            $stateProvider.state("Home", {
                url: '/Home',
                controller: "homeCtrl",
                templateUrl: "/App/Views/Home.html",
            }).state("TodoList", {
                url: '/TodoList',
                controller: "todoListCtrl",
                templateUrl: "/App/Views/TodoList.html",
                requireLogin: true
            })

            $locationProvider.html5Mode(false).hashPrefix('');

            msalProvider.init(
                // msal configuration
            );
        }]);
}
```

En Angular: app.module.ts

```javascript
// Imports...
@NgModule({
  declarations: [
    AppComponent,
    MsalComponent,
    MainMenuComponent,
    AccountMenuComponent,
    OsNavComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production }),
    MsalModule.forRoot(environment.MsalConfig),
    SuiModule,
    PagesModule
  ],
  providers: [
    HttpServiceHelper,
    {provide: HTTP_INTERCEPTORS, useClass: MsalInterceptor, multi: true},
    AuthService
  ],
  entryComponents: [
    AppComponent,
    MsalComponent
  ]
})
export class AppModule {
  constructor() {
    console.log('APP Module Constructor!');
  }

  ngDoBootstrap(ref: ApplicationRef) {
    if (window !== window.parent && !window.opener)
    {
      console.log("Bootstrap: MSAL");
      ref.bootstrap(MsalComponent);
    }
    else
    {
    //this.router.resetConfig(RouterModule);
      console.log("Bootstrap: App");
      ref.bootstrap(AppComponent);
    }
  }
}
```

MsalComponent:

```javascript
import { Component} from '@angular/core';
import { MsalService } from '@azure/msal-angular';

// This component is used only to avoid Angular reload
// when doing acquireTokenSilent()

@Component({
  selector: 'app-root',
  template: '',
})
export class MsalComponent {
  constructor(private Msal: MsalService) {
  }
}
```

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre [compilar una aplicación de página única (SPA)](scenario-spa-overview.md) mediante MSAL.js.