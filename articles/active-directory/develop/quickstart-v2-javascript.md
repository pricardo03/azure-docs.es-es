---
title: Guía de inicio rápido de Azure AD v2 para JavaScript | Microsoft Docs
description: Aprenda la forma en que las aplicaciones de JavaScript pueden llamar a una API que requiera tokens de acceso mediante un punto de conexión de Azure Active Directory v2.0
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 1b884571707aab71e8a8d124ba68f938e5a63a43
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063751"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Inicio rápido: inicio de sesión y adquisición de un token de acceso por parte de los usuarios desde una aplicación de JavaScript

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido aprenderá a usar un código de ejemplo que muestra cómo una aplicación de página única (SPA) de JavaScript puede iniciar sesión en cuentas personales, profesionales y educativas, obtener un token de acceso para llamar a Microsoft Graph API o a cualquier API web.

![Funcionamiento de la aplicación de ejemplo que se genera en esta guía de inicio rápido](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Registro de una aplicación y descarga de la aplicación de la guía de inicio rápido
>
> ### <a name="step-1-register-your-application"></a>Paso 1: Registro de la aplicación
>
> 1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registrar una aplicación.
> 1. En el cuadro**Application Name** (Nombre de la aplicación) escriba el nombre de su aplicación.
> 1. Asegúrese de que la casilla **Guided Setup** (Instalación guiada) esté desactivada y, luego, seleccione **Create** (Crear).
> 1. Haga clic en **Agregar plataforma** y, después, seleccione **Web**.
> 1. Asegúrese de que la casilla **Permitir flujo implícito** está *seleccionada*.
> 1. En **Direcciones URL de redirección**, agregue `http://localhost:30662/`.
> 1. Haga clic en **Save**(Guardar).

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> Para que el código de ejemplo de esta guía de inicio rápido funcione, debe agregar un identificador URI de redirección como `http://localhost:30662/` y habilitar **Concesión implícita**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Realizar estos cambios por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-javascript/green-check.png) La aplicación está configurada con estos atributos

#### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto

Puede elegir entre estas opciones la más adecuada para su entorno de desarrollo.
* [Descargue los archivos principales del proyecto (para un servidor web, como Node.js)](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
* [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Extraiga el archivo ZIP en una carpeta local (por ejemplo, **C:\Azure-Samples**).

#### <a name="step-3-configure-your-javascript-app"></a>Paso 3: Configuración de la aplicación de JavaScript

> [!div renderon="docs"]
> Edite `index.html` y reemplace `Enter_the_Application_Id_here` en `applicationConfig` por el identificador de la aplicación que acaba de registrar.

> [!div class="sxs-lookup" renderon="portal"]
> Edite `index.html` y reemplace `applicationConfig` por:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
> [!NOTE]
>Si usa [Node.js](https://nodejs.org/en/download/), el archivo *server.js* está configurado para que el servidor empiece a escuchar en el puerto 30662.
> Si usa [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), el archivo *.csproj* del ejemplo de código está configurado para que el servidor empiece a escuchar en el puerto 30662.
>

#### <a name="step-4-run-the-project"></a>Paso 4: Ejecución del proyecto

Si usa Node.js, en una línea de comandos puede ejecutar lo siguiente desde el directorio del proyecto para iniciar el servidor:
 ```batch
 npm install
 node server.js
 ```
Abra un explorador web y vaya a `http://localhost:30662/`. Haga clic en el botón **Iniciar sesión**, inicie sesión y llame a Microsoft Graph API.

Si usa Visual Studio, asegúrese de seleccionar la solución del proyecto y presione **F5** para ejecutar el proyecto.

## <a name="more-information"></a>Más información

### <a name="msaljs"></a>*msal.js*

MSAL es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por Microsoft Azure Active Directory (Azure AD). El archivo *index.html* de la guía de inicio rápido contiene una referencia a la biblioteca:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>
```

Si tiene Node instalado, también puede descargarlo a través de npm:

```batch
npm install msal
```

### <a name="msal-initialization"></a>Inicialización de MSAL

El código de la guía de inicio de sesión también muestra cómo inicializar la biblioteca:

```javascript
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, null, acquireTokenRedirectCallBack, {storeAuthStateInCookie: true, cacheLocation: "localStorage"});
```

> |Where  |  |
> |---------|---------|
> |`ClientId`     |El identificador de la aplicación registrada en Azure Portal|
> |`authority`    |Es la dirección URL de la autoridad. El paso de *null* establece la autoridad de forma predeterminada en `https://login.microsoftonline.com/common`. Si la aplicación tienen un solo inquilino (las cuentas de destino en un solo directorio), establezca este valor en `https://login.microsoftonline.com/<tenant name or ID>`|
> |`tokenReceivedCallback`| El método de devolución de llamada al que se llama después de la autenticación realiza la redirección a la aplicación. Aquí se pasa `acquireTokenRedirectCallBack`. Esto es NULL si se usa loginPopup.|
> |`options`  |Una colección de parámetros opcionales. En este caso, `storeAuthStateInCookie` y `cacheLocation` son una configuración opcional. Consulte la [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-basics#configuration-options) para más información sobre las opciones. |

### <a name="sign-in-users"></a>Inicio de sesión de usuarios

El siguiente fragmento de código muestra cómo iniciar la sesión de los usuarios:

```javascript
myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
    //Callback code here
})
```

> |Where  |  |
> |---------|---------|
> | `scopes`   | (Opcional) Contiene los ámbitos en los que se solicita el consentimiento del usuario en el momento del inicio de sesión (p. ej.: `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para API web personalizadas; es decir, `api://<Application ID>/access_as_user`). Aquí se pasa `applicationConfig.graphScopes`. |

> [!TIP]
> Como alternativa, puede usar el método `loginRedirect` para redirigir la página actual a la página de inicio de sesión en lugar de una ventana emergente.


### <a name="request-tokens"></a>Solicitud de tokens

MSAL tiene tres métodos para adquirir tokens: `acquireTokenRedirect`, `acquireTokenPopup` y `acquireTokenSilent`:

#### <a name="get-a-user-token-silently"></a>Obtención de un token de usuario en silencio

El método `acquireTokenSilent` controla la renovación y las adquisiciones de tokens sin la interacción del usuario. Después de que se ejecuten los métodos `loginRedirect` y `loginPopup` por primera vez, `acquireTokenSilent` es el método que se usa habitualmente para obtener los tokens que se utilizan para acceder a los recursos protegidos en las llamadas posteriores. Las llamadas para solicitar o renovar tokens se realizan en modo silencioso.

```javascript
myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> |Where  |  |
> |---------|---------|
> | `scopes`   | Contiene los ámbitos cuya devolución se solicita en el token de acceso de la API (p. ej.: `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para API web personalizadas; es decir; `api://<Application ID>/access_as_user`). Aquí se pasa `applicationConfig.graphScopes`.|

#### <a name="get-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

 Existen situaciones en las que hay que forzar a los usuarios a interactuar con el punto de conexión de Azure AD v2.0. Por ejemplo: 
* Es posible que los usuarios tengan que volver a escribir las credenciales porque su contraseña ha expirado
* La aplicación solicita acceso a ámbitos de recursos adicionales para los que se necesita el consentimiento del usuario
* Se requiere la autenticación en dos fases

El patrón habitual recomendado para la mayoría de las aplicaciones es llamar primero a `acquireTokenSilent`, después detectar la excepción y, por último, llamar a `acquireTokenRedirect` (o a `acquireTokenPopup`) para iniciar una solicitud interactiva.

Al llamar a `acquireTokenPopup(scope)` se abre una ventana emergente para iniciar sesión (o a `acquireTokenRedirect(scope)` se redirige a los usuarios al punto de conexión v2.0 de Azure AD) en la que los usuarios deben confirmar las credenciales, dar su consentimiento al recurso requerido o completar la autenticación en dos fases.

```javascript
myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> [!NOTE]
> Esta guía de inicio rápido usa los métodos `loginRedirect` y `acquireTokenRedirect` cuando el explorador utilizado es Internet Explorer, debido a un [problema conocido](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) por la manera en que Internet Explorer controla las ventanas emergentes.

## <a name="next-steps"></a>Pasos siguientes

Para obtener una guía paso a paso más detallada acerca de cómo compilar la aplicación para esta guía de inicio rápido, pruebe el tutorial de JavaScript siguiente.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Más información acerca de los pasos para crear la aplicación para esta guía de inicio rápido

> [!div class="nextstepaction"]
> [Llamar al tutorial de Graph API](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>Examen del repositorio de MSAL para encontrar documentación, preguntas más frecuentes, problemas, etc.

> [!div class="nextstepaction"]
> [Repositorio de GitHub msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
