---
title: 'Configuración de aplicaciones móviles que llaman a las API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web (configuración del código de la aplicación)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 61d3a7b7f06ffdb7f39f80f7f3bf19e9007945d2
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702120"
---
# <a name="mobile-app-that-calls-web-apis---code-configuration"></a>Aplicación móvil que llama a las API web: configuración de código

Una vez creada la aplicación, aprenderá a configurar el código con los parámetros de registro de la aplicación. Las aplicaciones móviles también tienen algunas características complejas, relacionadas con la instalación de la plataforma que se usa para compilar estas aplicaciones.

## <a name="msal-libraries-supporting-mobile-apps"></a>Bibliotecas MSAL que admiten aplicaciones móviles

Las bibliotecas de Microsoft que admiten aplicaciones móviles son:

  Biblioteca MSAL | Descripción
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Para desarrollar aplicaciones portátiles. Las plataformas compatibles con MSAL.NET para compilar una aplicación móvil son UWP, Xamarin.iOS y Xamarin.Android.
  ![MSAL.iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL.iOS | Para desarrollar aplicaciones de iOS nativas con Objective-C o Swift
  ![MSAL.Android](media/sample-v2-code/logo_android.png) <br/> MSAL.Android | Para desarrollar aplicaciones nativas de Android en Java para Android

## <a name="instantiating-the-application"></a>Creación de instancias de la aplicación

### <a name="android"></a>Android

Las aplicaciones móviles usan la clase `PublicClientApplication`. Aquí se muestra cómo crear una instancia:

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

Las aplicaciones móviles de iOS necesitan crear una instancia de la clase `MSALPublicClientApplication`.

Objective-C:

```objc
NSError *msalError = nil;
     
MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
```

Swift:
```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
if let application = try? MSALPublicClientApplication(configuration: config){ /* Use application */}
```

Hay [propiedades MSALPublicClientApplicationConfig adicionales](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALPublicClientApplicationConfig.html#/Configuration%20options) que pueden invalidar la autoridad predeterminada, especificar un URI de redirección o cambiar el comportamiento del almacenamiento en caché del token de MSAL. 

### <a name="xamarin-or-uwp"></a>Xamarin o UWP

En el siguiente párrafo se explica cómo crear instancias de la aplicación para las aplicaciones de Xamarin.iOS, Xamarin.Android y UWP.

#### <a name="instantiating-the-application"></a>Creación de instancias de la aplicación

En Xamarin o UWP, la manera más sencilla de crear una instancia de la aplicación es la siguiente, donde `ClientId` es el GUID de la aplicación registrada.

```csharp
var app = PublicClientApplicationBuilder.Create(clientId)
                                        .Build();
```

Hay otros métodos con *parámetros* que establecen el elemento primario de la interfaz de usuario, invalidan la autoridad predeterminada, especifican un nombre de cliente y una versión (para la telemetría), especifican un URI de redirección, especifican el generador HTTP que se va a usar (por ejemplo, para controlar servidores proxy, especifique la telemetría y el registro). Este es el tema de los párrafos siguientes.

##### <a name="specifying-the-parent-uiwindowactivity"></a>Especificación de la ventana, actividad o interfaz de usuario primaria

En Android, debe pasar la actividad primaria antes de realizar la autenticación interactiva. En iOS, al usar un agente, debe pasar ViewController. Del mismo modo en UWP, puede que quiera pasar la ventana primaria. Esto es posible al adquirir el token, pero también es posible especificar una devolución de llamada al crear la aplicación si un delegado devuelve UIParent.

```csharp
IPublicClientApplication application = PublicClientApplicationBuilder.Create(clientId)
  .ParentActivityOrWindowFunc(() => parentUi)
  .Build();
```

En Android, le recomendamos que usar el objeto `CurrentActivityPlugin` [aquí](https://github.com/jamesmontemagno/CurrentActivityPlugin).  A continuación, el código del compilador `PublicClientApplication` tendrá el siguiente aspecto:

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

##### <a name="more-app-building-parameters"></a>Más parámetros de compilación de aplicaciones

- Para obtener la lista de todos los modificadores disponibles en `PublicClientApplicationBuilder`, consulte la documentación de referencia [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods).
- Para obtener la descripción de todas las opciones que se muestran en `PublicClientApplicationOptions`, vea [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions) en la documentación de referencia.

## <a name="xamarin-ios-specific-considerations"></a>Consideraciones específicas de Xamarin iOS

En Xamarin iOS, hay varias consideraciones que debe tener en cuenta al usar MSAL.NET:

1. [Reemplazo e implementación de la función `OpenUrl` en `AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
1. [Habilitación de grupos de la cadena de claves](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
1. [Habilitación del uso compartido de caché de tokens](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
1. [Habilitación de acceso a la cadena de claves](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Los detalles se proporcionan en las [consideraciones de Xamarin iOS](msal-net-xamarin-ios-considerations.md)

## <a name="msal-for-ios-and-macos-specific-considerations"></a>Consideraciones específicas de MSAL para iOS y macOS

Se aplican consideraciones similares al usar MSAL para iOS y macOS:

1. [Implementación de la devolución de llamada `openURL`](#brokered-authentication-for-msal-for-ios-and-macos)
2. [Habilitación de los grupos de acceso al llavero](howto-v2-keychain-objc.md)
3. [Personalización de los exploradores y las vistas web](customize-webviews.md)

## <a name="xamarin-android-specific-considerations"></a>Consideraciones específicas de Xamarin Android

Estos son los detalles específicos de Xamarin Android:

- [Devolución del control a MSAL una vez que finaliza la parte interactiva del flujo de autenticación](msal-net-xamarin-android-considerations.md#ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends)
- [Actualización del manifiesto de Android](msal-net-xamarin-android-considerations.md#update-the-android-manifest)
- [Uso de la vista web incrustada (opcional)](msal-net-xamarin-android-considerations.md#use-the-embedded-web-view-optional)
- [Solución de problemas](msal-net-xamarin-android-considerations.md#troubleshooting)

Los detalles se proporcionan en las [consideraciones de Xamarin Android](msal-net-xamarin-android-considerations.md).

Por último, hay algunas características que hay que conocer sobre los exploradores en Android. Se explican en las [consideraciones específicas de Xamarin Android con MSAL.NET](msal-net-system-browser-android-considerations.md).

#### <a name="uwp-specific-considerations"></a>Consideraciones específicas de UWP

En UWP, puede usar redes corporativas. Para obtener información sobre cómo usar la biblioteca MSAL con UWP, consulte las [consideraciones específicas de Plataforma universal de Windows con MSAL.NET](msal-net-uwp-considerations.md).

## <a name="configuring-the-application-to-use-the-broker"></a>Configuración de la aplicación para usar el agente

### <a name="why-use-brokers-in-ios-and-android-applications"></a>¿Por qué usar agentes en las aplicaciones para iOS y Android?

En Android e iOS, los agentes permiten:

- El inicio de sesión único (SSO) cuando el dispositivo está registrado con AAD. Los usuarios no tendrán que iniciar sesión en cada aplicación.
- Identificación de dispositivos. Habilita directivas de acceso condicional relacionadas con el dispositivo de Azure AD mediante el acceso al certificado del dispositivo que se creó en el dispositivo al unirse al área de trabajo.
- Comprobación de identificación de la aplicación. Cuando una aplicación llama al agente, pasa su dirección URL de redireccionamiento y el agente la comprueba.

### <a name="enable-the-broker-on-xamarin"></a>Habilitación del agente en Xamarin

Para habilitar una de estas características, use el parámetro `WithBroker()` al llamar al método`PublicClientApplicationBuilder.CreateApplication`. `.WithBroker()` se establece en true de forma predeterminada. Realice los siguientes pasos para [Xamarin.iOS](#brokered-authentication-for-xamarinios).

### <a name="enable-the-broker-for-msal-for-android"></a>Habilitación del agente para MSAL para Android

Consulte [Autenticación con intermediación en Android](brokered-auth.md) para información sobre cómo habilitar un agente en Android. 

### <a name="enable-the-broker-for-msal-for-ios-and-macos"></a>Habilitación del agente para MSAL para iOS y macOS

La autenticación con agente está habilitada de forma predeterminada para los escenarios de AAD en MSAL para iOS y macOS. Siga los pasos que se indican a continuación para configurar la aplicación para que admita la autenticación con agente en [MSAL para iOS y macOS](#brokered-authentication-for-msal-for-ios-and-macos). Tenga en cuenta que algunos pasos difieren de [MSAL para Xamarin.iOS](#brokered-authentication-for-xamarinios) y [MSAL para iOS y macOS](#brokered-authentication-for-msal-for-ios-and-macos).

### <a name="brokered-authentication-for-xamarinios"></a>Autenticación asincrónica para Xamarin.iOS

Siga estos pasos para permitir que la aplicación de Xamarin.iOS pueda hablar con la aplicación [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458).

#### <a name="step-1-enable-broker-support"></a>Paso 1: Habilitar la compatibilidad con el agente

La compatibilidad con el agente se habilita de forma `PublicClientApplication`. De forma predeterminada, está deshabilitada. Debe usar el parámetro `WithBroker()` (establecido en true de forma predeterminada) al crear `PublicClientApplication` con `PublicClientApplicationBuilder`.

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

#### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>Paso 2: Actualizar AppDelegate para controlar la devolución de llamada

Cuando MSAL.NET llama al agente, el agente, a su vez, volverá a llamar a la aplicación con el método `AppDelegate.OpenUrl`. Como MSAL esperará la respuesta del agente, la aplicación debe cooperar para volver a llamar a MSAL.NET. Para ello, actualice el archivo `AppDelegate.cs` para reemplazar el método siguiente.

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url,
                             string sourceApplication,
                             NSObject annotation)
{
 if (AuthenticationContinuationHelper.IsBrokerResponse(sourceApplication))
 {
  AuthenticationContinuationHelper.SetBrokerContinuationEventArgs(url);
  return true;
 }
 else if (!AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url))
 {
  return false;
 }
 return true;
}
```

Este método se invoca cada vez que se inicia la aplicación y se usa como una oportunidad para procesar la respuesta del agente y completar el proceso de autenticación iniciado por MSAL.NET.

#### <a name="step-3-set-a-uiviewcontroller"></a>Paso 3: Establecer un UIViewController()

Con Xamarin iOS, normalmente no establece ninguna ventana de objeto, pero, en este caso, lo hace para enviar y recibir respuestas de un agente. Todavía en `AppDelegate.cs`, establezca un ViewController.

Haga lo siguiente para establecer la ventana de objeto:

1) En `AppDelegate.cs`, establezca `App.RootViewController` en un nuevo `UIViewController()`. Esto garantizará que haya un `UIViewController` con la llamada al agente. Si no se establece correctamente, puede obtener este error: `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) En la llamada a AcquireTokenInteractive, use `.WithParentActivityOrWindow(App.RootViewController)` y pase la referencia a la ventana de objeto que usará.

**Por ejemplo:**

En `App.cs`:
```csharp
   public static object RootViewController { get; set; }
```
En `AppDelegate.cs`:
```csharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
En la llamada al token de adquisición:
```csharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

#### <a name="step-4-register-a-url-scheme"></a>Paso 4: Registrar un esquema de dirección URL

MSAL.NET usa direcciones URL para invocar al agente y devolver la respuesta del agente a la aplicación. Para finalizar el recorrido de ida y vuelta, debe registrar un esquema de dirección URL para la aplicación en el archivo `Info.plist`.

Anteponga a `CFBundleURLSchemes` el prefijo `msauth`. A continuación, agregue `CFBundleURLName` al final.

`$"msauth.(BundleId)"`

**Por ejemplo:** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> Este esquema de dirección URL se convertirá en parte del valor de RedirectUri que se usa para identificar de forma única la aplicación al recibir la respuesta del agente.

```XML
 <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.yourcompany.xforms</string>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.com.yourcompany.xforms</string>
        </array>
      </dict>
    </array>
```

#### <a name="step-5-lsapplicationqueriesschemes"></a>Paso 5: LSApplicationQueriesSchemes

MSAL usa `–canOpenURL:` para comprobar si el agente está instalado en el dispositivo. En iOS 9, Apple ha bloqueado los esquemas que puede consultar una aplicación.

**Agregue** **`msauthv2`** a la sección `LSApplicationQueriesSchemes` del archivo `Info.plist`.

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="brokered-authentication-for-msal-for-ios-and-macos"></a>Autenticación con agente en MSAL para iOS y macOS

La autenticación con agente está habilitada de forma predeterminada para los escenarios de AAD.

#### <a name="step-1-update-appdelegate-to-handle-the-callback"></a>Paso 1: Actualizar AppDelegate para controlar la devolución de llamada

Cuando MSAL para iOS y macOS llama al agente, este a su vez vuelve a llamar a la aplicación con el método `openURL`. Como MSAL espera la respuesta del agente, la aplicación debe cooperar para volver a llamar a MSAL. Para ello, actualice el archivo `AppDelegate.m` para reemplazar el método siguiente.

Objective-C:

```objc
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
    return [MSALPublicClientApplication handleMSALResponse:url 
                                         sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

Swift:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

Tenga en cuenta que si ha adoptado UISceneDelegate en iOS 13 +, la devolución de llamada de MSAL debe colocarse en `scene:openURLContexts:` de UISceneDelegate en su lugar (consulte la [documentación de Apple](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)). MSAL `handleMSALResponse:sourceApplication:` solo debe llamarse una vez para cada dirección URL.

#### <a name="step-2-register-a-url-scheme"></a>Paso 2: Registrar un esquema de dirección URL

MSAL para iOS y macOS usa direcciones URL para invocar al agente y devolver la respuesta de este a la aplicación. Para finalizar el recorrido de ida y vuelta, debe registrar un esquema de dirección URL para la aplicación en el archivo `Info.plist`.

Prefije el esquema de la dirección URL personalizada con `msauth`. A continuación, agregue el **identificador del lote** al final.

`msauth.(BundleId)`

**Por ejemplo:** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> Este esquema de dirección URL se convertirá en parte del valor de RedirectUri que se usa para identificar de forma única la aplicación al recibir la respuesta del agente. Asegúrese de que RedirectUri está registrado con el formato de `msauth.(BundleId)://auth` para la aplicación en [Azure Portal](https://portal.azure.com).

```XML
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-lsapplicationqueriesschemes"></a>Paso 3: LSApplicationQueriesSchemes

**Agregue `LSApplicationQueriesSchemes`** para permitir que se realice una llamada a Microsoft Authenticator si está instalado.
Tenga en cuenta que se necesita el esquema "msauthv3" al compilar la aplicación con Xcode 11 y versiones posteriores. 

```XML 
<key>LSApplicationQueriesSchemes</key>
<array>
  <string>msauthv2</string>
  <string>msauthv3</string>
</array>
```

### <a name="brokered-authentication-for-xamarinandroid"></a>Autenticación asincrónica para Xamarin.Android

MSAL.NET todavía no es compatible con los agentes para Android.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquirir un token](scenario-mobile-acquire-token.md)
