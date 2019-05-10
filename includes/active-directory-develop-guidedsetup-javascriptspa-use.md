---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 8785f89a335f0ae7d983f267176da1656aee57a0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65199476"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Uso de la biblioteca de autenticación de Microsoft (MSAL) para iniciar la sesión del usuario

1. Agregue el siguiente código al archivo `index.html`, dentro de las etiquetas `<script></script>`:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "Enter_the_Application_Id_here"
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

Cuando un usuario hace clic en el botón **Iniciar sesión** por primera vez, el método `signIn` llama a `loginPopup` para iniciar la sesión del usuario. Este método hace que se abra una ventana emergente con el *punto de conexión de la plataforma de identidad de Microsoft* para pedir y validar las credenciales del usuario. Como resultado de un inicio de sesión correcto, se redirige al usuario a la página *index.html* original y se recibe un token, el que `msal.js` procesa, y se almacena en caché la información que contiene el token. Este token se conoce como el *token de identificador* y contiene información básica sobre el usuario, como su nombre. Si piensa utilizar los datos proporcionados por este token para algún propósito, debe asegurarse de que el servidor backend lo valide para garantizar que el token se emitió a un usuario válido para la aplicación.

La instancia de SPA generada por esta guía llama a `acquireTokenSilent` o a `acquireTokenPopup` para adquirir un *token de acceso* que se usa para consultar la información del perfil de usuario a Microsoft Graph API. Si necesita obtener un ejemplo que valide el token de identificador, eche un vistazo a [esta](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "aplicación de ejemplo Github active-directory-javascript-singlepageapp-dotnet-webapi-v2") en GitHub (se usa una ASP.NET WEB API para la validación del token).

#### <a name="getting-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

Después del inicio de sesión inicial, no desea pedir a los usuarios que se vuelvan a autenticar cada vez que tengan que solicitar un token para acceder a un recurso, por lo que se usará *acquireTokenSilent* la mayor parte del tiempo para adquirir los tokens. Sin embargo, hay situaciones en las que resulta necesario forzar a que los usuarios interactúen con el punto de conexión de la plataforma de identidad de Microsoft. Algunos ejemplos de esto son los siguientes:

- Es posible que los usuarios deban volver a escribir las credenciales porque la contraseña expiró
- La aplicación solicita acceso a un recurso para el cual el usuario necesita consentimiento
- Se requiere la autenticación en dos fases

Al llamar a *acquireTokenPopup*, se abre una ventana emergente (o al llamar a *acquireTokenRedirect*, se redirige a los usuarios al punto de conexión de la plataforma de identidad de Microsoft), donde los usuarios deben confirmar las credenciales, dar su consentimiento al recurso requerido o completar la autenticación en dos fases.

#### <a name="getting-a-user-token-silently"></a>Obtención de un token de usuario en silencio

El método `acquireTokenSilent` controla la renovación y las adquisiciones de tokens sin la interacción del usuario. Después de ejecutar `loginPopup` (o `loginRedirect`) por primera vez, `acquireTokenSilent` es el método usado habitualmente para obtener los tokens empleados para acceder a recursos protegidos en las llamadas posteriores, ya que las llamadas para solicitar o renovar los tokens se realizan en modo silencioso.
`acquireTokenSilent` puede generar errores en algunos casos, por ejemplo, si la contraseña del usuario expiró. La aplicación puede abordar esta excepción de dos maneras:

1. Realizar una llamada a `acquireTokenPopup` inmediatamente, lo que ocasiona que el usuario tenga que iniciar sesión. Este patrón se da comúnmente en aplicaciones en línea en las que no hay ningún contenido no autenticado en la aplicación disponible para el usuario. El ejemplo generado por esta instalación guiada usa este patrón.

2. Las aplicaciones también pueden hacer una indicación visual al usuario de que se requiere un inicio de sesión interactivo, de manera que el usuario pueda seleccionar el momento adecuado para iniciar sesión, o la aplicación puede reintentar `acquireTokenSilent` en un momento posterior. Esto se usa habitualmente cuando el usuario puede utilizar otras funciones de la aplicación sin que se le interrumpa: por ejemplo, hay contenido no autenticado disponible en la aplicación. En este caso, el usuario puede decidir cuando desea iniciar sesión para acceder al recurso protegido o para actualizar la información obsoleta.

> [!NOTE]
> En esta guía de inicio rápido, se usan los métodos `loginRedirect` y `acquireTokenRedirect` cuando el explorador utilizado es Internet Explorer, debido a un [problema conocido](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) con la manera en que Internet Explorer controla las ventanas emergentes.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Llamada a Microsoft Graph API con el token que acaba de obtener

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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Más información acerca de cómo realizar una llamada de REST a una API protegida

En la aplicación de ejemplo creada en esta guía, el método `callMSGraph()` se usa para realizar una solicitud HTTP `GET` a un recurso protegido que requiere un token y, a continuación, devolver el contenido al autor de la llamada. Este método agrega el token adquirido al *encabezado de autorización HTTP*. En este ejemplo de aplicación creada en esta guía, el recurso es el punto de conexión *me* de Microsoft Graph API, que muestra información del perfil del usuario.

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
