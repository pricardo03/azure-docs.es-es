---
title: Ejemplos de código de Azure Active Directory | Microsoft Docs
description: Proporciona un índice de ejemplos de código disponibles de Azure Active Directory (punto de conexión V2), organizados por escenario.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/17/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 7323f532787fa767d875e13262cce9f1ceaa79d8
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955531"
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Ejemplos de código de Azure Active Directory (punto de conexión V2)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Puede usar Microsoft Azure Active Directory (Azure AD) para:

- Agregar autenticación y autorización a sus aplicaciones web y API web.
- Requerir un token de acceso para tener acceso a una API web protegida.

Este artículo describe brevemente y proporciona vínculos a ejemplos para el punto de conexión de V2 de Azure AD. Estos ejemplos le muestran cómo se hace, junto con fragmentos de código que puede usar en sus aplicaciones. En la página de ejemplos de código, encontrará temas Léame detallados que le ayudarán con los requisitos, la instalación y la configuración. Dispone de comentarios dentro del código para ayudarle a comprender las secciones críticas.

> [!NOTE]
> Si está interesado en los ejemplos de V1, consulte [Azure AD code samples (V1 endpoint)](sample-v1-code.md) Ejemplos de código de Azure AD (punto de conexión V1).

Para entender el escenario básico para cada tipo de ejemplo, vea [Tipos de aplicación para el punto de conexión v2.0 de Azure Active Directory](v2-app-types.md).

También puede contribuir a los ejemplos en GitHub. Para saber cómo, vea [Ejemplos y documentación de Microsoft Azure Active Directory](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications-spa"></a>Aplicación de página única (SPA)

En este ejemplo se muestra cómo escribir una aplicación de página única protegida con Azure AD. En estos ejemplos se usa uno de los tipos de MSAL.js: [biblioteca de autenticación de Microsoft para JavaScript](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core), [biblioteca de autenticación de Microsoft para Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) o [biblioteca de autenticación de Microsoft para AngularJS](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)

 Plataforma |  Llama a Microsoft Graph
 -------- |  ---------------------
![JavaScript](media/sample-v2-code/logo_js.png) JavaScript (msal.js)  | [javascript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2)
![Angular JS](media/sample-v2-code/logo_angular.png) JavaScript (MSAL AngularJS) | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs/samples/MsalAngularjsDemoApp)
![Angular](media/sample-v2-code/logo_angular.png) JavaScript (MSAL Angular) | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular/samples/MSALAngularDemoApp)

## <a name="web-applications"></a>Aplicaciones web

Los ejemplos siguientes ilustran las aplicaciones web que inician la sesión de los usuarios. Algunos ejemplos también muestran la aplicación que llama a Microsoft Graph o a su propia API web con la identidad del usuario.

 Plataforma | Únicamente inicio de sesión del usuario | Inicio de sesión del usuario y llamada a Microsoft Graph
 -------- | ------------------- | ---------------------------------
![ASP.NET](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET | [appmodelv2-webapp-openIDConnect-dotNet](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [dotnet-webapp-openidconnect-v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [aspnet-connect-rest-sample](https://github.com/microsoftgraph/aspnet-connect-rest-sample)
![ASP.NET](media/sample-v2-code/logo_NETcore.png)<p/>ASP.NET Core 2.0 | [aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-connect-sample](https://github.com/microsoftgraph/aspnetcore-connect-sample)
![Node.js](media/sample-v2-code/logo_nodejs.png)  |                   | [AppModelv2-WebApp-OpenIDConnect-nodejs](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)
![Ruby](media/sample-v2-code/logo_ruby.png) |                   | [ruby-connect-rest-sample](https://github.com/microsoftgraph/ruby-connect-rest-sample)

## <a name="desktop-and-mobile-public-client-apps"></a>Aplicaciones de escritorio y móviles de cliente público

Los siguientes ejemplos muestran aplicaciones cliente público (aplicaciones de escritorio o móviles) que tienen acceso a Microsoft Graph o a una API web en nombre de un usuario. Todas estas aplicaciones cliente usan bibliotecas de autenticación de MicroSoft (MSAL)

Aplicación cliente | Plataforma | Flujo y concesión | Llama a Microsoft Graph | Llamada a una API web de ASP.NET Core 2.0
------------------ | -------- |  ----------| ---------- | -------------------------
Escritorio (WPF)      | ![.NET/C#](media/sample-v2-code/logo_NET.png) | interactiva | [dotnet-desktop-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [dotnet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Escritorio (consola)   | ![.NET/C# (escritorio)](media/sample-v2-code/logo_NET.png) | Autenticación integrada de Windows |[dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2)
Escritorio (consola)   | ![.NET/C# (escritorio)](media/sample-v2-code/logo_NETcore.png) | Nombre de usuario/contraseña |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-up-v2)
Móvil (UWP)   | ![.NET/C# (UWP)](media/sample-v2-code/logo_windows.png) | interactiva |[dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobile (Android, iOS, UWP)   | ![.NET/C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | interactiva |[xamarin-native-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobile (iOS)       | ![iOS / Objective C o Swift](media/sample-v2-code/logo_iOS.png) | interactiva |[ios-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Móvil (Android)   | ![Android / Java](media/sample-v2-code/logo_Android.png) | interactiva |  [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="daemon-applications"></a>Aplicaciones demonio

En el siguiente ejemplo se muestra una aplicación que accede a Microsoft Graph con su propia identidad (sin ningún usuario).

Aplicación cliente | Plataforma | Flujo y concesión | Llama a Microsoft Graph
------------------ | -------- | ---------- | --------------------
Consola | ![.NET Core](media/sample-v2-code/logo_NETcore.png)<p/> ASP.NET  | Credenciales de cliente | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2)
Aplicación web | ![ASP.NET](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET  | Credenciales de cliente | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2)

## <a name="headless-applications"></a>Aplicaciones sin periféricos

Los siguientes ejemplos muestran aplicaciones cliente público que se ejecutan en un dispositivo sin un explorador web. Estas aplicaciones pueden ser herramientas de la línea de comandos, aplicaciones que se ejecutan en Linux o Mac o una aplicación de IoT. Los ejemplos incluyen aplicaciones que acceden a Microsoft Graph en nombre de un usuario que inicia sesión de forma interactiva en otro dispositivo (por ejemplo, un teléfono móvil). Todas estas aplicaciones cliente usan bibliotecas de autenticación de MicroSoft (MSAL)

Aplicación cliente | Plataforma | Flujo y concesión | Llama a Microsoft Graph
------------------ | -------- |  ----------| ----------
Escritorio (consola)   | ![.NET/C# (escritorio)](media/sample-v2-code/logo_NETcore.png) | Flujo de código de dispositivo |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2)

## <a name="web-apis"></a>API web

En el ejemplo siguiente se muestra cómo proteger una API web con el punto de conexión V2 de Azure AD. Esta API la ejecuta una aplicación de WPF (pero realmente podría llamarla cualquier aplicación).

Plataforma | Muestra
 -------- | -------------------
![.NET/C#](media/sample-v2-code/logo_NET.png) | [dotnet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)

## <a name="other-microsoft-graph-samples"></a>Otros ejemplos de Microsoft Graph

Para ver [ejemplos](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) y tutoriales que muestren los diferentes patrones de uso de Microsoft Graph API, incluida la autenticación con Azure AD, consulte [Microsoft Graph Community samples & tutorials](https://github.com/microsoftgraph/msgraph-community-samples) (Ejemplos y tutoriales de la comunidad de Microsoft Graph).

## <a name="see-also"></a>Otras referencias

[Guía del desarrollador de Azure Active Directory](v1-overview.md)

[Conceptos y referencia de Graph API de Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteca auxiliar de Graph API de Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
