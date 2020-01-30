---
title: Consideraciones sobre Xamarin iOS (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre las consideraciones específicas al usar Xamarin iOS con Microsoft Authentication Library for .NET (MSAL.NET).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 599b8a3fdbad5747b0b303c71aeef084d04db6df
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76694927"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>Consideraciones específicas de Xamarin iOS con MSAL.NET
En Xamarin iOS, hay varias consideraciones que debe tener en cuenta al usar MSAL.NET.

- [Problemas conocidos con iOS 12 y la autenticación](#known-issues-with-ios-12-and-authentication)
- [Reemplazo e implementación de la función `OpenUrl` en `AppDelegate`](#implement-openurl)
- [Habilitación de grupos de la cadena de claves](#enable-keychain-access)
- [Habilitación del uso compartido de caché de tokens](#enable-token-cache-sharing-across-ios-applications)
- [Habilitación de acceso a la cadena de claves](#enable-keychain-access)

## <a name="implement-openurl"></a>Implementación de OpenUrl

En primer lugar, tiene que invalidar el método `OpenUrl` de la clase derivada `FormsApplicationDelegate` y llamar a `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`.

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

También deberá definir un esquema URL, requerir permisos para que la aplicación llame a otra aplicación, disponer de un formato específico para la dirección URL de redireccionamiento y registrar esta dirección URL de redireccionamiento en [Azure Portal](https://portal.azure.com).

### <a name="enable-keychain-access"></a>Habilitación de acceso a la cadena de claves

Para habilitar el acceso a la cadena de claves, la aplicación debe tener un grupo de acceso a la cadena de claves.
Puede establecer el grupo de acceso a la cadena de claves mediante la API `WithIosKeychainSecurityGroup()` al crear la aplicación como se muestra a continuación:

Para beneficiarse de la caché y el inicio de sesión único, debe establecer el grupo de acceso a la cadena de claves en el mismo valor en todas las aplicaciones.

Un ejemplo de esto con MSAL v4.x sería:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

Realice este cambio *además* de habilitar el acceso a la cadena de claves en el archivo `Entitlements.plist`, ya sea mediante el siguiente grupo de acceso o el suyo propio:

```xml
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
```

Al usar la API `WithIosKeychainSecurityGroup()`, MSAL anexa automáticamente el grupo de seguridad al final del *id. de equipo* de la aplicación (AppIdentifierPrefix), porque cuando compile la aplicación con Xcode el resultado será el mismo. Para más información, consulte la [documentación sobre los derechos de iOS](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps). Este es el motivo por el que los derechos tienen que incluir `$(AppIdentifierPrefix)` antes del grupo de acceso a la cadena de claves en `Entitlements.plist`.

### <a name="enable-token-cache-sharing-across-ios-applications"></a>Habilitación del uso compartido de la caché de tokens entre aplicaciones de iOS

A partir de MSAL 2.x, puede especificar un grupo de acceso a la cadena de claves para usarlo a fin de conservar la memoria caché de tokens entre varias aplicaciones. Esta opción le permite compartir la caché de tokens entre varias aplicaciones con el mismo grupo de acceso a la cadena de claves, incluidas las desarrolladas con [ADAL.NET](https://aka.ms/adal-net), las aplicaciones Xamarin.iOS de MSAL.NET y las aplicaciones nativas de iOS desarrolladas con [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) o [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc).

Al compartir la caché de tokens, se permite el inicio de sesión único entre todas las aplicaciones que usan el mismo grupo de acceso a la cadena de claves.

Para habilitar este uso compartido de la caché, debe usar el método "WithIosKeychainSecurityGroup()" para establecer el grupo de acceso a la cadena de claves en el mismo valor en todas las aplicaciones que comparten la caché, como se muestra en el ejemplo anterior.

Antes mencionamos que MSAL agrega $(AppIdentifierPrefix) cada vez que se usa la API `WithIosKeychainSecurityGroup()`. Esto se debe a que se usa AppIdentifierPrefix o el "Id. de equipo" para garantizar que solo las aplicaciones de un mismo editor puedan compartir el acceso a la cadena de claves.

> [!NOTE]
> **La propiedad `KeychainSecurityGroup` está en desuso.**
> 
> Anteriormente, en MSAL 2.x, los desarrolladores se veían obligados a incluir el prefijo TeamId cuando se usaba la propiedad `KeychainSecurityGroup`.
>
>  A partir de MSAL 2.7.x, cuando se use la nueva propiedad `iOSKeychainSecurityGroup`, MSAL resolverá el prefijo TeamId en tiempo de ejecución. Al usar esta propiedad, el valor no debe contener el prefijo TeamId.
>  Use la nueva propiedad `iOSKeychainSecurityGroup`, que no requiere el prefijo TeamId, ya que ahora la propiedad anterior `KeychainSecurityGroup` está en desuso.

### <a name="use-microsoft-authenticator"></a>Uso de Microsoft Authenticator

La aplicación puede usar Microsoft Authenticator (un agente) para habilitar lo siguiente:

- Inicio de sesión único (SSO). Los usuarios no tendrán que iniciar sesión en cada aplicación.
- Identificación de dispositivos. Mediante el acceso al certificado del dispositivo que se creó en este al unirse al área de trabajo. La aplicación estará lista si los administradores de inquilinos habilitan el acceso condicional relacionado con los dispositivos.
- Comprobación de identificación de la aplicación. Cuando una aplicación llama al agente, pasa su dirección URL de redireccionamiento y el agente la comprueba.

Para obtener más información sobre cómo habilitar el agente, consulte [Uso de Microsoft Authenticator o el portal de empresa de Microsoft Intune en aplicaciones de Xamarin para iOS y Android](msal-net-use-brokers-with-xamarin-apps.md).

### <a name="sample-illustrating-xamarin-ios-specific-properties"></a>Ejemplo en el que se ilustran las propiedades específicas de Xamarin iOS

Se proporcionan más detalles en el párrafo [Consideraciones específicas de iOS](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations) del archivo readme.md del ejemplo siguiente:

Muestra | Plataforma | Descripción
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Aplicación de Xamarin Forms sencilla que muestra cómo se usa MSAL para autenticar MSA y Azure AD mediante el punto de conexión de Azure AD v2.0 y cómo se obtiene acceso a Microsoft Graph con el token resultante.

<!--- https://github.com/Azure-Samples/active-directory-xamarin-native-v2/blob/master/ReadmeFiles/Topology.png -->

## <a name="known-issues-with-ios-12-and-authentication"></a>Problemas conocidos con iOS 12 y la autenticación
Microsoft ha publicado un [documento informativo sobre seguridad](https://github.com/aspnet/AspNetCore/issues/4647) para proporcionar información sobre una incompatibilidad entre iOS 12 y algunos tipos de autenticación. La incompatibilidad afecta a los inicios de sesión en redes sociales, WSFed y OIDC. Este documento informativo proporciona instrucciones sobre qué pueden hacer desarrolladores para quitar las restricciones de seguridad actuales agregadas por ASP.NET a sus aplicaciones para garantizar la compatibilidad con iOS 12.  

Al desarrollar aplicaciones MSAL.NET en Xamarin iOS, puede experimentar un bucle infinito al intentar iniciar sesión en sitios web desde iOS 12 (al igual que con esta [incidencia de ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329)). 

También puede observa una incidencia con la autenticación en OIDC de ASP.NET Core con Safari para iOS 12, tal como se describe en este [WebKit](https://bugs.webkit.org/show_bug.cgi?id=188165).
