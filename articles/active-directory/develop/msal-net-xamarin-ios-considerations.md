---
title: Consideraciones sobre Xamarin iOS (Microsoft Authentication Library for .NET) | Azure
description: Obtenga información sobre las consideraciones específicas al usar Xamarin iOS con Microsoft Authentication Library for .NET (MSAL.NET).
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
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf236bff2300129ec97d3b8946c4c2a2748bca77
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65602134"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>Consideraciones específicas de Xamarin iOS con MSAL.NET
En Xamarin iOS, hay varias consideraciones que debe tener en cuenta al usar MSAL.NET.

- [Problemas conocidos con iOS 12 y la autenticación](#known-issues-with-ios-12-and-authentication)
- [Reemplazo e implementación de la función `OpenUrl` en `AppDelegate`](#implement-openurl)
- [Habilitación de grupos de la cadena de claves](#enable-keychain-groups)
- [Habilitación del uso compartido de caché de tokens](#enable-token-cache-sharing-across-ios-applications)
- [Habilitación de acceso a la cadena de claves](#enable-keychain-access)

## <a name="known-issues-with-ios-12-and-authentication"></a>Problemas conocidos con iOS 12 y la autenticación
Microsoft ha publicado un [documento informativo sobre seguridad](https://github.com/aspnet/AspNetCore/issues/4647) para proporcionar información sobre una incompatibilidad entre iOS 12 y algunos tipos de autenticación. La incompatibilidad afecta a los inicios de sesión en redes sociales, WSFED y OIDC. Este documento informativo proporciona instrucciones sobre qué pueden hacer desarrolladores para quitar las restricciones de seguridad actuales agregadas por ASP.NET a sus aplicaciones para garantizar la compatibilidad con iOS 12.  

Al desarrollar aplicaciones de MSAL.NET en Xamarin iOS, puede experimentar un bucle infinito al intentar iniciar sesión en sitios web desde iOS 12 (al igual que con esta [incidencia de ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329). 

También puede observa una incidencia con la autenticación en OIDC de ASP.NET Core con Safari para iOS 12, tal como se describe en este [WebKit](https://bugs.webkit.org/show_bug.cgi?id=188165).

## <a name="implement-openurl"></a>Implementación de OpenUrl

En primer lugar, tiene que invalidar el método `OpenUrl` de la clase derivada `FormsApplicationDelegate` y llamar a `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`.

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

También deberá definir un esquema URL, requerir permisos para la aplicación para llamar a otra aplicación, disponer de un formato específico para la dirección URL de redireccionamiento y registrar esta dirección URL de redireccionamiento en [Azure Portal](https://portal.azure.com).

## <a name="enable-keychain-groups"></a>Habilitación de grupos de la cadena de claves

Para hacer que la caché de tokens y el método `AcquireTokenSilentAsync` funcionen, se deben seguir varios pasos:
1. Habilite el acceso a la cadena de claves en el archivo *`* Entitlements.plist* y especifique los **grupos de la cadena de claves** en el identificador del paquete.
2. Seleccione el archivo *`*Entitlements.plist*`* en el campo **Derechos personalizados** en la **vista Firma de lote** de la ventana de opciones del proyecto de iOS.
3. Al firmar un certificado, asegúrese de que XCode usa el mismo identificador de Apple.

## <a name="enable-token-cache-sharing-across-ios-applications"></a>Habilitación del uso compartido de la caché de tokens entre aplicaciones de iOS

A partir de MSAL 2.x, puede especificar un grupo de seguridad de cadena de claves para usarlo para mantener la memoria caché de tokens entre varias aplicaciones. Esto le permite compartir la caché de tokens entre varias aplicaciones con el mismo grupo de seguridad de cadena de claves, incluidas aquellas desarrolladas con [ADAL.NET](https://aka.ms/adal-net), las aplicaciones Xamarin.iOS de MSAL.NET y las aplicaciones nativas de iOS desarrolladas con [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) o [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc).

Al compartir la caché de tokens, se permite el inicio de sesión único (SSO) entre todas las aplicaciones que usan el mismo grupo de seguridad de cadena de claves.

Para habilitar el inicio de sesión único, deberá establecer la propiedad `PublicClientApplication.iOSKeychainSecurityGroup` en el mismo valor en todas las aplicaciones.

Un ejemplo de esto con MSAL v3.x sería:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.msalrocks")
     .Build();
```

Un ejemplo de esto con MSAL v2.7.x sería:

```csharp
PublicClientApplication.iOSKeychainSecurityGroup = "com.microsoft.msalrocks";
```

> [!NOTE]
> La propiedad `KeychainSecurityGroup` está en desuso. Anteriormente, en MSAL 2.x, los desarrolladores se veían obligados a incluir el prefijo TeamId cuando se usaba la propiedad `KeychainSecurityGroup`. 
> 
> Ahora, a partir de MSAL 2.7.x, MSAL resolverá el prefijo TeamId en tiempo de ejecución cuando se use la propiedad `iOSKeychainSecurityGroup`. Al usar esta propiedad, el valor no debe contener el prefijo TeamId. 
> 
> Use la nueva propiedad `iOSKeychainSecurityGroup`, que no requiere que los desarrolladores proporcionen el prefijo TeamId. La propiedad `KeychainSecurityGroup` ahora está obsoleta. 

## <a name="enable-keychain-access"></a>Habilitación de acceso a la cadena de claves

En MSAL 2.x y ADAL 4.x, el prefijo TeamId se usa para acceder a la cadena de claves, lo que permite a las bibliotecas de autenticación proporcionar el inicio de sesión-único (SSO) entre las aplicaciones del mismo editor. 

¿Qué es el prefijo [TeamIdentifierPrefix](/xamarin/ios/deploy-test/provisioning/entitlements?tabs=vsmac) (TeamId)? Es un identificador único (empresarial o personal) en App Store. El valor de AppId es único para una aplicación. Si tiene más de una aplicación, el prefijo TeamId de todas las aplicaciones será el mismo, pero el valor de AppId será diferente. El sistema agrega automáticamente al grupo de acceso de la cadena de claves el prefijo TeamId para cada grupo. De este modo, el sistema operativo establece que las apps del mismo editor puedan acceder a la cadena de claves compartida. 

Al inicializar `PublicClientApplication`, si recibe `MsalClientException` con el mensaje: `TeamId returned null from the iOS keychain...`, deberá hacer lo siguiente en la aplicación de iOS Xamarin:

1. En Visual Studio, en la pestaña Depurar, vaya a nameOfMyApp.iOS Properties...
2. A continuación, vaya a Firma de lote de iOS. 
3. En Derechos personalizados, haga clic en ... y seleccione el archivo Entitlements.plist de la aplicación.
4. En el archivo csproj de la aplicación de iOS, ahora debe tener esta línea incluida: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. **Recompile** el proyecto.

Realice esto *además* de habilitar el acceso a la cadena de claves en el archivo `Entitlements.plist`, ya sea mediante el siguiente grupo de acceso o el suyo propio:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
</plist>
```

## <a name="next-steps"></a>Pasos siguientes

Se proporcionan más detalles en el párrafo [Consideraciones específicas de iOS](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations) del archivo readme.md del ejemplo siguiente:

Muestra | Plataforma | DESCRIPCIÓN 
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Una aplicación Xamarin Forms sencilla que muestra cómo usar MSAL para autenticar MSA y Azure AD mediante el punto de conexión de AADD 2.0 y cómo acceder a Microsoft Graph con el token resultante. <br>![Topología](media/msal-net-xamarin-ios-considerations/topology.png)