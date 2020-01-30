---
title: 'Tutorial de aplicaciones de página única de JavaScript: Plataforma de identidad de Microsoft | Azure'
description: Cómo pueden llamar las aplicaciones SPA de JavaScript a una API que requiera tokens de acceso mediante el punto de conexión de Azure Active Directory v2.0
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 33e1037a0fe261f9fb0d06a9ebb0b3b323fe8d5f
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701270"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Inicio de sesión de usuarios y llamada a Microsoft Graph API desde una aplicación de página única (SPA) de JavaScript

En esta guía se muestra cómo una aplicación de una sola página (SPA) de JavaScript puede:
- Iniciar sesión en cuentas personales, profesionales y educativas 
- Adquisición de un token de acceso
- Llamar a Microsoft Graph API o a otras API que requieran tokens de acceso del *punto de conexión de la Plataforma de identidad de Microsoft*.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funcionamiento de la aplicación de ejemplo generada por esta guía

![Muestra cómo funciona la aplicación de ejemplo generada por este tutorial](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>Más información

La aplicación de ejemplo que se crea con esta guía permite que una aplicación de página única de JavaScript haga consultas a Microsoft Graph API o a una API web que acepte tokens de un punto de conexión de la Plataforma de identidad de Microsoft. En este escenario, después de que un usuario inicia sesión, se solicita un token de acceso y se agrega a las solicitudes HTTP mediante el encabezado de autorización. La adquisición y la renovación de los tokens se realizan por medio de la biblioteca de autenticación de Microsoft (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotecas

Esta guía utiliza la siguiente biblioteca:

|Biblioteca|Descripción|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Biblioteca de autenticación de Microsoft para la vista preliminar de JavaScript|

> [!NOTE]
> *Msal.js* apunta al punto de conexión de la Plataforma de identidad de Microsoft, lo que permite que las cuentas personales, profesionales y educativas inicien sesión y adquieran tokens. El punto de conexión de la Plataforma de identidad de Microsoft tiene [algunas limitaciones](azure-ad-endpoint-comparison.md#limitations).
> Para comprender las diferencias entre los puntos de conexión v1.0 y v2.0, consulte la [guía de comparación entre puntos de conexión](azure-ad-endpoint-comparison.md).

<!--end-collapse-->

## <a name="set-up-your-web-server-or-project"></a>Configuración de un proyecto o servidor web

> ¿Prefiere descargar este proyecto de ejemplo en su lugar? Realice cualquiera de las siguientes acciones:
> 
> - Para ejecutar el proyecto con un servidor web local, como Node.js, [descargue los archivos del proyecto](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).
>
> - (Opcional) Para ejecutar el proyecto mediante el servidor de Microsoft Internet Information Services (IIS), [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).
>
> Para configurar el ejemplo de código antes de ejecutarlo, vaya al [paso de configuración](#register-your-application).

## <a name="prerequisites"></a>Prerequisites

* Para ejecutar este tutorial, necesita un servidor web local, como [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core) o la integración de IIS Express con [Visual Studio 2017](https://www.visualstudio.com/downloads/).

* Si usa Node.js para ejecutar el proyecto, instale un entorno de desarrollo integrado (IDE), como [Visual Studio Code](https://code.visualstudio.com/download), para editar los archivos del proyecto.

* Las instrucciones de esta guía se basan tanto en Node.js como en Visual Studio 2017, pero puede usar cualquier otro entorno de desarrollo o servidor web.

## <a name="create-your-project"></a>Creación del proyecto

> ### <a name="option-1-nodejs-or-other-web-servers"></a>Opción 1: Node.js u otros servidores web
> Asegúrese de que tiene instalado [Node.js](https://nodejs.org/en/download/) y, a continuación, cree una carpeta para hospedar la aplicación.
>
> ### <a name="option-2-visual-studio"></a>Opción 2: Visual Studio
> Si usa Visual Studio y crea un proyecto, siga estos pasos:
> 1. Abra Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**.
> 1. En **Visual C#\Web**, seleccione **Aplicación web ASP.NET (.NET Framework)** .
> 1. Escriba un nombre para la aplicación y seleccione **Aceptar**.
> 1. En **Nueva aplicación web ASP.NET**, seleccione **Vacía**.

## <a name="create-the-spa-ui"></a>Creación de la interfaz de usuario de SPA
1. Cree un archivo *index.html* para JavaScript SPA. Si usa Visual Studio, seleccione el proyecto (carpeta raíz del proyecto). Haga clic con el botón derecho y seleccione **Agregar** > **Nuevo elemento** > **Página HTML**, y asigne un nombre al archivo *index.html*.

1. En el archivo *index.html*, agregue el código siguiente:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```

   > [!TIP]
   > Puede reemplazar la versión de MSAL.js del script anterior con la versión más reciente en la sección de [versiones de MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).

## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Uso de la biblioteca de autenticación de Microsoft (MSAL) para iniciar la sesión del usuario

Agregue el siguiente código al archivo `index.html`, dentro de las etiquetas `<script></script>`:

   ```JavaScript
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

   var graphConfig = {
       graphMeEndpoint: "https://graph.microsoft.com/v1.0/me"
   };

   // this can be used for login or token request, however in more complex situations
   // this can have diverging options
   var requestObj = {
        scopes: ["user.read"]
   };

   var myMSALObj = new Msal.UserAgentApplication(msalConfig);
   // Register Callbacks for redirect flow
   myMSALObj.handleRedirectCallback(authRedirectCallBack);


   function signIn() {

       myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
           //Login Success
           showWelcomeMessage();
           acquireTokenPopupAndCallMSGraph();
       }).catch(function (error) {
           console.log(error);
       });
   }

   function acquireTokenPopupAndCallMSGraph() {
       //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
       myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
       }).catch(function (error) {
            console.log(error);
            // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
            // Call acquireTokenPopup(popup window)
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
                    callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
                }).catch(function (error) {
                    console.log(error);
                });
            }
       });
   }


   function graphAPICallback(data) {
       document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
   }


   function showWelcomeMessage() {
       var divWelcome = document.getElementById('WelcomeMessage');
       divWelcome.innerHTML = 'Welcome ' + myMSALObj.getAccount().userName + "to Microsoft Graph API";
       var loginbutton = document.getElementById('SignIn');
       loginbutton.innerHTML = 'Sign Out';
       loginbutton.setAttribute('onclick', 'signOut();');
   }


   //This function can be removed if you do not need to support IE
   function acquireTokenRedirectAndCallMSGraph() {
        //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
        myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error);
            // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
            // Call acquireTokenRedirect
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenRedirect(requestObj);
            }
        });
   }


   function authRedirectCallBack(error, response) {
       if (error) {
           console.log(error);
       }
       else {
           if (response.tokenType === "access_token") {
               callMSGraph(graphConfig.graphEndpoint, response.accessToken, graphAPICallback);
           } else {
               console.log("token type is:" + response.tokenType);
           }
       }
   }

   function requiresInteraction(errorCode) {
       if (!errorCode || !errorCode.length) {
           return false;
       }
       return errorCode === "consent_required" ||
           errorCode === "interaction_required" ||
           errorCode === "login_required";
   }

   // Browser check variables
   var ua = window.navigator.userAgent;
   var msie = ua.indexOf('MSIE ');
   var msie11 = ua.indexOf('Trident/');
   var msedge = ua.indexOf('Edge/');
   var isIE = msie > 0 || msie11 > 0;
   var isEdge = msedge > 0;
   //If you support IE, our recommendation is that you sign-in using Redirect APIs
   //If you as a developer are testing using Edge InPrivate mode, please add "isEdge" to the if check
   // can change this to default an experience outside browser use
   var loginType = isIE ? "REDIRECT" : "POPUP";

   if (loginType === 'POPUP') {
        if (myMSALObj.getAccount()) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }
   }
   else if (loginType === 'REDIRECT') {
       document.getElementById("SignIn").onclick = function () {
            myMSALObj.loginRedirect(requestObj);
       };
       if (myMSALObj.getAccount() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenRedirectAndCallMSGraph();
        }
   } else {
       console.error('Please set a valid login type');
   }
   ```

<!--start-collapse-->
### <a name="more-information"></a>Más información

Cuando un usuario selecciona el botón **Iniciar sesión** por primera vez, el método `signIn` llama a `loginPopup` para iniciar la sesión del usuario. Este método hace que se abra una ventana emergente con el *punto de conexión de la Plataforma de identidad de Microsoft* para pedir y validar las credenciales del usuario. Después de iniciar sesión correctamente, el usuario se redirige de nuevo a la página *index.html* original. `msal.js` recibe y procesa un token, y la información contenida en él se almacena en caché. Este token se conoce como el *token de identificador* y contiene información básica sobre el usuario, como su nombre. Si piensa utilizar los datos proporcionados por este token para algún propósito, debe asegurarse de que el servidor backend lo valide para garantizar que el token se emitió a un usuario válido para la aplicación.

La instancia de SPA generada por esta guía llama a `acquireTokenSilent` o a `acquireTokenPopup` para adquirir un *token de acceso* que se usa para consultar la información del perfil de usuario a Microsoft Graph API. Si necesita un ejemplo que valide el token de identificador, eche un vistazo a [esta](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Ejemplo active-directory-javascript-singlepageapp-dotnet-webapi-v2 de GitHub") aplicación de ejemplo en GitHub. En el ejemplo se usa ASP.NET Web API para la validación de tokens.

#### <a name="getting-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

Después del inicio de sesión inicial, no desea pedir a los usuarios que se vuelvan a autenticar cada vez que tengan que solicitar un token para acceder a un recurso. De modo que se debe usar *acquireTokenSilent* la mayor parte del tiempo para adquirir tokens. Sin embargo, hay situaciones en las que es necesario hacer que los usuarios interactúen con el punto de conexión de la Plataforma de identidad de Microsoft. Algunos ejemplos son:

- Los usuarios deben volver a escribir las credenciales porque la contraseña expiró.
- La aplicación solicita acceso a un recurso para el cual el usuario necesita consentimiento.
- Se requiere la autenticación en dos fases.

A llamar a *acquireTokenPopup*, se abre una ventana emergente (o *acquireTokenRedirect* redirige a los usuarios al punto de conexión de la Plataforma de identidad de Microsoft). En esa ventana, los usuarios tienen que interactuar confirmando sus credenciales, dándole el consentimiento al recurso requerido o completando la autenticación en dos fases.

#### <a name="getting-a-user-token-silently"></a>Obtención de un token de usuario en silencio

El método `acquireTokenSilent` controla la renovación y las adquisiciones de tokens sin la interacción del usuario. Después de ejecutar `loginPopup` (o `loginRedirect`) por primera vez, `acquireTokenSilent` es el método que se usa habitualmente para obtener los tokens que se utilizan para acceder a los recursos protegidos en las llamadas posteriores. (Las llamadas para solicitar o renovar tokens se realizan en modo silencioso). `acquireTokenSilent` puede dar error en algunos casos. Por ejemplo, la contraseña del usuario puede haber expirado. La aplicación puede abordar esta excepción de dos maneras:

1. Realice una llamada a `acquireTokenPopup` inmediatamente, lo que desencadena un mensaje de inicio de sesión de usuario. Este patrón se da comúnmente en aplicaciones en línea en las que no hay ningún contenido no autenticado en la aplicación disponible para el usuario. El ejemplo generado por esta instalación guiada usa este patrón.

2. Las aplicaciones también pueden hacer una indicación visual al usuario de que se requiere un inicio de sesión interactivo, de manera que el usuario pueda seleccionar el momento adecuado para iniciar sesión, o la aplicación puede reintentar `acquireTokenSilent` en un momento posterior. Esto se suele usar cuando el usuario puede utilizar otra funcionalidad de la aplicación sin ser interrumpido. Por ejemplo, podría haber contenido no autenticado disponible en la aplicación. En esta situación, el usuario puede decidir cuándo desea iniciar sesión para acceder al recurso protegido o para actualizar la información obsoleta.

> [!NOTE]
> Este inicio rápido usa los métodos `loginRedirect` y `acquireTokenRedirect` cuando se usa Internet Explorer. Seguimos esta práctica debido a un [problema conocido](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) relacionado con la forma en que Internet Explorer controla las ventanas emergentes.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-acquired"></a>Llamada a Microsoft Graph API con el token que acaba de obtener

Agregue el siguiente código al archivo `index.html`, dentro de las etiquetas `<script></script>`:

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Más información acerca de cómo realizar una llamada de REST a una API protegida

En esta aplicación de ejemplo que se crea en esta guía, el método `callMSGraph()` se usa para realizar una solicitud HTTP `GET` a un recurso protegido que requiere un token. A continuación, el método devuelve el contenido al autor de la llamada. Este método agrega el token adquirido al *encabezado de autorización HTTP*. En este ejemplo de aplicación creada en esta guía, el recurso es el punto de conexión *me* de Microsoft Graph API, que muestra información del perfil del usuario.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Adición de un método para cerrar la sesión del usuario

Agregue el siguiente código al archivo `index.html`, dentro de las etiquetas `<script></script>`:

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```

## <a name="register-your-application"></a>Registrar su aplicación

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Si la cuenta proporciona acceso a más de un inquilino, selecciónela en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD que desee utilizar.
1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
1. Cuando se abra la página **Registrar una aplicación**, escriba el nombre de su aplicación.
1. En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
1. En la sección **URI de redireccionamiento**, seleccione la plataforma **Web** en la lisa desplegable y, luego, establezca el valor en la dirección URL de la aplicación que se basa en el servidor web.

   Para información sobre cómo establecer y obtener la dirección URL de redireccionamiento para Node.js y Visual Studio, vea la siguiente sección "Establecimiento de una dirección URL de redireccionamiento para Node.js" y [Establecimiento de una dirección URL de redireccionamiento para Visual Studio](#set-a-redirect-url-for-visual-studio).

1. Seleccione **Registrar**.
1. En la página de **información general** de la aplicación, anote el valor del **Identificador de aplicación (cliente)** para su uso posterior.
1. Para esta guía, se requiere que habilite el [flujo de concesión implícita](v2-oauth2-implicit-grant-flow.md). En el panel izquierdo de la aplicación registrada, seleccione **Autenticación**.
1. En **Configuración avanzada**, en **Concesión implícita**, active las casillas **Tokens de id.** y **Tokens de acceso**. Los tokens de identificador y los tokens de acceso son obligatorios, ya que esta aplicación tiene que iniciar la sesión de los usuarios y llamar a una API.
1. Seleccione **Guardar**.

> #### <a name="set-a-redirect-url-for-nodejs"></a>Configuración de una dirección URL de redireccionamiento para Node.js
> Para Node.js, puede establecer el puerto del servidor web en el archivo *server.js*. En este tutorial se usa el puerto 30662 como referencia, pero puede usar cualquier otro disponible.
>
> Para configurar una dirección URL de redireccionamiento en la información de registro de aplicación, vuelva al panel **Registro de aplicación** y realice una de las acciones siguientes:
>
> - Establezca *`http://localhost:30662/`* como la **dirección URL de redireccionamiento**.
> - Si usa un puerto TCP personalizado, use *`http://localhost:<port>/`* (donde *\<port>* es el número de puerto TCP personalizado).
>
> #### <a name="set-a-redirect-url-for-visual-studio"></a>Configuración de una dirección URL de redireccionamiento para Visual Studio
> Para obtener la dirección URL de redireccionamiento de Visual Studio, siga estos pasos:
> 1. En el Explorador de soluciones, seleccione el proyecto.
>
>    Se abre la ventana **Propiedades**. Si no se abre, presione F4.
>
>    ![La ventana Propiedades del proyecto JavaScriptSPA](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)
>
> 1. Copie el valor **URL**.
> 1. Vuelva al panel **Registro de aplicación** y pegue el valor copiado como la **Dirección URL de redireccionamiento**.

#### <a name="configure-your-javascript-spa"></a>Configuración de JavaScript SPA

1. En el archivo *index.html* creado durante la configuración del proyecto, agregue la información del registro de aplicación. En la parte superior del archivo, dentro de las etiquetas `<script></script>`, agregue el código siguiente:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "<Enter_the_Application_Id_here>",
            authority: "https://login.microsoftonline.com/<Enter_the_Tenant_info_here>"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Donde:
    - *\<Enter_the_Application_Id_here>* es el **identificador de aplicación (cliente)** de la aplicación que ha registrado.
    - *\<Enter_the_Tenant_info_here >* está establecido en una de las opciones siguientes:
       - Si la aplicación admite *solo las cuentas de este directorio organizativo*, reemplace este valor por el **identificador de inquilino** o el **nombre de inquilino** (por ejemplo, *contoso.microsoft.com*).
       - Si la aplicación admite *cuentas en cualquier directorio organizativo*, reemplace este valor por **organizaciones**.
       - Si la aplicación admite *cuentas en cualquier directorio organizativo y cuentas Microsoft personales*, reemplace este valor por **común**. Para restringir la compatibilidad a *Personal Microsoft accounts only* (Solo cuentas Microsoft personales), reemplace este valor por **consumidores**.

## <a name="test-your-code"></a>Prueba del código

Para probar el código, use cualquiera de estos entornos.

### <a name="test-with-nodejs"></a>Prueba con Node.js

Si no está utilizando Visual Studio, asegúrese de que el servidor web está activado.

1. Configure el servidor para que escuche un puerto TCP que esté basado en la ubicación del archivo *index.html*. Para Node.js, inicie el servidor web para que escuche el puerto; para ello, ejecute los siguientes comandos desde la carpeta de la aplicación en un símbolo del sistema de la línea de comandos:

    ```bash
    npm install
    node server.js
    ```
1. En el explorador, escriba **http://\<span>\</span>localhost:30662** o **http://\<span>\</span>localhost:{port}** , donde *port* es el puerto de escucha del servidor web. Verá el contenido del archivo *index.html* y el botón **Iniciar sesión**.

### <a name="test-with-visual-studio"></a>Pruebas con Visual Studio

Si usa Visual Studio, seleccione la solución del proyecto y, luego, presione F5 para ejecutar el proyecto. El explorador se abre en la ubicación http://<span></span>localhost:{port} y el botón **Iniciar sesión** debería verse.

## <a name="test-your-application"></a>Prueba de la aplicación

Cuando el explorador haya cargado el archivo *index.html*, seleccione **Iniciar sesión**. Se le pedirá que inicie sesión con el punto de conexión de la plataforma de identidad de Microsoft:

![La ventana de inicio de sesión en la cuenta de SPA de JavaScript](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Consentimiento para el acceso a la aplicación

La primera vez que inicie sesión en la aplicación, se le pedirá que le conceda acceso a su perfil e inicie su sesión:

![La ventana "Permisos solicitados"](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Visualización de los resultados de la aplicación

Una vez iniciada la sesión, la información del perfil de usuario se devuelve en la respuesta de Microsoft Graph API que se muestra en la página:

![Resultados de la llamada a Microsoft Graph API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Más información sobre los ámbitos y permisos delegados

Microsoft Graph API requiere el ámbito *user.read* para leer el perfil del usuario. De forma predeterminada, este ámbito se agrega automáticamente en todas las aplicaciones que se registran en el portal de registro. Otras API de Microsoft Graph, así como las API personalizadas para el servidor back-end, pueden requerir ámbitos adicionales. Por ejemplo, Microsoft Graph API requiere el ámbito *Calendars.Read* para mostrar los calendarios del usuario.

Para tener acceso a los calendarios del usuario en el contexto de una aplicación, agregue el permiso delegado *Calendars.Read* a la información del registro de la aplicación. A continuación, agregue el ámbito *Calendars.Read* a la llamada `acquireTokenSilent`.

>[!NOTE]
>Es posible que se pida al usuario algún consentimiento adicional a medida que aumente el número de ámbitos.

Si una API de back-end no requiere un ámbito (no se recomienda), puede usar *clientId* como ámbito en las llamadas de adquisición de tokens.

<!--end-collapse-->

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Ayúdenos a mejorar la Plataforma de identidad de Microsoft. Rellene una breve encuesta de dos preguntas y háganos saber su opinión.

> [!div class="nextstepaction"]
> [Encuesta sobre la Plataforma de identidad de Microsoft](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
