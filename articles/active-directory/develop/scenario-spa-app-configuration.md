---
title: Aplicación de página única (configuración del código de la aplicación) - Plataforma de identidad de Microsoft
description: Obtenga información sobre cómo crear una aplicación de página única (configuración del código de la aplicación)
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
ms.openlocfilehash: b71454fc553a0f81c26426a6a9588f15d5311e38
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65406436"
---
# <a name="single-page-application---code-configuration"></a>Aplicación de página única: configuración del código

Obtenga información sobre cómo configurar el código de la aplicación de página única (SPA).

## <a name="msal-libraries-supporting-implicit-flow"></a>Bibliotecas MSAL que admiten flujo implícito

La plataforma de identidad de Microsoft proporciona una biblioteca MSAL.js para admitir el flujo implícito mediante las prácticas seguras recomendadas del sector.  

Las bibliotecas que admiten flujo implícito son las siguientes:

| Biblioteca MSAL | DESCRIPCIÓN |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Biblioteca de JavaScript sin formato para usar en cualquier aplicación web cliente compilada con marcos de JavaScript o SPA, como Angular, Vue.js, React.js, etc. |
| ![MSAL Angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Contenedor de la biblioteca básica de MSAL.js para simplificar el uso en aplicaciones de página única creadas con el marco Angular. Esta biblioteca está en versión preliminar y tiene [problemas conocidos](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) con determinadas versiones de Angular y ciertos exploradores. |

## <a name="application-code-configuration"></a>Configuración del código de la aplicación

En la biblioteca MSAL, la información de registro de aplicación se pasa como configuración durante la inicialización de la biblioteca.

### <a name="javascript"></a>JavaScript

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```
Para obtener más detalles sobre las opciones configurables disponibles, consulte [Inicializar la aplicación con MSAL.js](msal-js-initializing-client-applications.md).

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientId: 'your_app_id'
            })]
         })

  export class AppModule { }
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio y cierre de sesión](scenario-spa-sign-in.md)
