---
title: Consideraciones sobre Xamarin Android (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre las consideraciones específicas al usar Xamarin Android con Microsoft Authentication Library for .NET (MSAL.NET).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: c916ac98774600c16eb26ed43b8ae4b273137865
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76695014"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Consideraciones específicas de Xamarin Android con MSAL.NET
En este artículo se describen las consideraciones específicas al usar Xamarin Android con Microsoft Authentication Library for .NET (MSAL.NET).

## <a name="set-the-parent-activity"></a>Definición de la actividad primaria

En Xamarin.Android, debe establecer la actividad primaria para que el token se devuelva una vez que ha ocurrido la interacción.

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```
También puede establecer esto en el nivel PublicClientApplication (en MSAL4.2+) mediante una devolución de llamada.

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

Una recomendación es usar CurrentActivityPlugin [aquí](https://github.com/jamesmontemagno/CurrentActivityPlugin).  El código del compilador de PublicClientApplication presentaría un aspecto similar a este:

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```


## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>Devolución del control a MSAL una vez que finaliza la parte interactiva del flujo de autenticación
En Android, debe reemplazar el método `OnActivityResult` de `Activity` y llamar al método SetAuthenticationContinuationEventArgs de la clase de MSAL AuthenticationContinuationHelper.

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```
Esa línea garantiza que el control se devuelva a MSAL una vez que se complete la parte interactiva del flujo de autenticación.

## <a name="update-the-android-manifest"></a>Actualización del manifiesto de Android
`AndroidManifest.xml` debe contener los valores siguientes:
```csharp
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msal{client_id}" android:host="auth" />
         </intent-filter>
</activity>
```

O bien, puede [crear la actividad en el código](https://docs.microsoft.com/xamarin/android/platform/android-manifest#the-basics) y no editar `AndroidManifest.xml`manualmente. Para ello, debe crear una clase que tenga los atributos `Activity` y `IntentFilter`. Una clase que representa los mismos valores del archivo XML anterior sería:

```csharp
  [Activity]
  [IntentFilter(new[] { Intent.ActionView },
        Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
        DataHost = "auth",
        DataScheme = "msal{client_id}")]
  public class MsalActivity : BrowserTabActivity
  {
  }
```

### <a name="xamarinforms-43x-manifest"></a>Manifiesto XamarinForms 4.3.X

El código generado por XamarinForms 4.3.x establece el atributo `package` en `com.companyname.{appName}` en el archivo `AndroidManifest.xml`. Es posible que desee cambiar el valor para que coincida con el del espacio de nombres `MainActivity.cs`, si usa `DataScheme` como `msal{client_id}`.

## <a name="use-the-embedded-web-view-optional"></a>Uso de la vista web incrustada (opcional)

De forma predeterminada, MSAL.NET usa el explorador web del sistema, que le permite obtener SSO con aplicaciones web y otras aplicaciones. En algunos casos excepcionales, es posible que prefiera especificar que desea utilizar la vista web incrustada. Para más información, consulte [MSAL.NET utiliza un explorador web](msal-net-web-browsers.md) y [Explorador del sistema Android](msal-net-system-browser-android-considerations.md).

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>Solución de problemas
Si crea una nueva aplicación Xamarin.Forms y agrega una referencia al paquete NuGet de MSAL.Net, funcionará.
Sin embargo, si desea actualizar una aplicación Xamarin.Forms existente a la versión preliminar de MSAL.NET 1.1.2 o posterior, puede experimentar problemas de compilación.

Para solucionar estos problemas, debe hacer lo siguiente:
- Actualice el paquete NuGet de MSAL.NET existente a la versión preliminar de MSAL.NET 1.1.2 o posterior.
- Compruebe que Xamarin.Forms se actualiza automáticamente a la versión 2.5.0.122203 (si no es así, actualice a esta versión).
- Compruebe que Xamarin.Android.Support.v4 se actualiza automáticamente a la versión 25.4.0.2 (si no es así, actualice a esta versión).
- Todos los paquetes de Xamarin.Android.Support deben dirigir a la versión 25.4.0.2
- Limpie/recompile.
- Intente establecer el máximo de compilaciones paralelas del proyecto en 1 en Visual Studio (Opciones -> Proyectos y soluciones -> Compilar y ejecutar-> Número máximo de generaciones de proyecto paralelas).
- Como alternativa, si va a compilar desde la línea de comandos, pruebe a quitar /m del comando si lo está utilizando.


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>Error: El nombre 'AuthenticationContinuationHelper' no existe en el contexto actual.

Esto se debe probablemente a que Visual Studio no ha actualizado correctamente el archivo Android.csproj*. A veces la ruta de archivo **\<HintPath>** contiene incorrectamente netstandard13 en lugar de **monoandroid90**.

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Pasos siguientes

Se proporcionan más detalles y ejemplos en el párrafo [Consideraciones específicas de Android](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations) del archivo readme.md del ejemplo siguiente:

| Muestra | Plataforma | Descripción |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Una aplicación Xamarin Forms sencilla que muestra cómo usar MSAL para autenticar MSA y Azure AD mediante el punto de conexión de AADD 2.0 y cómo acceder a Microsoft Graph con el token resultante. <br>![Topología](media/msal-net-xamarin-android-considerations/topology.png) |
