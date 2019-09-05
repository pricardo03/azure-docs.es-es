---
title: 'Aplicación móvil que llama a las API web (configuración del código): Plataforma de identidad de Microsoft | Azure'
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ebf524d932322fa08729f229a451afe656900d5
ms.sourcegitcommit: 388c8f24434cc96c990f3819d2f38f46ee72c4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70061402"
---
# <a name="mobile-app-that-calls-web-apis---code-configuration"></a>Aplicación móvil que llama a las API web: configuración de código

Una vez creada la aplicación, aprenderá a configurar el código con los parámetros de registro de la aplicación. Las aplicaciones móviles también tienen algunas características complejas, relacionadas con la instalación de la plataforma que se usa para compilar estas aplicaciones.

## <a name="msal-libraries-supporting-mobile-apps"></a>Bibliotecas MSAL que admiten aplicaciones móviles

Las bibliotecas de Microsoft que admiten aplicaciones móviles son:

  Biblioteca MSAL | DESCRIPCIÓN
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Para desarrollar aplicaciones portátiles. Las plataformas compatibles con MSAL.NET para compilar una aplicación móvil son UWP, Xamarin.iOS y Xamarin.Android.
  ![MSAL.iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL.iOS | Para desarrollar aplicaciones de iOS nativas con Objective-C o SWIFT
  ![MSAL.Android](media/sample-v2-code/logo_android.png) <br/> MSAL.Android | Para desarrollar aplicaciones nativas de Android en Java para Android

## <a name="configuring-the-application"></a>Configuración de la aplicación

Las aplicaciones móviles usan la clase `PublicClientApplication`. Aquí se muestra cómo crear una instancia:

### <a name="android"></a>Android

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

```swift
// Initialize the app.
guard let authorityURL = URL(string: kAuthority) else {
    self.loggingText.text = "Unable to create authority URL"
    return
}
let authority = try MSALAADAuthority(url: authorityURL)
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
}
```

### <a name="xamarin-or-uwp"></a>Xamarin o UWP

En el siguiente párrafo se explica cómo configurar el código de la aplicación para las aplicaciones de Xamarin.iOS, Xamarin.Android y UWP. El primer paso es crear una instancia de la aplicación. Un paso opcional es configurar el agente.

#### <a name="instantiating-the-application"></a>Creación de instancias de la aplicación

En Xamarin o UWP, la manera más sencilla de crear una instancia de la aplicación es la siguiente, donde `ClientId` es el GUID de la aplicación registrada.

```CSharp
var app = PublicClientApplicationBuilder.Create(clientId)
                                        .Build();
```

Hay otros métodos con *parámetros* que establecen el elemento primario de la interfaz de usuario, invalidan la autoridad predeterminada, especifican un nombre de cliente y una versión (para la telemetría), especifican un URI de redirección, especifican el generador HTTP que se va a usar (por ejemplo, para controlar servidores proxy, especifique la telemetría y el registro). Este es el tema de los párrafos siguientes.

##### <a name="specifying-the-parent-uiwindowactivity"></a>Especificación de la ventana, actividad o interfaz de usuario primaria

En Android, debe pasar la actividad primaria antes de realizar la autenticación interactiva. En iOS, al usar un agente, debe pasar ViewController. Del mismo modo en UWP, puede que quiera pasar la ventana primaria. Esto es posible al adquirir el token, pero también es posible especificar una devolución de llamada al crear la aplicación si un delegado devuelve UIParent.

```CSharp
IPublicClientApplication application = PublicClientApplicationBuilder.Create(clientId)
  .ParentActivityOrWindowFunc(() => parentUi)
  .Build();
```

En Android, le recomendamos que use `CurrentActivityPlugin` [aquí](https://github.com/jamesmontemagno/CurrentActivityPlugin).  A continuación, el código del compilador `PublicClientApplication` tendrá el siguiente aspecto:

```CSharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

##### <a name="more-app-building-parameters"></a>Más parámetros de compilación de aplicaciones

- Para obtener la lista de todos los modificadores disponibles en `PublicClientApplicationBuilder`, consulte la documentación de referencia [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods).
- Para obtener la descripción de todas las opciones que se muestran en `PublicClientApplicationOptions`, vea [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions) en la documentación de referencia.

#### <a name="xamarin-ios-specific-considerations"></a>Consideraciones específicas de Xamarin iOS

En Xamarin iOS, hay varias consideraciones que debe tener en cuenta al usar MSAL.NET:

1. [Reemplazo e implementación de la función `OpenUrl` en `AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
1. [Habilitación de grupos de la cadena de claves](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
1. [Habilitación del uso compartido de caché de tokens](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
1. [Habilitación de acceso a la cadena de claves](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Los detalles se proporcionan en las [consideraciones de Xamarin iOS](msal-net-xamarin-ios-considerations.md)

#### <a name="other-xamarin-android-specific-considerations"></a>Otras consideraciones específicas de Xamarin Android

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

### <a name="why-use-brokers-on-xamarinios-and-xamarinandroid-applications"></a>¿Por qué usar agentes en las aplicaciones de Xamarin.iOS y Xamarin.Android?

En Android e iOS, los agentes permiten:

- Inicio de sesión único (SSO). Los usuarios no tendrán que iniciar sesión en cada aplicación.
- Identificación de dispositivos. Habilita directivas de acceso condicional relacionadas con el dispositivo de Azure AD mediante el acceso al certificado del dispositivo que se creó en el dispositivo al unirse al área de trabajo.
- Comprobación de identificación de la aplicación. Cuando una aplicación llama al agente, pasa su dirección URL de redireccionamiento y el agente la comprueba.

### <a name="enable-the-brokers-on-xamarin"></a>Habilitar los agentes en Xamarin

Para habilitar una de estas características, use el parámetro `WithBroker()` al llamar al método`PublicClientApplicationBuilder.CreateApplication`. `.WithBroker()` se establece en true de forma predeterminada. Siga estos pasos para [iOS](#brokered-authentication-for-xamarinios).

### <a name="brokered-authentication-for-xamarinios"></a>Autenticación asincrónica para Xamarin.iOS

Siga estos pasos para permitir que la aplicación de Xamarin.iOS pueda hablar con la aplicación [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458).

#### <a name="step-1-enable-broker-support"></a>Paso 1: Habilitar la compatibilidad con el agente

La compatibilidad con el agente se habilita de forma `PublicClientApplication`. De forma predeterminada, está deshabilitada. Debe usar el parámetro `WithBroker()` (establecido en true de forma predeterminada) al crear `PublicClientApplication` con `PublicClientApplicationBuilder`.

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

#### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>Paso 2: Actualizar AppDelegate para controlar la devolución de llamada

Cuando MSAL.NET llama al agente, el agente, a su vez, volverá a llamar a la aplicación con el método `AppDelegate.OpenUrl`. Como MSAL esperará la respuesta del agente, la aplicación debe cooperar para volver a llamar a MSAL.NET. Para ello, actualice el archivo `AppDelegate.cs` para reemplazar el método siguiente.

```CSharp
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
```CSharp
   public static object RootViewController { get; set; }
```
En `AppDelegate.cs`:
```CSharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
En la llamada al token de adquisición:
```CSharp
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

**Agregue** **`msauthv2`** a la sección `LSApplicationQueriesSchemes` del archivo `Info.plist`.

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="brokered-authentication-for-xamarinandroid"></a>Autenticación asincrónica para Xamarin.Android

MSAL.NET todavía no es compatible con los agentes para Android.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquirir un token](scenario-mobile-acquire-token.md)
