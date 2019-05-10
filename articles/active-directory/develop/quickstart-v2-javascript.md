---
title: Inicio rápido de JavaScript para la Plataforma de identidad de Microsoft | Azure
description: Aprenda la forma en que las aplicaciones de JavaScript pueden llamar a una API que requiera tokens de acceso mediante la Plataforma de identidad de Microsoft.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 605206682cb70d430773cdbf9ff746eabf594103
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190843"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-single-page-application-spa"></a>Inicio rápido: Inicio de sesión y adquisición de un token de acceso por parte de una aplicación de página única de JavaScript (SPA)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido aprenderá a usar un código de ejemplo que muestra cómo una aplicación de página única (SPA) de JavaScript puede iniciar sesión en cuentas personales, profesionales y educativas, y obtener un token de acceso para llamar a Microsoft Graph API o a cualquier API web.

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-javascript/javascriptspa-intro.svg)

## <a name="prerequisites"></a>Requisitos previos

Para este inicio rápido, necesitará la siguiente configuración:
* Para ejecutar el proyecto con un servidor de node.js,
    * Instalar [Node.js](https://nodejs.org/en/download/)
    * Instalación de [Visual Studio Code](https://code.visualstudio.com/download) para editar los archivos de proyecto
* Para ejecutar el proyecto como una solución de Visual Studio, instale [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Registro y descarga de la aplicación de inicio rápido
> Tiene dos opciones para comenzar con la aplicación de inicio rápido:
> * [Rápido] [Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Manual] [Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo
>
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
> 1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
> 1. Vaya al nuevo panel de [Azure Portal: Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs).
> 1. Escriba un nombre para la aplicación y haga clic en **Registrar**.
> 1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación en un solo clic.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo
>
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
>
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
> 1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
> 1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
> 1. Seleccione **Nuevo registro**.
> 1. Cuando se abra la página **Registrar una aplicación**, escriba el nombre de su aplicación.
> 1. En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
> 1. Seleccione la plataforma **Web** en la sección **URI de redirección** y establezca el valor en `http://localhost:30662/`.
> 1. Cuando termine, seleccione **Registrar**.  En la página de **Información general** de la aplicación, anote el valor en **Id. de aplicación (cliente)**.
> 1. Para esta guía, se requiere que habilite el [flujo de concesión implícita](v2-oauth2-implicit-grant-flow.md). En el panel de navegación izquierdo de la aplicación registrada, seleccione **Autenticación**.
> 1. En **Configuración avanzada**, vaya a **Concesión implícita** y habilite las casillas **Tokens de id.** y **Tokens de acceso**. Los tokens de identificador y los tokens de acceso son necesarios, ya que esta aplicación debe iniciar la sesión de los usuarios y llamar a una API.
> 1. Seleccione **Guardar**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> Para que el código de ejemplo de esta guía de inicio rápido funcione, debe agregar un identificador URI de redirección como `http://localhost:30662/` y habilitar **Concesión implícita**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Realizar estos cambios por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-javascript/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto

Puede elegir entre estas opciones la más adecuada para su entorno de desarrollo.
* [Descargue los archivos principales del proyecto](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) para ejecutar un servidor web con Node.js. Para abrir los archivos, use un editor como [Visual Studio Code](https://code.visualstudio.com/).

* (Opcional) [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip) que se va a ejecutar con el servidor IIS. Extraiga el archivo ZIP en una carpeta local (por ejemplo, **C:\Azure-Samples**).



#### <a name="step-3-configure-your-javascript-app"></a>Paso 3: Configuración de la aplicación de JavaScript

> [!div renderon="docs"]
> En la carpeta *JavaScriptSPA*, edite `index.html` y establezca los valores `clientID` y `authority` que hay bajo `msalConfig`.

> [!div class="sxs-lookup" renderon="portal"]
> En la carpeta *JavaScriptSPA*, edite `index.html` y reemplace `msalConfig` por:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

```
> [!div renderon="docs"]
>
> Donde:
> - `Enter_the_Application_Id_here`: es el **identificador de aplicación (cliente)** de la aplicación que registró.
> - `Enter_the_Tenant_Info_Here` se establece como una de las opciones siguientes:
>   - Si la aplicación admite **Accounts in this organizational directory** (Cuentas en este directorio de la organización), reemplace este valor por el **identificador de inquilino** o el **nombre de inquilino** (por ejemplo, contoso.microsoft.com)
>   - Si la aplicación admite **Cuentas en cualquier directorio organizativo**, reemplace este valor por `organizations`
>   - Si la aplicación admite **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio organizativo y cuentas Microsoft personales), reemplace este valor por `common`. Para restringir el soporte técnico a *Personal Microsoft accounts only* (Solo a cuentas Microsoft personales), reemplace este valor por `consumers`.
>
> > [!TIP]
> > Para buscar los valores de **Identificador de aplicación (cliente)**, **Identificador de directorio (inquilino)** y **Tipos de cuenta admitidos**, vaya a la página **Información general** en Azure Portal.
>

#### <a name="step-4-run-the-project"></a>Paso 4: Ejecución del proyecto

* Si usa [Node.js](https://nodejs.org/en/download/):

    1. Ejecute el comando siguiente desde el directorio del proyecto para iniciar el servidor:

        ```batch
        npm install
        node server.js
        ```

    1. Abra un explorador web y vaya a `http://localhost:30662/`.
    1. Haga clic en el botón **Iniciar sesión**, inicie sesión y llame a Microsoft Graph API.


* Si usa [Visual Studio](https://visualstudio.microsoft.com/downloads/), asegúrese de seleccionar la solución del proyecto y presione **F5** para ejecutar el proyecto.

Cuando el explorador haya cargado la aplicación, haga clic en **Iniciar sesión**.  La primera vez que inicie sesión, también se le pedirá que dé su consentimiento para permitir que la aplicación obtenga acceso al perfil e inicie sesión automáticamente. Cuando se haya iniciado sesión correctamente, debería ver la información del perfil de usuario en la página.

## <a name="more-information"></a>Más información

### <a name="msaljs"></a>*msal.js*

MSAL es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por la Plataforma de identidad de Microsoft. El archivo *index.html* de la guía de inicio rápido contiene una referencia a la biblioteca:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.min.js"></script>
```
> [!TIP]
> Puede reemplazar la versión anterior con la versión más reciente en [versiones MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).


Si tiene Node instalado, también puede descargar la versión más reciente a través de npm:

```batch
npm install msal
```

### <a name="msal-initialization"></a>Inicialización de MSAL

El código de la guía de inicio de sesión también muestra cómo inicializar la biblioteca:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

var myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Where  |  |
> |---------|---------|
> |`ClientId`     |El identificador de la aplicación registrada en Azure Portal|
> |`authority`    | (Opcional) Es la dirección URL de autoridad que se describe en la sección de configuración anterior para admitir tipos de cuentas. La autoridad predeterminada es `https://login.microsoftonline.com/common`. |
> |`cacheLocation`  | (Opcional) Esto permite establecer el almacenamiento del explorador para el estado de autenticación. El valor predeterminado es sessionStorage.   |
> |`storeAuthStateInCookie`  | (Opcional) La biblioteca almacenará el estado de la solicitud de autenticación necesario para la validación de los flujos de autenticación en las cookies del explorador. Esto se establece para que los exploradores IE y Edge puedan solucionar determinados [problemas conocidos](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues). |

 Consulte la [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-basics#configuration-options) para más información sobre las opciones de configuración disponibles.

### <a name="sign-in-users"></a>Inicio de sesión de usuarios

El siguiente fragmento de código muestra cómo iniciar la sesión de los usuarios:

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
    //Login Success callback code here
}).catch(function (error) {
    console.log(error);
});
```

> |Where  |  |
> |---------|---------|
> | `scopes`   | (Opcional) Contiene los ámbitos en los que se solicita el consentimiento del usuario en el momento del inicio de sesión. Por ejemplo, `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para API web personalizadas, es decir, `api://<Application ID>/access_as_user`. |

> [!TIP]
> Como alternativa, puede usar el método `loginRedirect` para redirigir la página actual a la página de inicio de sesión en lugar de una ventana emergente.

### <a name="request-tokens"></a>Solicitud de tokens

MSAL tiene tres métodos para adquirir tokens: `acquireTokenRedirect`, `acquireTokenPopup` y `acquireTokenSilent`.

#### <a name="get-a-user-token-silently"></a>Obtención de un token de usuario en silencio

El método `acquireTokenSilent` controla la renovación y las adquisiciones de tokens sin la interacción del usuario. Después de que se ejecuten los métodos `loginRedirect` y `loginPopup` por primera vez, `acquireTokenSilent` es el método que se usa habitualmente para obtener los tokens que se utilizan para acceder a los recursos protegidos en las llamadas posteriores. Las llamadas para solicitar o renovar tokens se realizan en modo silencioso.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> |Where  |  |
> |---------|---------|
> | `scopes`   | Contiene los ámbitos cuya devolución se solicita en el token de acceso de la API. Por ejemplo, `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para API web personalizadas, es decir, `api://<Application ID>/access_as_user`.|

#### <a name="get-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

Hay situaciones en las que es necesario forzar a los usuarios a interactuar con el punto de conexión de la Plataforma de identidad de Microsoft. Por ejemplo: 
* Es posible que los usuarios tengan que volver a escribir las credenciales porque su contraseña ha expirado
* La aplicación solicita acceso a ámbitos de recursos adicionales para los que se necesita el consentimiento del usuario
* Se requiere la autenticación en dos fases

El patrón habitual recomendado para la mayoría de las aplicaciones es llamar primero a `acquireTokenSilent`, después detectar la excepción y, por último, llamar a `acquireTokenPopup` (o a `acquireTokenRedirect`) para iniciar una solicitud interactiva.

Al llamar a `acquireTokenPopup` se abre una ventana emergente para iniciar sesión (o a `acquireTokenRedirect` se redirige a los usuarios al punto de conexión de la Plataforma de identidad de Microsoft) en la que los usuarios deben confirmar las credenciales, dar su consentimiento al recurso requerido o completar la autenticación en dos fases.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```
> [!NOTE]
> Esta guía de inicio rápido usa los métodos `loginRedirect` y `acquireTokenRedirect` cuando el explorador utilizado es Internet Explorer, debido a un [problema conocido](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) por la manera en que Internet Explorer controla las ventanas emergentes.

## <a name="next-steps"></a>Pasos siguientes

Para obtener una guía paso a paso más detallada acerca de cómo compilar la aplicación para esta guía de inicio rápido, pruebe el tutorial de JavaScript siguiente.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Más información acerca de los pasos para crear la aplicación para esta guía de inicio rápido

> [!div class="nextstepaction"]
> [Tutorial para iniciar sesión y llamar a MS Graph](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>Examen del repositorio de MSAL para encontrar documentación, preguntas más frecuentes, problemas, etc.

> [!div class="nextstepaction"]
> [Repositorio de GitHub MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)
