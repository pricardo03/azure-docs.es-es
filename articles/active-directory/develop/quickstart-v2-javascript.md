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
ms.openlocfilehash: d044b1ad18df6eee1235e881038bbb9734a999ff
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52317354"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Inicio rápido: inicio de sesión y adquisición de un token de acceso por parte de los usuarios desde una aplicación de JavaScript

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido aprenderá a usar un código de ejemplo que muestra cómo una aplicación de página única (SPA) de JavaScript puede iniciar sesión en cuentas personales, profesionales y educativas, obtener un token de acceso para llamar a Microsoft Graph API o a cualquier API web.

![Funcionamiento de la aplicación de ejemplo que se genera en esta guía de inicio rápido](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Registro de una aplicación y descarga de la aplicación de la guía de inicio rápido
>
> #### <a name="step-1-register-your-application"></a>Paso 1: Registro de la aplicación
>
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com/) para registrar una aplicación.
> 1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
> 1. En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory**, y después **Registros de aplicaciones (versión preliminar) > Nuevo registro**.
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
* [Descargue los archivos principales del proyecto (para un servidor web, como Node.js)](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
* [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Extraiga el archivo ZIP en una carpeta local (por ejemplo, **C:\Azure-Samples**).

#### <a name="step-3-configure-your-javascript-app"></a>Paso 3: Configuración de la aplicación de JavaScript

> [!div renderon="docs"]
> Edite `index.html` y establezca los valores `clientID` y `authority` bajo `applicationConfig`.

> [!div class="sxs-lookup" renderon="portal"]
> Edite `index.html` y reemplace `applicationConfig` por:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
> [!div renderon="docs"]
>
> Donde:
> - `Enter_the_Application_Id_here`: es el **identificador de aplicación (cliente)** de la aplicación que registró.
> - `Enter_the_Tenant_Info_Here` se establece como una de las opciones siguientes:
>   - Si la aplicación admite **Accounts in this organizational directory** (Cuentas en este directorio de la organización), reemplace este valor por el **identificador de inquilino** o el **nombre de inquilino** (por ejemplo, contoso.microsoft.com)
>   - Si la aplicación admite **Cuentas en cualquier directorio organizativo**, reemplace este valor por `organizations`
>   - Si la aplicación admite **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio organizativo y cuentas Microsoft personales), reemplace este valor por `common`
>
> > [!TIP]
> > Para buscar los valores de **Identificador de aplicación (cliente)**, **Identificador de directorio (inquilino)** y **Tipos de cuenta admitidos**, vaya a la página **Información general** en Azure Portal.

> [!NOTE]
> El servidor está configurado para escuchar en el puerto 30662 en el archivo *server.js* en el proyecto [Node.js](https://nodejs.org/en/download/) y el archivo *.csproj* en el proyecto de [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
>

#### <a name="step-4-run-the-project"></a>Paso 4: Ejecución del proyecto

* Si usa Node.js:

    1. Ejecute el comando siguiente desde el directorio del proyecto para iniciar el servidor:

        ```batch
        npm install
        node server.js
        ```

    1. Abra un explorador web y vaya a `http://localhost:30662/`.
    1. Haga clic en el botón **Iniciar sesión**, inicie sesión y llame a Microsoft Graph API.


* Si usa Visual Studio, asegúrese de seleccionar la solución del proyecto y presione **F5** para ejecutar el proyecto.

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
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, applicationConfig.authority, acquireTokenRedirectCallBack, {storeAuthStateInCookie: true, cacheLocation: "localStorage"});
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
> | `scopes`   | (Opcional) Contiene los ámbitos en los que se solicita el consentimiento del usuario en el momento del inicio de sesión. Por ejemplo, `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para API web personalizadas, es decir, `api://<Application ID>/access_as_user`. Aquí se pasa `applicationConfig.graphScopes`. |

> [!TIP]
> Como alternativa, puede usar el método `loginRedirect` para redirigir la página actual a la página de inicio de sesión en lugar de una ventana emergente.

### <a name="request-tokens"></a>Solicitud de tokens

MSAL tiene tres métodos para adquirir tokens: `acquireTokenRedirect`, `acquireTokenPopup` y `acquireTokenSilent`.

#### <a name="get-a-user-token-silently"></a>Obtención de un token de usuario en silencio

El método `acquireTokenSilent` controla la renovación y las adquisiciones de tokens sin la interacción del usuario. Después de que se ejecuten los métodos `loginRedirect` y `loginPopup` por primera vez, `acquireTokenSilent` es el método que se usa habitualmente para obtener los tokens que se utilizan para acceder a los recursos protegidos en las llamadas posteriores. Las llamadas para solicitar o renovar tokens se realizan en modo silencioso.

```javascript
myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> |Where  |  |
> |---------|---------|
> | `scopes`   | Contiene los ámbitos cuya devolución se solicita en el token de acceso de la API. Por ejemplo, `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para API web personalizadas, es decir, `api://<Application ID>/access_as_user`. Aquí se pasa `applicationConfig.graphScopes`.|

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
