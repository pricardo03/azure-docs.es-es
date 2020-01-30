---
title: Consideraciones de UWP (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre las consideraciones específicas al usar la Plataforma universal de Windows con la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 549e36099e06d665ecab879ceb3c38987d000852
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76695099"
---
# <a name="universal-windows-platform-specific-considerations-with-msalnet"></a>Consideraciones específicas de la Plataforma universal de Windows con MSAL.NET
En UWP, hay varias consideraciones que debe tener en cuenta al usar MSAL.NET.

## <a name="the-usecorporatenetwork-property"></a>La propiedad UseCorporateNetwork
En la plataforma de WinRT, `PublicClientApplication` tiene la siguiente propiedad booleana ``UseCorporateNetwork``. Esta propiedad permite que las aplicaciones de Win8.1 y UWP se beneficien de la autenticación integrada (y, por lo tanto, del inicio de sesión único con el inicio de sesión de usuario con el sistema operativo) si el usuario ha iniciado sesión con una cuenta de un inquilino federado de Azure AD. Al establecer esta propiedad, MSAL.NET usa WAB (agente de autenticación web).

> [!IMPORTANT]
> Si establece esta propiedad en true, se da por supuesto que el desarrollador de aplicaciones ha habilitado la autenticación integrada (IWA) en la aplicación. Para esto:
> - En el ``Package.appxmanifest`` de la aplicación de UWP, en la pestaña **Funcionalidades**, habilite las siguientes funcionalidades:
>   - Autenticación empresarial
>   - Redes privadas (cliente y servidor)
>   - Certificado de usuario compartido

La autenticación integrada no está habilitada de manera predeterminada porque las aplicaciones que solicitan las funcionalidades Autenticación empresarial o Certificados de usuario compartidos requieren un mayor nivel de comprobación para que Windows Store las acepte, y no todos los desarrolladores quieren realizar un mayor nivel de comprobación.

La implementación subyacente en la plataforma UWP (WAB) no funciona correctamente en escenarios empresariales en los que se ha habilitado el acceso condicional. El síntoma es que el usuario intenta iniciar sesión con Windows Hello y se le propone que elija un certificado, pero:

- No se encuentra el certificado para el PIN.
- O el usuario lo elige pero no se le solicita el PIN.

Una solución alternativa es usar un método alternativo (nombre de usuario/contraseña + autenticación por teléfono), pero la experiencia no es buena.

## <a name="troubleshooting"></a>Solución de problemas

Algunos clientes han comunicado que, en algunos entornos empresariales específicos, se produjo el siguiente error de inicio de sesión:

```Text
We can't connect to the service you need right now. Check your network connection or try this again later
```

Sin embargo, saben que tienen una conexión a Internet y que funciona con una red pública.

Una solución alternativa consiste en asegurarse de que WAB (el componente subyacente de Windows) permita la red privada. Puede hacerlo estableciendo una clave del Registro:

```Text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe\EnablePrivateNetwork = 00000001
```

Para obtener información, vea [Agente de autenticación web: Fiddler](https://docs.microsoft.com/windows/uwp/security/web-authentication-broker#fiddler).

## <a name="next-steps"></a>Pasos siguientes
En los ejemplos siguientes se proporcionan más detalles:

Muestra | Plataforma | Descripción 
|------ | -------- | -----------|
|[active-directory-dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) | UWP | Una aplicación cliente de la Plataforma universal de Windows que utiliza msal.net, accediendo a Microsoft Graph para un usuario que se autentica con el punto de conexión Azure AD v2.0. <br>![Topología](media/msal-net-uwp-considerations/topology-native-uwp.png)|
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Una aplicación de Xamarin Forms sencilla que muestra cómo usar MSAL para autenticar MSA y Azure AD mediante el punto de conexión AAD v2.0 y cómo obtener acceso a Microsoft Graph con el token resultante. <br>![Topología](media/msal-net-uwp-considerations/topology-xamarin-native.png)|
