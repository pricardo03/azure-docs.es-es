---
title: Inicio rápido de la Plataforma de identidad de Microsoft para iOS | Azure
description: Aprenda a iniciar sesión por los usuarios y a consultar Microsoft Graph en una aplicación de iOS.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: brandwe
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b3802d8f92913e416cc6a80f899179fde80cec30
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65962592"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Inicio rápido: Hacer que los usuarios inicien sesión y llamar a Microsoft Graph API desde una aplicación de iOS

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido se incluye un código de ejemplo que muestra cómo una aplicación de iOS puede iniciar sesión en cuentas personales, profesionales y educativas, obtener un token de acceso y llamar a Microsoft Graph API.

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-ios/ios-intro.svg)

> [!NOTE]
> **Requisitos previos**
> * XCode 10+
> * iOS 10+ 

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Registro y descarga de la aplicación de inicio rápido
> Tiene dos opciones para comenzar con la aplicación de inicio rápido:
> * [Rápido] [Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Manual] [Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
> Registro de aplicaciones
> 1. Vaya al nuevo panel de [Azure Portal: Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs).
> 1. Escriba un nombre para la aplicación y seleccione **Registrar**.
> 1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación con un solo clic.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo
>
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
> Para registrar la aplicación y agregar la información de registro de la aplicación a la solución de forma manual, siga estos pasos:
>
> 1. Vaya a la página [Registros de aplicaciones](https://aka.ms/MobileAppReg) de la plataforma de identidad de Microsoft para desarrolladores.
> 1. Seleccione **Nuevo registro**.
> 1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
>      - En la sección **Nombre**, escriba un nombre descriptivo para la aplicación que se mostrará a los usuarios de la aplicación cuando inicien sesión o den su consentimiento para la aplicación, por ejemplo `iOSQuickstart`.
>      - Omita las demás configuraciones de esta página. 
>      - Presione el botón `Register`.
> 1. Haga clic en la nueva aplicación y vaya a `Authentication` > `Add Platform` > `iOS`.    
>      - Escriba el ***identificador de agrupación*** de la aplicación. 
> 1. Seleccione `Configure` y guarde los detalles de la ***Configuración de MSAL*** para usarlos más adelante. 

> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>Paso 1: Configuración de la aplicación
> Para que el código de ejemplo de esta guía de inicio rápido funcione, debe agregar un identificador URI de redirección compatible con el agente de autenticación. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-ios/green-check.png) La aplicación está configurada con estos atributos

#### <a name="step-2-download-your-web-server-or-project"></a>Paso 2: Descarga del servidor web o proyecto

- [Descarga del código de ejemplo](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Paso 3: Configuración del proyecto

> [!div renderon="docs"]
> Si ha seleccionado la opción 1 anterior, puede omitir estos pasos. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Extraiga el archivo ZIP y abra el proyecto en XCode.
> 1. Edite **ViewController.swift** y reemplace la línea que empieza con "let kClientID" por el siguiente fragmento de código:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_here"
>    let kAuthority = "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
>
>    ```
> 1. Haga clic con el botón derecho en **Info.plist** y seleccione **Abrir como** > **Código fuente**.
> 1. En el nodo raíz dict, reemplace por su ***identificador de agrupación***:
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.Enter_the_Bundle_Id_Here</string>
>          </array>
>       </dict>
>    </array>
> 
>    ```
> 1. Compile y ejecute la aplicación. 

> [!div renderon="docs"]
>
> 1. Extraiga el archivo ZIP y abra el proyecto en XCode.
> 1. Edite **ViewController.swift** y reemplace la línea que empieza con "let kClientID" por el siguiente fragmento de código:
>
>    ```swift
>    let kClientID = "<ENTER_YOUR_APPLICATION/CLIENT_ID>"
> 
>    ```
> 1. Haga clic con el botón derecho en **Info.plist** y seleccione **Abrir como** > **Código fuente**.
> 1. En el nodo raíz dict, reemplace por su ***identificador de agrupación***:
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.<ENTER_YOUR_BUNDLE_ID></string>
>          </array>
>       </dict>
>    </array>
>
>    ```
> 1. Compile y ejecute la aplicación. 

## <a name="more-information"></a>Más información

Lea estas secciones para obtener más información sobre esta guía de inicio rápido.

### <a name="getting-msal"></a>Obtención de MSAL

MSAL ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por la Plataforma de identidad de Microsoft. Puede agregar MSAL a la aplicación mediante el proceso siguiente:

```
$ vi Podfile

```
Agregue lo siguiente a este podfile (con el destino de su proyecto):

```
use_frameworks!

target 'MSALiOS' do
   pod 'MSAL', '~> 0.4.0'
end

```

### <a name="msal-initialization"></a>Inicialización de MSAL

Puede agregar la referencia de MSAL con el código siguiente:

```swift
import MSAL
```

A continuación, realice la inicialización de MSAL con el siguiente código:

```swift
let authority = try MSALAADAuthority(url: URL(string: kAuthority)!)
            
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)

```

> |Donde: ||
> |---------|---------|
> | `clientId` | El identificador de aplicación de la aplicación registrada en *portal.azure.com* |
> | `authority` | Punto de conexión de la Plataforma de identidad de Microsoft En la mayoría de los casos será *https<span/>://login.microsoftonline.com/common* |
> | `redirectUri` | URI de redireccionamiento de la aplicación. Puede pasar "nil" para usar el valor predeterminado o su URI de redireccionamiento personalizado. |

### <a name="additional-app-requirements"></a>Requisitos adicionales de la aplicación  

La aplicación también debe tener lo siguiente en `AppDelegate`. Esto permite que el SDK de MSAL controle la respuesta del token desde la aplicación de agente de autenticación cuando realice la autenticación.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
         guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
             return false
         }
         
         return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
     }

```

Por último, la aplicación debe tener una entrada `LSApplicationQueriesSchemes` en ***Info.plist*** junto con `CFBundleURLTypes`. El ejemplo incluye esto. 

   ```xml 
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauth</string>
      <string>msauthv2</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>Inicio de sesión de usuarios y solicitud de tokens

MSAL tiene dos métodos para adquirir tokens: `acquireToken` y `acquireTokenSilent`.

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Obtención de un token de forma interactiva

Algunas situaciones requieren que los usuarios interactúen con la plataforma de identidad de Microsoft. En estos casos, puede que sea necesario que el usuario final seleccione su cuenta, escriba sus credenciales o dé su consentimiento a los permisos de la aplicación. Por ejemplo, 

* La primera vez que los usuarios inician sesión en la aplicación
* Si un usuario restablece su contraseña, deberá escribir sus credenciales. 
* Cuando la aplicación solicita acceso a un recurso por primera vez.
* Cuando se requieren MFA u otras directivas de acceso condicional.

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
applicationContext.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Donde:||
> |---------|---------|
> | `scopes` | Contiene los ámbitos que se solicitan (es decir, `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para las API web personalizadas `api://<Application ID>/access_as_user`) |

#### <a name="acquiretokensilent-getting-an-access-token-silently"></a>acquireTokenSilent: Obtención de un token de acceso de forma automática

Las aplicaciones no requieren que sus usuarios inicien sesión cada vez que soliciten un token. Si el usuario ya ha iniciado sesión, este método permite que las aplicaciones soliciten tokens de forma silenciosa. 

```swift
let parameters = MSALSilentTokenParameters(scopes: kScopes, account: applicationContext.allAccounts().first)
applicationContext.acquireTokenSilent(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Donde: ||
> |---------|---------|
> | `scopes` | Contiene los ámbitos que se solicitan (es decir, `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para las API web personalizadas `api://<Application ID>/access_as_user`) |
> | `account` | La cuenta para la que se solicita un token. Este inicio rápido es una aplicación de una cuenta única. Si quiere compilar una aplicación de varias cuentas, deberá definir una lógica para identificar qué cuenta usar para las solicitudes de token `applicationContext.account(forHomeAccountId: self.homeAccountId)` |

## <a name="next-steps"></a>Pasos siguientes

Pruebe el tutorial de iOS para ver una guía completa paso a paso sobre cómo compilar aplicaciones, incluida una explicación exhaustiva de este inicio rápido.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Más información sobre los pasos para crear la aplicación que se usa en esta guía de inicio rápido

> [!div class="nextstepaction"]
> [Tutorial de iOS de Graph API de llamada](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]