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
ms.date: 04/26/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0be97bfbbaafd6045fd36be57d85e917f0325779
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600659"
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Ejemplos de código de Azure Active Directory (punto de conexión V2)

Puede usar Microsoft Azure Active Directory (Azure AD) para:

- Agregar autenticación y autorización a sus aplicaciones web y API web.
- Requerir un token de acceso para tener acceso a una API web protegida.

Este artículo describe brevemente y proporciona vínculos a ejemplos para el punto de conexión de V2 de Azure AD. Estos ejemplos le muestran cómo se hace, junto con fragmentos de código que puede usar en sus aplicaciones. En la página de ejemplos de código, encontrará temas Léame detallados que le ayudarán con los requisitos, la instalación y la configuración. Dispone de comentarios dentro del código para ayudarle a comprender las secciones críticas.

> [!NOTE]
> Si está interesado en los ejemplos de V1, consulte [Ejemplos de código de Azure Active Directory (punto de conexión V1)](sample-v1-code.md).

Para entender el escenario básico para cada tipo de ejemplo, vea [Tipos de aplicación para el punto de conexión v2.0 de Azure Active Directory](v2-app-types.md).

También puede contribuir a los ejemplos en GitHub. Para saber cómo, vea [Ejemplos y documentación de Microsoft Azure Active Directory](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-apps"></a>Aplicaciones de escritorio y móviles de cliente público

Los siguientes ejemplos muestran aplicaciones de cliente público (aplicaciones de escritorio o móviles) que tienen acceso a Microsoft Graph o a una API web en nombre de un usuario.

Aplicación cliente | Plataforma | Flujo y concesión | Llama a Microsoft Graph | Llamada a una API web de ASP.NET Core 2.0
------------------ | -------- | ---------- | -------------------- | -------------------------
Escritorio (WPF)      | .NET/C#  | Interactive | [dotnet-desktop-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [dotnet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Móvil (UWP)   | .NET/C# (UWP) | Interactive | [dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobile (Android, iOS, UWP)   | .NET/C# (Xamarin) | Interactive | [xamarin-native-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobile (iOS)       | iOS / Objective C o Swift | Interactive | [ios-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Móvil (Android)   | Android / Java | Interactive |   [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="web-applications"></a>Aplicaciones web

Los ejemplos siguientes ilustran las aplicaciones web que inician sesión por los usuarios, llaman a Microsoft Graph o llaman a una API web con la identidad del usuario.

 Plataforma | Únicamente inicio de sesión del usuario | Inicio de sesión del usuario y llamada a Microsoft Graph 
 -------- | ------------------- | --------------------------------- 
ASP.NET 4.x | [appmodelv2-webapp-openIDConnect-dotNet](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [dotnet-webapp-openidconnect-v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [aspnet-connect-rest-sample](https://github.com/microsoftgraph/aspnet-connect-rest-sample)   
ASP.NET Core 2.0 | [aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-connect-sample](https://github.com/microsoftgraph/aspnetcore-connect-sample)   
Node.js      |                   | [AppModelv2-WebApp-OpenIDConnect-nodejs](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)     
Ruby      |                   | [ruby-connect-rest-sample](https://github.com/microsoftgraph/ruby-connect-rest-sample)     

## <a name="daemon-applications"></a>Aplicaciones demonio

En los siguientes ejemplos se muestran aplicaciones de escritorio o web que acceden a Microsoft Graph o una API web con la identidad de la aplicación (sin usuario).

Aplicación cliente | Plataforma | Flujo y concesión | Llama a Microsoft Graph 
------------------ | -------- | ---------- | -------------------- 
Aplicación web | .NET/C#  | Credenciales de cliente | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) 

## <a name="single-page-applications-spa"></a>Aplicación de una sola página (SPA)

En este ejemplo se muestra cómo escribir una aplicación de una sola página protegida con Azure AD.

 Plataforma |  Llama a Microsoft Graph 
 -------- |  --------------------- 
JavaScript (msal.js)  | [javascript-graphapi-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-v2) 
JavaScript (msal.js + AngularJS) | [angular-connect-rest-sample](https://github.com/microsoftgraph/angular-connect-rest-sample) 
JavaScript (Hello.JS)  | [javascript-graphapi-web-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-web-v2) 
JavaScript (hello.js + Angular 4) | [angular4-connect-sample](https://github.com/microsoftgraph/angular4-connect-sample) 

## <a name="web-apis"></a>API web

### <a name="web-api-protected-by-azure-ad"></a>API web protegida por Azure AD

En el ejemplo siguiente se muestra cómo proteger una API web con el punto de conexión V2 de Azure AD.

Plataforma | Muestra | DESCRIPCIÓN
 -------- | ------------------- | ---------------------
Node.js | [dotnet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2) | Llama a una API web de ASP.NET Core desde una aplicación de WPF con Azure AD V2.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>API web llamando a Microsoft Graph o a otra API web

Este ejemplo no está disponible todavía.

## <a name="other-microsoft-graph-samples"></a>Otros ejemplos de Microsoft Graph

Para obtener información sobre [ejemplos](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) y tutoriales que muestren los diferentes patrones de uso de la API de Microsoft Graph, incluida la autenticación con Azure AD, consulte [Microsoft Graph Community samples & tutorials](https://github.com/microsoftgraph/msgraph-community-samples) (Ejemplos y tutoriales de la comunidad de Microsoft Graph).

## <a name="see-also"></a>Otras referencias

[Guía del desarrollador de Azure Active Directory](azure-ad-developers-guide.md)

[Conceptos y referencia de Graph API de Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteca auxiliar de Graph API de Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
