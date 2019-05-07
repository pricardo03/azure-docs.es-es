---
title: Aplicación de página única (configuración de código de la aplicación), plataforma Microsoft identity
description: Aprenda a crear una aplicación de página única (configuración de código de la aplicación)
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
ms.openlocfilehash: 6236f0fa2400113225e04ffd4884cf743d1e250a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138286"
---
# <a name="single-page-application---code-configuration"></a>Aplicación de página única - configuración de código

Obtenga información sobre cómo configurar el código de la aplicación de página única (SPA).

## <a name="msal-libraries-supporting-implicit-flow"></a>Bibliotecas MSAL que admiten flujo implícito

Plataforma de identidad de Microsoft proporciona MSAL.js biblioteca para admitir el flujo implícito con el sector de seguros recomendados.  

Las bibliotecas que admiten flujo implícito son:

| Biblioteca MSAL | DESCRIPCIÓN |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Biblioteca de JavaScript sin formato para su uso en cualquier aplicación de web del lado cliente compilada con marcos de JavaScript o SPA como Angular, Vue.js, React.js, etcetera. |
| ![MSAL Angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Contenedor de la biblioteca básica de MSAL.js para simplificar el uso en aplicaciones de página única creadas con el marco Angular. Esta biblioteca está en versión preliminar y tiene [problemas conocidos](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) con determinadas versiones Angular y exploradores. |

## <a name="application-code-configuration"></a>Configuración de código de aplicación

En la biblioteca MSAL, se pasa la información de registro de aplicación como configuración durante la inicialización de la biblioteca.

### <a name="javascript"></a>JavaScript

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientID: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```
Para obtener más detalles sobre las opciones configurables disponibles, consulte [inicialización de la aplicación con MSAL.js](msal-js-initializing-client-applications.md).

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id'
            })]
         })

  export class AppModule { }
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio de sesión y cierre de sesión](scenario-spa-sign-in.md)
