---
title: Consideraciones sobre Xamarin iOS (Microsoft Authentication Library for .NET) | Azure
description: Obtenga información sobre las consideraciones específicas al usar Xamarin iOS con Microsoft Authentication Library for .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 906f2fc8cdac31922e6e93526f65577fe76c4b9c
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532367"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>Consideraciones específicas de Xamarin iOS con MSAL.NET
En Xamarin iOS, hay varias consideraciones que debe tener en cuenta al usar MSAL.NET.

- [Problemas conocidos con iOS 12 y la autenticación](#known-issues-with-ios-12-and-authentication)
- [Reemplazo e implementación de la función `OpenUrl` en `AppDelegate`](#implement-openurl)
- [Habilitación de grupos de la cadena de claves](#enable-keychain-access)
- [Habilitación del uso compartido de caché de tokens](#enable-token-cache-sharing-across-ios-applications)
- [Habilitación de acceso a la cadena de claves](#enable-keychain-access)

## <a name="known-issues-with-ios-12-and-authentication"></a>Problemas conocidos con iOS 12 y la autenticación
Microsoft ha publicado un [documento informativo sobre seguridad](https://github.com/aspnet/AspNetCore/issues/4647) para proporcionar información sobre una incompatibilidad entre iOS 12 y algunos tipos de autenticación. La incompatibilidad afecta a los inicios de sesión en redes sociales, WSFed y OIDC. Este documento informativo proporciona instrucciones sobre qué pueden hacer desarrolladores para quitar las restricciones de seguridad actuales agregadas por ASP.NET a sus aplicaciones para garantizar la compatibilidad con iOS 12.  

Al desarrollar aplicaciones de MSAL.NET en Xamarin iOS, puede experimentar un bucle infinito al intentar iniciar sesión en sitios web desde iOS 12 (al igual que con esta [incidencia de ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329)). 

También puede observa una incidencia con la autenticación en OIDC de ASP.NET Core con Safari para iOS 12, tal como se describe en este [WebKit](https://bugs.webkit.org/show_bug.cgi?id=188165).

## <a name="implement-openurl"></a>Implementación de OpenUrl

En primer lugar, tiene que invalidar el método `OpenUrl` de la clase derivada `FormsApplicationDelegate` y llamar a `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`.

```CSharp
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

Para habilitar el inicio de sesión único, deberá establecer la propiedad `PublicClientApplication.iOSKeychainSecurityGroup` en el mismo valor en todas las aplicaciones.

Un ejemplo de esto con MSAL v3.x sería:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.msalrocks")
     .Build();
```

El archivo entitlements.plist debe actualizarse de modo que tenga un aspecto similar al siguiente fragmento XML:

Realice este cambio *además* de habilitar el acceso a la cadena de claves en el archivo `Entitlements.plist`, ya sea mediante el siguiente grupo de acceso o el suyo propio:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.msalrocks</string>
  </array>
</dict>
</plist>
```

Un ejemplo de esto con MSAL v4.x sería:

```csharp
PublicClientApplication.iOSKeychainSecurityGroup = "com.microsoft.msalrocks";
```

Al usar la API `WithIosKeychainSecurityGroup()`, MSAL anexará automáticamente el grupo de seguridad al final del "Id. de equipo" de la aplicación (AppIdentifierPrefix), porque cuando usted compile la aplicación con Xcode, hará lo mismo. [Consulte la documentación sobre los derechos de iOS para conocer los detalles](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps). Esta es la razón por la que necesita actualizar los derechos para incluir $(AppIdentifierPrefix) antes del grupo de acceso a la cadena de claves en el archivo entitlements.plist.

### <a name="enable-token-cache-sharing-across-ios-applications"></a>Habilitación del uso compartido de la caché de tokens entre aplicaciones de iOS

A partir de MSAL 2.x, puede especificar un grupo de acceso a la cadena de claves para usarlo a fin de conservar la memoria caché de tokens entre varias aplicaciones. Esta opción le permite compartir la caché de tokens entre varias aplicaciones con el mismo grupo de acceso a la cadena de claves, incluidas las desarrolladas con [ADAL.NET](https://aka.ms/adal-net), las aplicaciones Xamarin.iOS de MSAL.NET y las aplicaciones nativas de iOS desarrolladas con [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) o [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc).

Al compartir la caché de tokens, se permite el inicio de sesión único entre todas las aplicaciones que usan el mismo grupo de acceso a la cadena de claves.

Para habilitar este uso compartido de la caché, debe usar el método "WithIosKeychainSecurityGroup()" para establecer el grupo de acceso a la cadena de claves en el mismo valor en todas las aplicaciones que comparten la caché, como se muestra en el ejemplo anterior.

Antes mencionamos que MSAL agrega $(AppIdentifierPrefix) cada vez que se usa la API `WithIosKeychainSecurityGroup()`. Esto se debe a que se usa AppIdentifierPrefix o el "Id. de equipo" para garantizar que solo las aplicaciones de un mismo editor puedan compartir el acceso a la cadena de claves.

#### <a name="note-keychainsecuritygroup-property-deprecated"></a>Nota: La propiedad KeychainSecurityGroup está en desuso

Anteriormente, en MSAL 2.x, los desarrolladores se veían obligados a incluir el prefijo TeamId cuando se usaba la propiedad `KeychainSecurityGroup`.

A partir de MSAL 2.7.x, cuando se use la nueva propiedad `iOSKeychainSecurityGroup`, MSAL resolverá el prefijo TeamId en tiempo de ejecución. Al usar esta propiedad, el valor no debe contener el prefijo TeamId.

Use la nueva propiedad `iOSKeychainSecurityGroup`, que no requiere que los desarrolladores proporcionen el prefijo TeamId, ya que ahora la propiedad anterior `KeychainSecurityGroup` está en desuso.

### <a name="sample-illustrating-xamarin-ios-specific-properties"></a>Ejemplo en el que se ilustran las propiedades específicas de Xamarin iOS

Se proporcionan más detalles en el párrafo [Consideraciones específicas de iOS](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations) del archivo readme.md del ejemplo siguiente:

Muestra | Plataforma | DESCRIPCIÓN
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Aplicación de Xamarin Forms sencilla que muestra cómo se usa MSAL para autenticar MSA y Azure AD mediante el punto de conexión de Azure AD v2.0 y cómo se obtiene acceso a Microsoft Graph con el token resultante.

<!--- https://github.com/Azure-Samples/active-directory-xamarin-native-v2/blob/master/ReadmeFiles/Topology.png -->
