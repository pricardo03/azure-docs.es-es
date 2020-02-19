---
title: Uso de agentes con Xamarin iOS y Android | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a configurar aplicaciones de Xamarin iOS que puedan usar Microsoft Authenticator y la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET). Obtenga información sobre cómo migrar de la Biblioteca de autenticación de Azure AD para .NET (ADAL.NET) a la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/08/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 25b8aa9b5e80720e9543dafce7970404a62b7d1f
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132639"
---
# <a name="use-microsoft-authenticator-or-intune-company-portal-on-xamarin-applications"></a>Uso de Microsoft Authenticator o el Portal de empresa de Intune en aplicaciones de Xamarin

En iOS y Android, agentes como Microsoft Authenticator y el Portal de empresa de Microsoft Intune específico para Android permiten lo siguiente:

- **Inicio de sesión único (SSO)** : los usuarios no tienen que iniciar sesión en cada aplicación.
- **Identificación de dispositivos**: el agente tiene acceso al certificado del dispositivo. Este certificado se crea en el dispositivo cuando se une al área de trabajo.
- **Comprobación de identificación de la aplicación**: cuando una aplicación llama al agente, pasa su URL de redireccionamiento. El agente comprueba la URL.

Para habilitar una de estas características, use el parámetro `WithBroker()` al llamar al método `PublicClientApplicationBuilder.CreateApplication`. El parámetro `.WithBroker()` se establece en true de forma predeterminada. 

También puede usar las instrucciones de las secciones siguientes para configurar la autenticación asincrónica para aplicaciones [iOS](#brokered-authentication-for-ios) o [Android](#brokered-authentication-for-android).

## <a name="brokered-authentication-for-ios"></a>Autenticación asincrónica para iOS

Realice los pasos siguientes para permitir que la aplicación de Xamarin.iOS establezca comunicación con la aplicación [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458).

### <a name="step-1-enable-broker-support"></a>Paso 1: Habilitar la compatibilidad con el agente
Debe habilitar la compatibilidad del agente para las instancias individuales de `PublicClientApplication`. La compatibilidad está deshabilitada de manera predeterminada. Al crear `PublicClientApplication` a través de `PublicClientApplicationBuilder`, utilice el parámetro `WithBroker()` como se muestra en el ejemplo siguiente. El parámetro `WithBroker()` se establece en true de forma predeterminada.

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-enable-keychain-access"></a>Paso 2: Habilitación de acceso a la cadena de claves

Para habilitar el acceso a la cadena de claves, debe tener un grupo de acceso a la cadena de claves para su aplicación. Puede usar la API de `WithIosKeychainSecurityGroup()` para establecer el grupo de acceso a cadenas de claves al crear la aplicación:

```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
      
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

Para más información, consulte [Habilitación de acceso a la cadena de claves](msal-net-xamarin-ios-considerations.md#enable-keychain-access).

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>Paso 3: Actualizar AppDelegate para controlar la devolución de llamada
Cuando la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET) llama al agente, el agente devuelve la llamada a la aplicación mediante el método `OpenUrl` de la clase `AppDelegate`. Dado que MSAL espera la respuesta del agente, la aplicación debe cooperar para volver a llamar a MSAL.NET. Para permitir esta cooperación, actualice el archivo `AppDelegate.cs` para invalidar el método siguiente.

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

Este método se invoca cada vez que se inicia la aplicación. Se usa como una oportunidad para procesar la respuesta del agente y finalizar el proceso de autenticación iniciado por MSAL.NET.

### <a name="step-4-set-uiviewcontroller"></a>Paso 4: Establecer UIViewController()
En el archivo `AppDelegate.cs`, debe establecer una ventana de objeto. Normalmente, para Xamarin iOS, no es necesario hacerlo. No obstante, se necesita una ventana de objeto para enviar y recibir respuestas del agente. 

Para configurar la ventana de objeto: 
1. En el archivo `AppDelegate.cs`, establezca `App.RootViewController` en un nuevo elemento `UIViewController()`. Esta asignación garantiza que la llamada al agente incluye `UIViewController`. Si esta configuración se asigna de forma incorrecta, puede obtener este error:

      `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`

1. En la llamada a `AcquireTokenInteractive`, use `.WithParentActivityOrWindow(App.RootViewController)` y, a continuación, pase la referencia a la ventana de objeto que usará.

    En `App.cs`:
    
    ```csharp
       public static object RootViewController { get; set; }
    ```
    
    En `AppDelegate.cs`:
    
    ```csharp
       LoadApplication(new App());
       App.RootViewController = new UIViewController();
    ```
    
    En la llamada `AcquireToken`:
    
    ```csharp
    result = await app.AcquireTokenInteractive(scopes)
                 .WithParentActivityOrWindow(App.RootViewController)
                 .ExecuteAsync();
    ```

### <a name="step-5-register-a-url-scheme"></a>Paso 5: Registrar un esquema de dirección URL
MSAL.NET usa direcciones URL para invocar al agente y devolver la respuesta del agente a la aplicación. Para finalizar el recorrido de ida y vuelta, registre un esquema de dirección URL para la aplicación en el archivo `Info.plist`.

El nombre `CFBundleURLSchemes` debe incluir el prefijo `msauth.`. El prefijo debe ir seguido de `CFBundleURLName`. 

En el esquema de dirección URL, `BundleId` identifica de forma única la aplicación: `$"msauth.(BundleId)"`. Por tanto, si `BundleId` es `com.yourcompany.xforms`, el esquema de dirección URL es `msauth.com.yourcompany.xforms`.

> [!NOTE]
> Este esquema de dirección URL se convierte en parte del URI de redirección que identifica de forma única la aplicación cuando recibe la respuesta del agente.

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

### <a name="step-6-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>Paso 6: Agregar el identificador de agente a la sección LSApplicationQueriesSchemes
MSAL usa `–canOpenURL:` para comprobar si el agente está instalado en el dispositivo. En iOS 9, Apple ha bloqueado los esquemas que puede consultar una aplicación. 

Agregue `msauthv2` a la sección `LSApplicationQueriesSchemes` del archivo `Info.plist`, como en el ejemplo siguiente:

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
    </array>
```

### <a name="step-7-register-your-redirect-uri-in-the-application-portal"></a>Paso 7: Registrar el URI de redirección en el portal de aplicaciones
SI usa el agente, se agrega un requisito adicional al URI de redirección. El URI de redirección _debe_ tener el siguiente formato:
```csharp
$"msauth.{BundleId}://auth"
```

Este es un ejemplo: 

```csharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
Observe que el URI de redirección coincide con el nombre `CFBundleURLSchemes` que incluyó en el archivo `Info.plist`.

### <a name="step-8-make-sure-the-redirect-uri-is-registered-with-your-app"></a>Paso 8: Asegurarse de que el URI de redirección está registrado en la aplicación

Este URI de redirección debe estar registrado en el [portal de registro de aplicaciones](https://portal.azure.com) como válido para la aplicación. 

El portal de registro de aplicaciones proporciona una nueva experiencia para ayudarle a calcular el URI de respuesta asincrónica a partir del identificador de paquete. 

Para procesar el URI de redirección:

1. En el portal de registro de aplicaciones, seleccione **Autenticación** > **Probar la nueva experiencia**.

   ![Probar la nueva experiencia de registro de aplicaciones](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

1. Seleccione **Agregar una plataforma**.

   ![Agregar una plataforma](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

1. Cuando se admita la lista de plataformas, seleccione **iOS**.

   ![Configurar iOS](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

1. Escriba el identificador del conjunto que se solicita y, luego, presione **Configurar**.

   ![Escribir el ID del paquete](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

Al finalizar los pasos, el URI de redirección se procesa automáticamente.

![Copiar el URI de redirección](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Autenticación asincrónica para Android

MSAL.NET solo es compatible con la plataforma Xamarin.iOS. Todavía no es compatible con los agentes de la plataforma Xamarin.Android.

La biblioteca nativa de Android para MSAL ya admite la autenticación asincrónica. Para más información, consulte [Autenticación con agente en Android](brokered-auth.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información, consulte [Consideraciones para usar la Plataforma universal de Windows con MSAL.NET](msal-net-uwp-considerations.md).
