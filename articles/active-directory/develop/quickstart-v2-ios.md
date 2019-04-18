---
title: Inicio rápido de la Plataforma de identidad de Microsoft para iOS | Azure
description: Obtenga información sobre cómo iniciar sesión por los usuarios y consultar Microsoft Graph en una aplicación nativa de iOS.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6340e0f349d66ecf6baaca481722396a6d786c5
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59496136"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-native-app"></a>Inicio rápido: Hacer que los usuarios inicien sesión y llamar a Microsoft Graph API desde una aplicación nativa de iOS

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido se incluye un código de ejemplo que muestra cómo una aplicación de iOS puede iniciar sesión en cuentas personales, profesionales y educativas, obtener un token de acceso y llamar a Microsoft Graph API.

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-ios/ios-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download"></a>Registro y descarga
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Registro y configuración de una aplicación y el código de ejemplo
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
> Para registrar la aplicación y agregar la información de registro de aplicación a la solución, siga estos pasos:
> 1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registrar una aplicación.
> 1. En el cuadro**Application Name** (Nombre de la aplicación) escriba el nombre de su aplicación.
> 1. Asegúrese de que la casilla **Guided Setup** (Instalación guiada) esté desactivada y, luego, seleccione **Create** (Crear).
> 1. Seleccione **Add Platform** (Agregar plataforma), seleccione **Native Application** (Aplicación nativa) y, luego, seleccione **Save** (Guardar).

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>Paso 1: Configuración de la aplicación
> Para que el código de ejemplo de este inicio rápido funcione, debe agregar una dirección URL de respuesta como `msal<AppId>://auth` (donde msal\<AppId es el identificador de esta aplicación).
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-ios/green-check.png) La aplicación está configurada con este atributo

#### <a name="step-2-download-your-web-server-or-project"></a>Paso 2: Descarga del servidor web o proyecto

- [Descarga del proyecto de XCode](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Paso 3: Configuración del proyecto

1. Extraiga el archivo ZIP y abra el proyecto en XCode.
1. Edite **ViewController.swift** y reemplace la línea que empieza con "let kClientID" por el siguiente fragmento de código:

    > [!div renderon="portal" class="sxs-lookup"]
    > ```swift
    > let kClientID = "Enter_the_Application_Id_here"
    > ```

    > [!div renderon="docs"]
    > ```swift
    > let kClientID = "<ENTER_THE_APPLICATION_ID_HERE>"
    > ```   
1. Presione la tecla CTRL y haga clic en **Info.plist** para mostrar el menú contextual y, a continuación, seleccione **Abrir como** > **Código fuente**.
1. Agregue el siguiente código bajo el nodo raíz dict:

    > [!div renderon="portal" class="sxs-lookup"]
    > ```xml
    > <key>CFBundleURLTypes</key>
    > <array>
    >     <dict>
    >         <key>CFBundleTypeRole</key>
    >         <string>Editor</string>
    >         <key>CFBundleURLName</key>
    >         <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
    >         <key>CFBundleURLSchemes</key>
    >         <array>
    >             <string>msalEnter_the_Application_Id_here</string>
    >         </array>
    >     </dict>
    > </array>
    > ```

    > [!div renderon="docs"]
    > ```xml
    > <key>CFBundleURLTypes</key>
    > <array>
    >     <dict>
    >         <key>CFBundleTypeRole</key>
    >         <string>Editor</string>
    >         <key>CFBundleURLName</key>
    >         <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
    >         <key>CFBundleURLSchemes</key>
    >         <array>
    >             <string>msal<ENTER_THE_APPLICATION_ID_HERE></string>
    >         </array>
    >     </dict>
    > </array>
    > ```
    
> [!div renderon="docs"]
> <span>5.</span> Reemplace `<ENTER_THE_APPLICATION_ID_HERE>` por el *id. de aplicación* de su aplicación. Si necesita encontrar el *id. de aplicación*, vaya a la página *Información general*.

## <a name="more-information"></a>Más información

Lea estas secciones para obtener más información sobre esta guía de inicio rápido.

### <a name="msal"></a>MSAL

MSAL ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para obtener acceso a una API protegida por Microsoft Azure Active Directory. Puede agregar MSAL a la aplicación mediante el proceso siguiente:

```
$ vi Podfile
```
Agregue lo siguiente a este podfile:

```
 target 'QuickStart' do
   use_frameworks!
 pod 'MSAL'
 end
```

### <a name="msal-initialization"></a>Inicialización de MSAL

Puede agregar la referencia de MSAL con el código siguiente:

```swift
import MSAL
```

A continuación, realice la inicialización de MSAL con el siguiente código:

```swift
let authority = MSALAuthority(url: URL(string: kAuthority)!)
self.applicationContext = try MSALPublicClientApplication(clientId: kClientID, authority: authority)
```

> |Donde: ||
> |---------|---------|
> | `clientId` | El identificador de aplicación de la aplicación registrada en *portal.azure.com* |
> | `authority` | Punto de conexión de la Plataforma de identidad de Microsoft En la mayoría de los casos será *https<span/>://login.microsoftonline.com/common* |

### <a name="requesting-tokens"></a>Solicitud de tokens

MSAL tiene dos métodos para adquirir tokens: `acquireToken` y `acquireTokenSilent`.

#### <a name="getting-an-access-token-interactively"></a>Obtención de un token de acceso interactivamente

En algunas situaciones, es necesario forzar a los usuarios a que interactúen con el punto de conexión de la Plataforma de identidad de Microsoft, lo que cambiará de contexto al explorador del sistema, ya sea para validar las credenciales del usuario o para obtener su consentimiento. Estos son algunos ejemplos:

* La primera vez que los usuarios inician sesión en la aplicación
* Cuando los usuarios deben volver a escribir sus credenciales porque la contraseña expiró
* Cuando la aplicación solicita acceso a un recurso para el cual el usuario necesita dar su consentimiento
* Cuando se requiere la autenticación en dos fases

```swift
applicationContext.acquireToken(forScopes: self.kScopes) { (result, error) in /* Add your handling logic */}
```

> |Donde:||
> |---------|---------|
> | `forScopes` | Contiene los ámbitos que se solicitan (es decir, `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para las API web personalizadas; por ejemplo, `api://<Application ID>/access_as_user`). |

#### <a name="getting-an-access-token-silently"></a>Obtención de un token de acceso de forma automática

No desea pedirle al usuario que valide sus credenciales cada vez que necesite obtener acceso a un recurso. La mayor parte del tiempo, quiere que la renovación y adquisición de tokens ocurra sin la interacción del usuario. Puede usar el método `acquireTokenSilent` para obtener tokens que permiten obtener acceso a recursos protegidos después del método `acquireToken` inicial:

```swift
applicationContext.acquireTokenSilent(forScopes: self.kScopes, account: applicationContext.allAccounts().first) { (result, error) in /* Add your handling logic */}
```

> |Donde: ||
> |---------|---------|
> | `forScopes` | Contiene los ámbitos que se solicitan (es decir, `[ "user.read" ]` para Microsoft Graph o `[ "<Application ID URL>/scope" ]` para las API web personalizadas; por ejemplo, `api://<Application ID>/access_as_user`). |
> | `account` | La cuenta que solicita el token (MSAL admite varias cuentas en una única aplicación). En el caso de esta guía de inicio rápido, el valor apunta a la primera cuenta en la memoria caché (`applicationContext.allAccounts().first`). |

## <a name="next-steps"></a>Pasos siguientes

Visite el tutorial de iOS para acceder a una guía completa paso a paso sobre la creación de aplicaciones y nuevas características, que incluye una explicación completa de esta guía de inicio rápido.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Más información sobre los pasos para crear la aplicación que se usa en esta guía de inicio rápido

> [!div class="nextstepaction"]
> [Tutorial de iOS de Graph API de llamada](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
