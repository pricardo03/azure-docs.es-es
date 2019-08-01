---
title: Exploradores web en la biblioteca de autenticación de Microsoft para .NET | Azure
description: Obtenga información sobre las consideraciones específicas al usar Xamarin Android con Microsoft Authentication Library for .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 061b8a9f16396841c3f0d650ccc2f2c4a907aab3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111311"
---
# <a name="using-web-browsers-in-msalnet"></a>Uso de los exploradores web en MSAL.NET
Se requieren exploradores web para la autenticación no interactiva. De forma predeterminada, MSAL.NET admite el [explorador web del sistema](#system-web-browser-on-xamarinios-and-xamarinandroid) en Xamarin.iOS y [Xamarin.Android](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/system-browser). Pero [también puede habilitar el explorador web insertado](#enable-embedded-webviews) en función de los necesidades (UX, necesidad de inicio de sesión único (SSO), seguridad) en aplicaciones [Xamarin.iOS](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinios) y [Xamarin.Android](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid). E incluso puede [elegir dinámicamente](#detecting-the-presence-of-custom-tabs-on-xamarinandroid) qué explorador web utilizar en función de la presencia de Chrome o de un explorador compatible con las pestañas personalizadas de Chrome en Android.

## <a name="web-browsers-in-msalnet"></a>Exploradores web en MSAL.NET

Es importante entender que al adquirir un token de forma interactiva, el contenido del cuadro de diálogo no lo proporciona la biblioteca sino el servicio de token de seguridad (STS). El punto de conexión de autenticación devuelve algo de HTML y JavaScript que controla la interacción, que se representa en un explorador web o control web. Permitir que el servicio de token de seguridad controle la interacción HTML tiene muchas ventajas:

- La contraseña (si se escribe una) nunca se almacena en la aplicación, ni en la biblioteca de autenticación.
- Permite redireccionar a otros proveedores de identidades (por ejemplo, iniciar sesión con una cuenta educativa o una cuenta personal con MSAL, o con una cuenta de redes sociales con Azure AD B2C).
- Permite al servicio de token de seguridad controlar el acceso condicional, por ejemplo, haciendo que el usuario realice la autenticación multifactor (MFA) durante la fase de autenticación (mediante la escritura de un pin de Windows Hello, o una llamada al teléfono, o en una aplicación de autenticación en el teléfono). En los casos en los que la autenticación multifactor requerida aún no está configurada, el usuario puede configurarla en el momento justo en el mismo cuadro de diálogo.  El usuario escribe su número de teléfono móvil y se le guía para instalar una aplicación de autenticación y escanear una etiqueta QR para agregar la cuenta. Esta interacción orientada por servidor es una gran experiencia.
- Permite al usuario cambiar la contraseña en este mismo cuadro de diálogo cuando la contraseña ha expirado (para ello, proporciona campos adicionales para la contraseña anterior y la nueva contraseña).
- Permite la personalización de marca del inquilino o de la aplicación (imágenes) controlada por el administrador de inquilinos o propietario de la aplicación de Azure AD.
- Permite a los usuarios dar su consentimiento para que la aplicación acceda a los recursos o ámbitos en su nombre justo después de la autenticación.

## <a name="system-web-browser-on-xamarinios-and-xamarinandroid"></a>Explorador web del sistema en Xamarin.iOS y Xamarin.Android

De forma predeterminada, MSAL.NET admite el explorador web del sistema en Xamarin.iOS y Xamarin.Android. Para todas las plataformas que proporcionan una interfaz de usuario (es decir, que no son .NET Core), la biblioteca proporciona un cuadro de diálogo que inserta un control del explorador web. MSAL.NET también usa una vista web insertada para el escritorio de .NET y WAB para la plataforma UWP. Sin embargo, de forma predeterminada, aprovecha el **explorador web del sistema** para aplicaciones Xamarin iOS y Xamarin Android. En iOS, incluso elige la vista web que se va a utilizar según la versión del sistema operativo (iOS12, iOS11 y anteriores).

El uso del explorador del sistema tiene la ventaja significativa de compartir el estado de inicio de sesión único con otras aplicaciones y con aplicaciones web sin necesidad de un agente (portal de empresa o autenticador). De manera predeterminada, el explorador del sistema se utilizó en MSAL.NET para las plataformas Xamarin iOS y Xamarin Android porque, en estas plataformas, el explorador del sistema ocupa toda la pantalla y la experiencia del usuario es mejor. La vista web del sistema no se distingue de una ventana de diálogo. En iOS, sin embargo, es posible que el usuario tenga que dar su consentimiento para que el explorador vuelva a llamar a la aplicación, lo que puede resultar molesto.

### <a name="uwp-does-not-use-the-system-webview"></a>UWP no utiliza la vista web del sistema

Sin embargo, en el caso de las aplicaciones de escritorio, el inicio de una vista web del sistema conduce a una experiencia de usuario secundaria, ya que el usuario ve el explorador, donde es posible que ya tenga otras pestañas abiertas. Y cuando la autenticación ha ocurrido, los usuarios obtienen una página en la que les solicita que cierren esta ventana. Si el usuario no presta atención, puede cerrar todo el proceso (incluidas otras pestañas que no están relacionadas con la autenticación). Aprovechar el explorador del sistema en el escritorio también requeriría abrir puertos locales y escuchar en ellos, lo que podría requerir permisos avanzados para la aplicación. Usted, como desarrollador, usuario o administrador, puede ser reacio a este requisito.

## <a name="enable-embedded-webviews"></a>Habilitación de vistas web insertadas 
También puede habilitar vistas web insertadas en aplicaciones Xamarin.iOS y Xamarin.Android. A partir de MSAL.NET 2.0.0-preview, MSAL.NET también admite el uso de la opción de vista web **insertada**. Para ADAL.NET, la vista web insertada es la única opción que se admite.

Como desarrollador que utiliza MSAL.NET con Xamarin como destino, puede optar por utilizar vistas web integradas o exploradores del sistema. Esta es su elección en función de la experiencia del usuario y de las preocupaciones de seguridad a las que desea dirigirse.

Actualmente, MSAL.NET todavía no admite los agentes de iOS y Android. Por lo tanto, si necesita proporcionar un inicio de sesión único (SSO), el explorador del sistema podría ser una mejor opción. La compatibilidad de los agentes con el explorador web insertado se encuentra todavía pendiente en MSAL.NET.

### <a name="differences-between-embedded-webview-and-system-browser"></a>Diferencias entre la vista web insertada y el explorador del sistema 
Hay algunas diferencias visuales entre la vista web insertada y el explorador del sistema en MSAL.NET.

**Inicio de sesión interactivo con MSAL.NET mediante la vista web insertada:**

![insertada](media/msal-net-web-browsers/embedded-webview.png)

**Inicio de sesión interactivo con MSAL.NET mediante el explorador del sistema:**

![Explorador del sistema](media/msal-net-web-browsers/system-browser.png)

### <a name="developer-options"></a>Opciones del desarrollador

Como desarrollador que utiliza MSAL.NET, tiene varias opciones para mostrar el diálogo interactivo de STS:

- **Explorador del sistema.** El explorador del sistema se establece de forma predeterminada en la biblioteca. Si utiliza Android, lea sobre los [exploradores del sistema](msal-net-system-browser-android-considerations.md) para obtener información específica sobre los exploradores compatibles con la autenticación. Al utilizar el explorador del sistema en Android, se recomienda que el dispositivo tenga un explorador compatible con las pestañas personalizadas de Chrome.  En caso contrario, puede dar lugar a un error de autenticación.
- **Vista web insertada.** Para utilizar solo la vista web insertada en MSAL.NET, el generador de parámetros `AcquireTokenInteractively` contiene el método `WithUseEmbeddedWebView()`.

    iOS

    ```csharp
    AuthenticationResult authResult;
    authResult = app.AcquireTokenInteractively(scopes)
                    .WithUseEmbeddedWebView(useEmbeddedWebview)
                    .ExecuteAsync();
    ```

    Android:

    ```csharp
    authResult = app.AcquireTokenInteractively(scopes)
                .WithParentActivityOrWindow(activity)
                .WithUseEmbeddedWebView(useEmbeddedWebview)
                .ExecuteAsync();
    ```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinios"></a>Elección entre el explorador web insertado o el explorador del sistema en Xamarin.iOS

En la aplicación de iOS en `AppDelegate.cs` se puede inicializar `ParentWindow` en `null`. No se usa en iOS.

```csharp
App.ParentWindow = null; // no UI parent on iOS
```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid"></a>Elección entre el explorador web insertado o el explorador del sistema en Xamarin.Android

En la aplicación de Android, en `MainActivity.cs`, puede establecer la actividad primaria, para que los resultados de la autenticación vuelvan a ella:

```csharp
 App.ParentWindow = this;
```

A continuación, en `MainPage.xaml.cs`:

```csharp
authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(true)
                      .ExecuteAsync();
```

#### <a name="detecting-the-presence-of-custom-tabs-on-xamarinandroid"></a>Detección de la presencia de las pestañas personalizadas en Xamarin.Android

Si desea utilizar el explorador web del sistema para habilitar el inicio de sesión único con las aplicaciones que se ejecutan en el explorador, pero está preocupado por la experiencia de usuario de los dispositivos Android que no tienen un explorador que admita las pestañas personalizadas, tiene la opción de decidirlo llamando al método `IsSystemWebViewAvailable()` en `IPublicClientApplication`. Este método devuelve `true` si el administrador de paquetes detecta pestañas personalizadas y `false` si no se detectan en el dispositivo.

Según el valor devuelto por este método y sus requisitos, puede tomar una decisión:

- Puede devolver un mensaje de error personalizado al usuario. Por ejemplo:  "Instale Chrome para continuar con la autenticación", o bien
- puede recurrir a la opción de vista previa insertada e iniciar la interfaz de usuario como una vista web insertada.

El código siguiente muestra la opción de vista web insertada:

```csharp
bool useSystemBrowser = app.IsSystemWebviewAvailable();

authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(!useSystemBrowser)
                      .ExecuteAsync();
```

## <a name="net-core-does-not-support-interactive-authentication-out-of-the-box"></a>.NET Core no admite la autenticación interactiva de serie.

Para .NET Core, la adquisición de tokens de forma interactiva no está disponible. De hecho, .NET Core todavía no proporciona la interfaz de usuario. Si desea proporcionar un inicio de sesión interactivo para una aplicación de .NET Core, puede permitir que la aplicación presente al usuario un código y una dirección URL para iniciar sesión de forma interactiva (consulte [Flujo de código de dispositivo](msal-authentication-flows.md#device-code)).

Como alternativa puede implementar la interfaz [IWithCustomUI](scenario-desktop-acquire-token.md#withcustomwebui) y proporcionar su propio explorador.