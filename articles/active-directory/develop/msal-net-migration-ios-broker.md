---
title: Migración de aplicaciones de Xamarin iOS con Microsoft Authenticator de ADAL.NET a MSAL.NET | Azure
description: Obtenga información sobre cómo migrar aplicaciones de Xamarin iOS mediante Microsoft Authenticator de la biblioteca de autenticación de Azure AD para .NET (ADAL.NET) a la biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/08/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 609aeeada78985466cb7c254cab32864e5d84b0e
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70875605"
---
# <a name="migrating-ios-applications-using-microsoft-authenticator-from-adalnet-to-msalnet"></a>Migración de aplicaciones de iOS con Microsoft Authenticator de ADAL.NET a MSAL.NET

Hasta ahora ha usado ADAL.NET y el agente de iOS y es el momento de migrar a la [biblioteca de autenticación de Microsoft](msal-overview.md) MSAL.NET, que admite el agente en iOS a partir de la versión 4.3. 

¿Por dónde empezar? Este artículo le ayudará a migrar la aplicación de Xamarin iOS de ADAL a MSAL.

## <a name="prerequisites"></a>Requisitos previos
En este documento se supone que ya tiene una aplicación de Xamarin iOS que se integra con el agente de iOS. Si no es así, sería mejor que pase directamente a MSAL.NET y comience ahí la implementación del agente. Consulte [esta documentación](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS#why-use-brokers-on-xamarinios-and-xamarinandroid-applications) para obtener más información sobre cómo invocar el agente de iOS en MSAL.NET con una nueva aplicación.

## <a name="background"></a>Fondo

### <a name="what-are-brokers"></a>¿Qué son los agentes?

Los agentes son aplicaciones proporcionadas por Microsoft en Android e iOS([Microsoft Authenticator](https://www.microsoft.com/account/authenticator) en iOS y Android, portal de empresa de Intune en Android). 

Habilitan lo siguiente:

- Inicio de sesión único.
- Identificación del dispositivo, que es necesaria para algunas [directivas de acceso condicional](../conditional-access/overview.md) (consulte [administración de dispositivos](../conditional-access/conditions.md#device-platforms)).
- Comprobación de identificación de las aplicaciones, que también se requiere en algunos escenarios empresariales (consulte la [administración de aplicaciones móviles de Intune o MAM](https://docs.microsoft.com/intune/mam-faq)).

## <a name="migrate-from-adal-to-msal"></a>Migración de ADAL a MSAL

### <a name="step-1-enable-the-broker"></a>Paso 1: Habilitar el agente

<table>
<tr><td>Código de ADAL actual:</td><td>Homólogo de MSAL:</td></tr>
<tr><td>
En ADAL.NET, la compatibilidad con agentes se habilitaba en cada contexto de autenticación, pero esta opción está deshabilitada de forma predeterminada. Tenía que establecer una marca `useBroker` en true en el constructor `PlatformParameters` para llamar al agente:

```CSharp
public PlatformParameters(
        UIViewController callerViewController, 
        bool useBroker)
```
Además, en el código específico de la plataforma de este ejemplo, en el representador de páginas para iOS, establezca el valor de la marca `useBroker` 
en true:
```CSharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```

A continuación, incluya los parámetros en la llamada de adquisición de token:
```CSharp
 AuthenticationResult result =
                    await
                        AuthContext.AcquireTokenAsync(
                              Resource, 
                              ClientId, 
                              new Uri(RedirectURI), 
                              platformParameters)
                              .ConfigureAwait(false);
```

</td><td>
En MSAL.NET, la compatibilidad con el agente se habilita por aplicación cliente pública. Esta opción está deshabilitada de manera predeterminada. Para habilitarla, use 

el parámetro `WithBroker()` (establecido en true de manera predeterminada) para llamar al agente:

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos)
                .Build();
```
En la llamada al token de adquisición:
```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```
</table>

### <a name="step-2-set-a-uiviewcontroller"></a>Paso 2: Establecer un UIViewController()
En ADAL.NET, pasó el elemento UIViewController como parte de PlatformParameters (consulte el ejemplo en el paso 1). Sin embargo, en MSAL.NET, para ofrecer al desarrollador mayor flexibilidad, se usa una ventana de objetos, pero no se requiere en el uso normal de iOS. No obstante, para usar el agente debe establecer la ventana de objetos para enviar y recibir respuestas del agente. 
<table>
<tr><td>Código de ADAL actual:</td><td>Homólogo de MSAL:</td></tr>
<tr><td>
UIViewController se pasa a PlatformParamters en la plataforma específica de iOS.

```CSharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```
</td><td>
En MSAL.NET, deberá hacer dos cosas para configurar la ventana de objetos para iOS:

1) En `AppDelegate.cs`, establezca `App.RootViewController` en un nuevo `UIViewController()`. Esta asignación garantizará que hay un elemento UIViewController con la llamada al agente. Si no se establece correctamente, puede obtener este error: `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) En la llamada a AcquireTokenInteractive, use el elemento `.WithParentActivityOrWindow(App.RootViewController)`
y pase la referencia a la ventana de objetos que va a usar.

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

</table>

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>Paso 3: Actualizar AppDelegate para controlar la devolución de llamada
ADAL y MSAL llamarán al agente y, el agente, a su vez, volverá a llamar a la aplicación con el método `OpenUrl` de la clase `AppDelegate`. Hay más información disponible [aquí](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS/_edit#step-two-update-appdelegate-to-handle-the-callback).

:heavy_check_mark:**No hay ningún cambio aquí entre ADAL.NET y MSAL.NET**

### <a name="step-4-register-a-url-scheme"></a>Paso 4: Registrar un esquema de dirección URL
ADAL.NET y MSAL.NET usan direcciones URL para invocar al agente y devolver la respuesta del agente a la aplicación. Registre el esquema de dirección URL en el archivo `Info.plist` de la aplicación de la siguiente manera:

<table>
<tr><td>Código de ADAL actual:</td><td>Homólogo de MSAL:</td></tr>
<tr><td>
El esquema de dirección URL es exclusivo de la aplicación.
</td><td>
Tenga en cuenta que 

el nombre de `CFBundleURLSchemes` debe incluir 

`msauth.`

como prefijo, seguido del `CFBundleURLName`.

Por ejemplo: `$"msauth.(BundleId")`

```CSharp
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

> [!NOTE]
>  Este esquema de dirección URL se convertirá en parte del valor de RedirectUri que se usa para identificar de forma única la aplicación al recibir la respuesta del agente.

</table>

### <a name="step-5-lsapplicationqueriesschemes"></a>Paso 5: LSApplicationQueriesSchemes

ADAL.NET y MSAL.NET usan `-canOpenURL:` para comprobar si el agente está instalado en el dispositivo. Agregue el identificador correcto para el agente de iOS a la sección LSApplicationQueriesSchemes del archivo info.plist de la siguiente manera: 
<table>
<tr><td>Código de ADAL actual:</td><td>Homólogo de MSAL:</td></tr>
<tr><td>
Usos 

`msauth`


```CSharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>
```
</td><td>
Usos 

`msauthv2`


```CSharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauthv2</string>
</array>
```
</table>

### <a name="step-6-register-you-redirecturi-in-the-portal"></a>Paso 6: Registrarse en el portal de RedirectUri

ADAL.NET y MSAL.NET agregan un requisito adicional a redirectUri cuando el destino es el agente. Registre el URI de redirección con la aplicación en el portal.
<table>
<tr><td>Código de ADAL actual:</td><td>Homólogo de MSAL:</td></tr>
<tr><td>

`"<app-scheme>://<your.bundle.id>"` Ejemplo: `mytestiosapp://com.mycompany.myapp`
</td><td>

`$"msauth.{BundleId}://auth"`

ejemplo:

`public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; `

</table>

Para obtener más información sobre cómo registrar el redirectUri en el portal, consulte [Uso del agente en aplicaciones de Xamarin.iOS](msal-net-use-brokers-with-xamarin-apps.md#step-7-make-sure-the-redirect-uri-is-registered-with-your-app).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre las [Consideraciones específicas de Xamarin iOS con MSAL.NET](msal-net-xamarin-ios-considerations.md). 
