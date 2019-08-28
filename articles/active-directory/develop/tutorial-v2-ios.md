---
title: 'Introducción a iOS: plataforma de identidad de Microsoft | Azure'
description: Aprenda la forma en que las aplicaciones de iOS (Swift) pueden llamar a una API que requiera tokens de acceso mediante la plataforma de identidad de Microsoft.
services: active-directory
documentationcenter: dev-center-name
author: tylermsft
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 08/14/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2c9afa63a26b7ac990867517f44825054b4c5436
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69512399"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>Hacer que los usuarios inicien sesión y llamar a Microsoft Graph desde una aplicación de iOS

En este tutorial, aprenderá a integrar una aplicación iOS con la plataforma de identidad de Microsoft. La aplicación iniciará la sesión de un usuario, obtendrá un token de acceso para llamar a Microsoft Graph API y realizará una solicitud a Microsoft Graph API.  

Cuando haya completado la guía, la aplicación podrá aceptar inicios de sesión de cuentas Microsoft personales (como outlook.com, live.com y otras), y cuentas profesionales y educativas de cualquier empresa u organización que utilice Azure Active Directory.

## <a name="how-this-tutorial-works"></a>Funcionamiento de este tutorial

![Muestra cómo funciona la aplicación de ejemplo generada por este tutorial](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

La aplicación de este tutorial iniciará la sesión de los usuarios y obtendrá datos en su nombre.  A estos datos se accederá a través de una API protegida (Microsoft Graph API en este caso) que requiere autorización y que está protegida por la plataforma de identidad de Microsoft.

Más concretamente:

* La aplicación iniciará la sesión del usuario mediante un explorador o Microsoft Authenticator.
* El usuario final aceptará los permisos que la aplicación ha solicitado.
* Se emitirá un token de acceso a la aplicación para Microsoft Graph API.
* El token de acceso se incluirán en la solicitud HTTP a la API web.
* Se procesa la respuesta de Microsoft Graph.

Este ejemplo utiliza la Biblioteca de autenticación de Microsoft (MSAL) para implementar la autenticación. MSAL renovará automáticamente los tokens, proporcionará el inicio de sesión único entre las otras aplicaciones del dispositivo y administrará las cuentas.

## <a name="prerequisites"></a>Requisitos previos

- Para compilar la aplicación de esta guía, se necesita la versión 10.x de XCode. Puede descargar XCode desde [el sitio web de iTunes](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode Download URL").
- Biblioteca de autenticación de Microsoft ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Puede usar un administrador de dependencias o agregar la biblioteca manualmente. Las instrucciones siguientes muestran cómo hacerlo.

En este tutorial, vamos a crear un nuevo proyecto. Si desea descargar el tutorial completado en su lugar, [descargue el código](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip).

## <a name="create-a-new-project"></a>Creación de un nuevo proyecto

1. Abra Xcode y seleccione **Create a new Xcode project** (Crear un nuevo proyecto en Xcode).
2. Seleccione **iOS** > **Aplicación de vista única** y seleccione **Siguiente**.
3. Especifique un nombre de producto.
4. En **Lenguaje**, seleccione **Swift** y seleccione **Siguiente**.
5. Seleccione una carpeta para crear la aplicación y haga clic en **Crear**.

## <a name="register-your-application"></a>Registrar su aplicación

1. Vaya a [Azure Portal](https://aka.ms/MobileAppReg).
2. Abra la [hoja de Registros de aplicaciones](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) y haga clic en **+Nuevo registro**.
3. Escriba un **nombre** para ka aplicación y, después, sin configurar un URI de redirección, haga clic en **Registrar**.
4. En la sección **Administrar** del panel que aparece, seleccione **Autenticación**.
5. Haga clic en **Probar la nueva experiencia**  cerca de la parte superior de la pantalla para abrir el registro de aplicaciones y, después, haga clic en **+Nuevo registro** >  **+ Agregar una plataforma** > **iOS**.
    - Escriba el identificador de agrupación del proyecto. Si descargó el código, es `com.microsoft.identitysample.MSALiOS`. Si va a crear su propio proyecto, seleccione el proyecto en Xcode y abra la pestaña **General**. El identificador de la agrupación aparece en la sección **Identidad**.
6. Haga clic en `Configure` y guarde el valor de **Configuración de MSAL**  que aparece en la página **Configuración de iOS** para que pueda escribirlo cuando configure la aplicación más tarde.  Haga clic en **Done**(Listo).

## <a name="add-msal"></a>Adición de MSAL

Elija una de las siguientes formas de instalar la biblioteca de MSAL en su aplicación:

### <a name="cocoapods"></a>CocoaPods

1. Si usa [CocoaPods](https://cocoapods.org/), instale `MSAL`, para lo que primero debe crear un archivo vacío llamado `podfile` en la misma carpeta que el archivo `.xcodeproj` del proyecto. Agregue lo siguiente a `podfile`:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL', '~> 0.4.0'
   end
   ```

2. Reemplace `<your-target-here>` por el nombre del proyecto.
3. En una ventana de terminal, vaya a la carpeta que contiene el archivo `podfile` que ha creado y ejecute `pod install` para instalar la biblioteca MSAL.
4. Cierre Xcode y abra `<your project name>.xcworkspace` para volver a cargar el proyecto en Xcode.

### <a name="carthage"></a>Carthage

Si usa [Carthage](https://github.com/Carthage/Carthage), instale `MSAL`, para lo que debe agregarlo a `Cartfile`:

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

En una ventana de terminal, en el mismo directorio que el archivo `Cartfile` actualizado, ejecute el siguiente comando para que Carthage actualice las dependencias del proyecto:

```bash
carthage update --platform iOS
```

### <a name="manually"></a>Manualmente

También puede usar un submódulo de Git o consultar la versión más reciente y usarla como marco en su aplicación.

## <a name="add-your-app-registration"></a>Adición del registro de aplicaciones

A continuación, agregaremos el registro de aplicaciones al código. 

En primer lugar, agregue la siguiente instrucciones de importación al principio de los archivos `ViewController.swift` y `AppDelegate.swift`:

```swift
import MSAL
```

Después agregue el siguiente código a `ViewController.swift`, antes de `viewDidLoad()`:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
let kAuthority = "https://login.microsoftonline.com/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

Modifique el valor asignado a `kClientID`para que sea el identificador de la aplicación. Este valor forma parte de los datos de Configuración de MSAL que guardó al principio de este tutorial para registrar la aplicación en Azure Portal.

## <a name="configure-url-schemes"></a>Configuración de esquemas de direcciones URL

En este paso, registrará `CFBundleURLSchemes` para que se pueda redirigir al usuario a la aplicación después de iniciar sesión. Por cierto, `LSApplicationQueriesSchemes` también permite a la aplicación hacer uso de Microsoft Authenticator.

En Xcode, abra `Info.plist` como un archivo de código fuente y agregue lo siguiente en la sección `<dict>`. Reemplace `[BUNDLE_ID]` por el valor que usó en Azure Portal que, si descargó el código, es `com.microsoft.identitysample.MSALiOS`. Si va a crear su propio proyecto, seleccione el proyecto en Xcode y abra la pestaña **General**. El identificador de la agrupación aparece en la sección **Identidad**.

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauth</string>
    <string>msauthv2</string>
</array>
```

## <a name="create-your-apps-ui"></a>Creación de la interfaz de usuario de la aplicación

Ahora cree una interfaz de usuario que incluya un botón para llamar a Microsoft Graph API, otro para cerrar la sesión y una vista de texto para parte de la salida agregando el siguiente código a la clase `ViewController`:

```swift
var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = UIButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
        callGraphButton.setTitleColor(.blue, for: .normal)
        callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
        callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
        // Add sign out button
        signOutButton = UIButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.setTitle("Sign Out", for: .normal)
        signOutButton.setTitleColor(.blue, for: .normal)
        signOutButton.setTitleColor(.gray, for: .disabled)
        signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = UITextView()
        loggingText.isUserInteractionEnabled = false
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: 10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
    }
```

A continuación, también en la clase `ViewController`, reemplace el método `viewDidLoad()` por:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }
```

## <a name="use-msal"></a>Uso de MSAL

### <a name="initialize-msal"></a>Inicializar MSAL

Agregue el siguiente método `InitMSAL` a la clase `ViewController`:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.loggingText.text = "Unable to create authority URL"
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
    }
```

### <a name="handle-the-sign-in-callback"></a>Control de la devolución de llamada del inicio de sesión

Abra el archivo `AppDelegate.swift` . Para controlar la devolución de llamada después de iniciar sesión, agregue `MSALPublicClientApplication.handleMSALResponse` a la clase `appDelegate` de la siguiente forma:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>Adquisición de tokens

Ahora, podemos implementar la lógica de procesamiento de la interfaz de usuario de la aplicación y obtener los tokens interactivamente a través de MSAL.

MSAL expone dos métodos principales para obtener tokens: `acquireTokenSilently()` y `acquireTokenInteractively()`: 

- `acquireTokenSilently()` intenta iniciar la sesión de un usuario y obtener tokens sin interacción del usuario, siempre que haya una cuenta.

- `acquireTokenInteractively()` siempre muestra la interfaz de usuario al intentar iniciar la sesión del usuario. Puede utilizar las cookies de sesión en el explorador o una cuenta de Microsoft Authenticator para proporcionar una experiencia de inicio de sesión único interactivo.

Agregue el siguiente código a la clase `ViewController`:

```swift
    @objc func callGraphAPI(_ sender: UIButton) {
        
        guard let currentAccount = self.currentAccount() else {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            acquireTokenInteractively()
            return
        }
        
        acquireTokenSilently(currentAccount)
    }

    func currentAccount() -> MSALAccount? {
        
        guard let applicationContext = self.applicationContext else { return nil }
        
        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        
        do {
            let cachedAccounts = try applicationContext.allAccounts()
            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }
        } catch let error as NSError {
            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }
        
        return nil
    }
```

#### <a name="get-a-token-interactively"></a>Obtención de un token interactivamente

El código siguiente obtiene un token por primera vez mediante la creación de un objeto `MSALInteractiveTokenParameters` y la llamada a `acquireToken`. A continuación, agregará código que:

1. Crea `MSALInteractiveTokenParameters` con ámbitos.
2. Llama a `acquireToken()` con los parámetros creados.
3. Controla los errores. Para obtener más información, consulte la [guía de control de errores de iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling).
4. Controla el caso de éxito.

Agregue el siguiente código a la clase `ViewController` .

```swift
    func acquireTokenInteractively() {
   
        guard let applicationContext = self.applicationContext else { return }
     // #1    
        let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
     // #2        
        applicationContext.acquireToken(with: parameters) { (result, error) in
     // #3            
            if let error = error {
                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }
            guard let result = result else {   
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
     // #4            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```



#### <a name="get-a-token-silently"></a>Obtención de un token en silencio

Para adquirir un token actualizado en modo silencioso, agregue el código siguiente a la clase`ViewController`. Crea un objeto `MSALSilentTokenParameters` y llama a `acquireTokenSilent()`:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        guard let applicationContext = self.applicationContext else { return }
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in    
            if let error = error {
                let nsError = error as NSError
                if (nsError.domain == MSALErrorDomain) {
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>Llamar a Microsoft Graph API 

Una vez que tenga un token, la aplicación puede usarlo este valor en el encabezado de HTTP para realizar una solicitud autorizada a Microsoft Graph:

| clave del encabezado    | value                 |
| ------------- | --------------------- |
| Authorization | Portador \<token_de_acceso> |

Agregue el siguiente código a la clase `ViewController`:

```swift
    func getContentWithToken() {        
        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
               
        URLSession.shared.dataTask(with: request) { data, response, error in
               
        if let error = error {
            self.updateLogging(text: "Couldn't get graph result: \(error)")
            return
        }
               
        guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
               
        self.updateLogging(text: "Couldn't deserialize result JSON")
            return
        }
               
        self.updateLogging(text: "Result from Graph: \(result))")
        
        }.resume()
    }
```

Consulte [Microsoft Graph API](https://graph.microsoft.com) para más información acerca de Microsoft Graph API.

### <a name="use-msal-for-sign-out"></a>Uso de MSAL para cierre de sesión

A continuación, agregue compatibilidad para el cierre de sesión.

> [!Important]
> Si cierra sesión con MSAL, se elimina de esta aplicación toda la información conocida sobre un usuario, pero el usuario seguirá teniendo una sesión activa en su dispositivo. Si el usuario intenta iniciar sesión nuevamente, puede ver la interfaz de usuario de inicio de sesión, pero es posible que no tenga que volver a escribir las credenciales debido a que la sesión del dispositivo sigue activa.

Para agregar la funcionalidad de cierre de sesión, agregue el siguiente código a la clase `ViewController`. Este método recorre en iteración todas las cuentas y las quita:

```swift 
    @objc func signOut(_ sender: UIButton) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache */
            
            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signOutButton.isEnabled = false
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>Habilitación del almacenamiento en caché de tokens

De forma predeterminada, MSAL almacena en caché los tokens de la aplicación en la aplicación Llaveros de iOS. 

Para habilitar el almacenamiento en caché de tokens:
1. Vaya a la configuración del proyecto de Xcode > **Pestaña Capabilities** (Funcionalidades) > **Enable Keychain Sharing** (Habilitar uso compartido de la cadena de claves)
2. Haga clic en **+** y escriba`com.microsoft.adalcache` como entrada de **Keychain Groups** (Grupos de cadenas de claves).

### <a name="add-helper-methods"></a>Adición de métodos auxiliares

Agregue los siguientes métodos auxiliares a la clase `ViewController` para completar el ejemplo:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
```

### <a name="multi-account-applications"></a>Aplicaciones de varias cuentas

Esta aplicación se ha creado para un escenario de una única cuenta. MSAL también admite escenarios de varias cuentas, pero requiere algún trabajo adicional por parte de las aplicaciones. Deberá crear una interfaz de usuario para ayudar al usuario a seleccionar qué cuenta desea usar para cada acción que requiera tokens. Como alternativa, la aplicación puede implementar un método heurístico para seleccionar qué cuenta se usará mediante el método `getAccounts()`.

## <a name="test-your-app"></a>Prueba de la aplicación

### <a name="run-locally"></a>Ejecución en modo local

Cree e implemente la aplicación en un emulador o dispositivo de prueba. Debe ser capaz de iniciar sesión y obtener tokens para Azure AD o cuentas personales de Microsoft.

La primera vez que un usuario inicie sesión en la aplicación, Microsoft Identity le pedirá que acepte los permisos solicitados.  Aunque la mayoría de los usuarios pueden dar su consentimiento, algunos inquilinos de Azure AD tienen el consentimiento del usuario deshabilitado, lo que requiere que los administradores den su consentimiento en nombre de todos los usuarios. Para admitir este escenario, registre los ámbitos de la aplicación en Azure Portal.

Después de iniciar sesión, esta aplicación mostrará los datos devueltos desde el punto de conexión `/me` de Microsoft Graph.

## <a name="get-help"></a>Obtención de ayuda

Visite [Ayuda y soporte técnico](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) si tienes algún problema con este tutorial o con la plataforma de identidad de Microsoft.

Ayúdenos a mejorar la Plataforma de identidad de Microsoft. Rellene una breve encuesta de dos preguntas y háganos saber su opinión.

> [!div class="nextstepaction"]
> [Encuesta sobre la Plataforma de identidad de Microsoft](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)