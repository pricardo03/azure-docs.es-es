---
title: Compilación de una aplicación de página única de AngularJS de Azure AD para el inicio y cierre de sesión | Microsoft Docs
description: Aprenda compilar una aplicación de página única de AngularJS que integre Azure AD para el inicio y cierre de sesión y llame a las API protegidas con Azure AD mediante OAuth.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.devlang: javascript
ms.topic: quickstart
ms.date: 10/25/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1bc8c05a2f5b85dbd1b24dbf3a259b75cfdc2f77
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76704075"
---
# <a name="quickstart-build-an-angularjs-single-page-app-for-sign-in-and-sign-out-with-azure-active-directory"></a>Inicio rápido: Compilación de una aplicación de página única de AngularJS para el inicio y el cierre de sesión con Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

> [!IMPORTANT]
> La [Plataforma de identidad de Microsoft](v2-overview.md) es una evolución de la plataforma para desarrolladores de Azure Active Directory (Azure AD). Permite a los desarrolladores compilar aplicaciones que inicien sesión en todas las identidades de Microsoft, obtener tokens para llamar a las API de Microsoft como Microsoft Graph, o a otras API que los desarrolladores hayan creado.
> Si necesita habilitar los inicios de sesión para las cuentas personales además de hacerlo para las cuentas profesionales y educativas, puede usar el *[punto de conexión de la plataforma de identidad de Microsoft](azure-ad-endpoint-comparison.md)* .
> Esta guía de inicio rápido es para el punto de conexión anterior de Azure AD v1.0. Se recomienda usar el punto de conexión v2.0 para los proyectos nuevos. Para más información, consulte [este tutorial de SPA de JavaScript](tutorial-v2-javascript-spa.md) y [este artículo](active-directory-v2-limitations.md), que explica el *punto de conexión de la plataforma de identidad de Microsoft*.

Con Azure Active Directory (Azure AD), es sencillo y directo agregar llamadas de inicio y cierre de sesión, así como llamadas seguras de API de OAuth a las aplicaciones de una sola página. Permite que la aplicación autentique a los usuarios con sus cuentas de Active Directory de Windows Server, además de consumir cualquier web API que Azure AD ayuda a proteger, como las API de Office 365 o la API de Azure.

Para las aplicaciones de JavaScript que se ejecutan en un explorador, Azure AD proporciona la biblioteca de autenticación de Active Directory (ADAL), o adal.js. La única finalidad de adal.js es facilitar a su aplicación la obtención de tokens de acceso.

En este tutorial de inicio rápido, aprenderá a compilar una aplicación de lista de tareas pendientes de AngularJS con la que podrá:

* Iniciar sesión del usuario en la aplicación con Azure AD como proveedor de identidades.
* Mostrar información sobre el usuario
* Realizar una llamada con seguridad a la API de la lista de tareas pendientes de la aplicación mediante tokens de portador de Azure AD.
* Cerrar la sesión del usuario en la aplicación

Para crear la aplicación de trabajo completa, tendrá que:

1. Registrar la aplicación con Azure AD.
2. Instalar ADAL y configurar la aplicación de una sola página.
3. Usar ADAL para ayudar a proteger las páginas en la aplicación de una sola página.

## <a name="prerequisites"></a>Prerequisites

Para empezar, complete estos requisitos previos:

* Descargue el [esqueleto de la aplicación](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) o el [ejemplo completado](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).
* Necesita un inquilino de Azure AD en el que pueda crear usuarios y registrar una aplicación. Si aún no tiene un inquilino, [descubra cómo conseguir uno](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>Paso 1: Registro de la aplicación DirectorySearcher

Para habilitar la aplicación a fin de autenticar a los usuarios y obtener tokens, primero debe registrarla en el inquilino de Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Si inició sesión en varios directorios, debe asegurarse de que está viendo el directorio correcto. Para ello, en la barra superior, haga clic en su cuenta. En la lista **Directorio** lista, elija el inquilino de Azure AD donde desea registrar la aplicación.
1. Haga clic en **Todos los servicios** en el panel izquierdo y seleccione **Azure Active Directory**.
1. Haga clic en **Registros de aplicaciones** y luego **Nuevo registro**.
1. Cuando se abra la página **Registrar una aplicación**, escriba el nombre de su aplicación.
1. En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
1. Seleccione la plataforma **Web** en la sección **URI de redireccionamiento** y establezca el valor en `https://localhost:44326/` (la ubicación a la que Azure AD devolverá tokens).
1. Cuando termine, seleccione **Registrar**. En la página de **Información general** de la aplicación, anote el valor en **Id. de aplicación (cliente)** .
1. Adal.js utiliza el flujo implícito de OAuth para comunicarse con Azure AD. Debe habilitar el flujo implícito para la aplicación mediante estos pasos. En el panel de navegación izquierdo de la aplicación registrada, seleccione **Autenticación**.
1. En **Configuración avanzada**, vaya a **Concesión implícita** y habilite las casillas **Tokens de id.** y **Tokens de acceso**. Los tokens de identificador y los tokens de acceso son necesarios, ya que esta aplicación debe iniciar la sesión de los usuarios y llamar a una API.
1. Seleccione **Guardar**.
1. Conceda permisos para la aplicación en todo el inquilino. Vaya a **Permisos de API** y seleccione el botón **Conceder consentimiento de administrador** en **Conceder consentimiento**.
1. Seleccione **Sí** para confirmar la acción.

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a>Paso 2: Instalación de ADAL y configuración de la aplicación de una sola página

Ahora que tiene una aplicación en Azure AD, puede instalar adal.js y escribir el código relacionado con la identidad.

### <a name="configure-the-javascript-client"></a>Configuración del cliente de JavaScript

Comience agregando adal.js al proyecto TodoSPA mediante la Consola del Administrador de paquetes:

1. Descargue [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) y agréguelo al directorio del proyecto `App/Scripts/`.
2. Descargue [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) y agréguelo al directorio del proyecto `App/Scripts/`.
3. Cargue cada script antes del fin de `</body>` in `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a>Configuración del servidor back-end

Para que la API de tareas pendientes del back-end de la aplicación de una sola página acepte tokens del explorador, el back-end necesita información de configuración acerca del registro de la aplicación. En el proyecto TodoSPA, abra `web.config`. Reemplace los valores de los elementos de la sección `<appSettings>` para que reflejen los valores usados en Azure Portal. El código hará referencia a estos valores cada vez que use ADAL.

   * `ida:Tenant` es el dominio del inquilino de Azure AD, por ejemplo, contoso.onmicrosoft.com.
   * `ida:Audience` es el identificador de cliente de la aplicación que copió del portal.

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a>Paso 3: Uso de ADAL para ayudar a proteger las páginas en la aplicación de una sola página

Adal.js se integra con la ruta de AngularJS y los proveedores HTTP, por lo que puede ayudar a proteger vistas individuales en la aplicación de una sola página.

1. En `App/Scripts/app.js`, importe el módulo adal.js:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Inicialice `adalProvider` con los valores de configuración del registro de su aplicación, también en `App/Scripts/app.js`:

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. Ayude a proteger la vista `TodoList` en la aplicación utilizando solamente una línea de código: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Resumen

Ahora tiene una aplicación de una sola página segura con capacidad para iniciar la sesión de los usuarios y emitir solicitudes protegidas de token de portador a su API de back-end. Cuando un usuario hace clic en el vínculo **TodoList**, adal.js le redireccionará automáticamente a Azure AD para iniciar sesión si es necesario. Además, adal.js adjuntará automáticamente un token de acceso a cualquier solicitud de Ajax que se envíe al servidor back-end de la aplicación.

Los pasos anteriores son los mínimos necesarios para compilar una aplicación de una sola página mediante adal.js. Pero algunas otras características son útiles en una aplicación de una sola página:

* Para emitir explícitamente solicitudes de inicio y cierre de sesión, puede definir funciones en sus controladores que invocan adal.js. En `App/Scripts/homeCtrl.js`:

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* Puede que desee presentar información de usuario en la interfaz de usuario de la aplicación. El servicio ADAL ya se ha agregado al controlador `userDataCtrl`, por lo que puede tener acceso al objeto `userInfo` en la vista asociada, `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Hay muchos casos en los que deseará saber si el usuario ha iniciado sesión o no. También puede utilizar el objeto `userInfo` para recopilar esta información. Por ejemplo, en `index.html` puede mostrar el botón **Iniciar sesión** o **Cerrar sesión** en función del estado de autenticación:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

La aplicación de una sola página integrada con Azure AD puede autenticar a los usuarios, efectuar llamadas seguras a back-end mediante OAuth 2.0 y obtener información básica sobre el usuario. Si todavía no lo ha hecho, ahora es el momento de completar el inquilino con algunos usuarios. Ejecute la aplicación de una sola página de la lista de tareas pendientes e inicie sesión con uno de esos usuarios. Agregue tareas a la lista de tareas pendientes de los usuarios, cierre la sesión y vuelva a iniciarla.

Adal.js facilita la incorporación de características comunes de identidad en la aplicación. Se encarga de todo el trabajo duro: administración de la memoria caché, compatibilidad con el protocolo OAuth, presentación del usuario con una interfaz de usuario de inicio de sesión, actualización de los tokens caducados, etc.

Como referencia, en [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip) está disponible el ejemplo finalizado (sin sus valores de configuración).

## <a name="next-steps"></a>Pasos siguientes

Ahora puede pasar a otros escenarios.

> [!div class="nextstepaction"]
> [Llamar a una API web CORS desde una aplicación de página única](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)
