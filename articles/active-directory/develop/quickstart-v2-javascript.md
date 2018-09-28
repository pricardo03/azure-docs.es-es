---
title: Guía de inicio rápido de Azure AD v2 para JavaScript | Microsoft Docs
description: Aprenda la forma en que las aplicaciones de JavaScript pueden llamar a una API que requiera tokens de acceso mediante un punto de conexión de Azure Active Directory v2.0
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/21/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4c64552ab23331755bf1d292bede61e78b339df0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987431"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Inicio rápido: inicio de sesión y adquisición de un token de acceso por parte de los usuarios desde una aplicación de JavaScript

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido aprenderá a usar un código de ejemplo que muestra cómo una aplicación de página única (SPA) de JavaScript puede iniciar sesión en cuentas personales, profesionales y educativas, obtener un token de acceso y llamar a Microsoft Graph API.

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
* [Descargue los archivos principales del proyecto (para un servidor web, como Node.js)](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip)
* [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)

Extraiga el archivo ZIP en una carpeta local (por ejemplo, **C:\Azure-Samples**).

#### <a name="step-3-configure-your-javascript-app"></a>Paso 3: Configuración de la aplicación de JavaScript

> [!div renderon="docs"]
> Edite `msalconfig.js` y reemplace `Enter_the_Application_Id_here` por el identificador de la aplicación que acaba de registrar. El *identificador de la aplicación* se puede encontrar en la página *Introducción*.

> [!div class="sxs-lookup" renderon="portal"]
> Edite `msalconfig.js` y reemplace msalconfig por:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
> [!NOTE]
> Si usa [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), el identificador URI de redirección se establecerá en `http://localhost:30662/`, tal como está configurado en el proyecto del ejemplo de código. Si usa [Node.js](https://nodejs.org/en/download/) o cualquier otro servidor web, establezca el identificador URI de redirección en `http://localhost:30662/` y, después, configure el servidor para empezar a escuchar en este puerto.
>

#### <a name="step-4-run-the-project"></a>Paso 4: Ejecución del proyecto

Si usa Visual Studio, presione **F5** para ejecutar el proyecto.

Si usa Node.js, en una línea de comandos puede ejecutar lo siguiente desde el directorio del proyecto para iniciar el servidor:
 ```batch
 npm install
 node server.js
 ```
Abra un explorador web y vaya a `http://localhost:30662/`. Haga clic en el botón **Call Microsoft Graph API** (Llamar a Microsoft Graph API).


## <a name="more-information"></a>Más información

### <a name="msaljs"></a>*msal.js*

MSAL es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por Microsoft Azure Active Directory (Azure AD). El archivo *index.html* de la guía de inicio rápido contiene una referencia a la biblioteca:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>
````

Si tiene Node instalado, también puede descargarlo a través de npm:

```batch
npm install msal
```

### <a name="msal-initialization"></a>Inicialización de MSAL

El código de la guía de inicio de sesión también muestra cómo inicializar la biblioteca:

```javascript
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});
```

> |Where  |  |
> |---------|---------|
> |`ClientId`     |El identificador de la aplicación registrada en Azure Portal|
> |`authority`    |Es la dirección URL de la autoridad. El paso de *null* establece la autoridad de forma predeterminada en `https://login.microsoftonline.com/common`. Si la aplicación tienen un solo inquilino (las cuentas de destino en un solo directorio), establezca este valor en `https://login.microsoftonline.com/<tenant name or ID>`|
> |`loginCallBack`| El método de devolución de llamada al que se llama después de la autenticación realiza la redirección a la aplicación|
> |`redirectUri`  |La dirección URL a la que usuarios se redirigen después de la autenticación con Azure AD|

### <a name="sign-in-users"></a>Inicio de sesión de usuarios

El siguiente fragmento de código muestra cómo iniciar la sesión de los usuarios:

```javascript
userAgentApplication.loginRedirect(graphAPIScopes);
```

> |Where  |  |
> |---------|---------|
> | `graphAPIScopes`   | (Opcional) Contiene los ámbitos en los que se solicita el consentimiento del usuario en el momento del inicio de sesión (p. ej.: `[ "user.read" ]` para Microsoft Graph o `[ "api://<Application ID>/access_as_user" ]` para API web personalizadas). |

> [!TIP]
> Como alternativa, puede usar el método `loginPopup` para mostrar una ventana emergente para iniciar la sesión del usuario.

### <a name="request-tokens"></a>Solicitud de tokens

MSAL tiene tres métodos para adquirir tokens: `acquireTokenRedirect`, `acquireTokenPopup` y `acquireTokenSilent`:

#### <a name="get-a-user-token-silently"></a>Obtención de un token de usuario en silencio

El método `acquireTokenSilent` controla la renovación y las adquisiciones de tokens sin la interacción del usuario. Después de que se ejecuten los métodos `loginRedirect` y `loginPopup` por primera vez, `acquireTokenSilent` es el método que se usa habitualmente para obtener los tokens que se utilizan para acceder a los recursos protegidos en las llamadas posteriores. Las llamadas para solicitar o renovar tokens se realizan en modo silencioso.

```javascript
// Try to acquire the token used to query Graph API silently first:
userAgentApplication.acquireTokenSilent(graphAPIScopes)
```

> |Where  |  |
> |---------|---------|
> | `graphAPIScopes`   | Contiene los ámbitos cuya devolución se solicita en el token de acceso de la API (p. ej.: `[ "user.read" ]` para Microsoft Graph o `[ "api://<Application ID>/access_as_user" ]` para API web personalizadas). |

#### <a name="get-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

 Existen situaciones en las que hay que forzar a los usuarios a interactuar con el punto de conexión de Azure AD v2.0. Por ejemplo: 
* Es posible que los usuarios tengan que volver a escribir las credenciales porque su contraseña ha expirado
* La aplicación solicita acceso a ámbitos de recursos adicionales para los que se necesita el consentimiento del usuario
* Se requiere la autenticación en dos fases

El patrón habitual recomendado para la mayoría de las aplicaciones es llamar primero a `acquireTokenSilent`, después detectar la excepción y, por último, llamar a `acquireTokenRedirect` (o a `acquireTokenPopup`) para iniciar una solicitud interactiva.

La llamada a `acquireTokenRedirect(scope)` provoca la redirección de los usuarios al punto de conexión de Azure AD v2.0 (o `acquireTokenPopup(scope)` abre una ventana emergente), en el que los usuarios deben interactuar, ya sea confirmando las credenciales, dando el consentimiento al recurso requerido o completando la autenticación en dos fases.

```javascript
userAgentApplication.acquireTokenRedirect(graphAPIScopes);
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener una guía paso a paso más detallada acerca de cómo compilar la aplicación para esta guía de inicio rápido, pruebe el tutorial de JavaScript siguiente.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Más información acerca de los pasos para crear la aplicación para esta guía de inicio rápido

> [!div class="nextstepaction"]
> [Llamar al tutorial de Graph API](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>Examen del repositorio de MSAL para encontrar documentación, preguntas más frecuentes, problemas, etc.

> [!div class="nextstepaction"]
> [Repositorio de GitHub msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
