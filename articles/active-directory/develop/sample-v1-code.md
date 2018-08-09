---
title: Ejemplos de código de Azure Active Directory | Microsoft Docs
description: Proporciona un índice de ejemplos de código de Azure Active Directory (punto de conexión v1), organizados por escenario.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 221ff82e9c8d5164dfc7d7ee25623e6f73f59e13
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39580903"
---
# <a name="azure-active-directory-code-samples-v1-endpoint"></a>Ejemplos de código de Azure Active Directory (punto de conexión V1)

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Puede usar Microsoft Azure Active Directory (Azure AD) para agregar autenticación y autorización a sus aplicaciones web y API web.

Esta sección proporciona vínculos a ejemplos que puede usar para más información sobre el punto de conexión de Azure AD V1. En estos ejemplos se muestra cómo se hace, junto con fragmentos de código que puede usar en sus aplicaciones. En la página de ejemplos de código, encontrará temas Léame detallados que le ayudarán con los requisitos, la instalación y la configuración. Y se comenta el código para ayudarle a comprender las secciones críticas.

> [!NOTE]
> Si está interesado en ejemplos de código de Azure AD V2, consulte [ejemplos de código de v2.0 por escenario](sample-v2-code.md).

Para comprender los escenarios básicos para cada tipo de ejemplo, consulte [Escenarios de autenticación para Azure AD](authentication-scenarios.md).

También puede contribuir a nuestros ejemplos en GitHub. Para saber cómo, vea [Ejemplos y documentación de Microsoft Azure Active Directory](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Aplicaciones de escritorio y móviles de cliente público que llaman a Microsoft Graph o a una API web

Los siguientes ejemplos muestran aplicaciones de cliente público (aplicaciones de escritorio o móviles) que tienen acceso a Microsoft Graph o a una API web en nombre de un usuario.

Aplicación cliente | Plataforma | Flujo y concesión | Llama a Microsoft Graph | Llama a una API web de ASP.NET o de ASP.NET Core 2.0
------------------ | -------- | ---------- | -------------------- | -------------------------
Escritorio (WPF)           | .NET/C# | Interactive | [dotnet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [Dotnet-native-desktop](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [dotnet-native-aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [dotnet-webapi-manual-jwt-validation](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Móvil (UWP)            | .NET/C#  | Interactive | [dotnet-native-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  [dotnet-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (API web de un solo inquilino) </p> [dotnet-webapi-multitenant-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (API web de varios inquilinos)|
Mobile (Android, iOS, UWP)   | .NET/C# (Xamarin) | Interactive | [dotnet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Móvil (Android)           | Android/Java | Interactive |   [android](https://github.com/Azure-Samples/active-directory-android) |
Mobile (iOS)           | iOS/Objective C | Interactive |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Escritorio (consola)          | .NET/C# | Nombre de usuario y contraseña </p> Autenticación integrada de Windows | | [dotnet-native-headless](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Escritorio (consola)           | .NET Core/C# | Perfil de dispositivo | | [dotnet-deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="web-applications"></a>Aplicaciones web

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Aplicaciones Web que inician sesión por los usuarios, llaman a Microsoft Graph o una API web con la identidad del usuario

 Plataforma | Únicamente inicio de sesión del usuario | Llama a Microsoft Graph o a AAD Graph| Llama a otra API web de ASP.NET o de ASP.NET Core 2.0
 -------- | ------------------- | --------------------- | -------------------------
ASP.NET 4.5 | [webApp-openidconnect-dotnet](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-aspnetwebapp-v1) </p> [webapp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | [dotnet-webapp-multitenant-openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) (AAD Graph) |
ASP.NET Core 2.0 | [dotnet-webapp-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [webapp-webapi-multitenant-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) (AAD Graph) | [dotnet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
ASP.NET 4.5 | [dotnet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | |
Python | | [python-webapp-graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
Java | | [java-webapp-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
PHP | | [php-graphapi-web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Aplicaciones web que muestran el control de acceso basado en roles (autorización)

Los siguientes ejemplos muestran cómo implementar el control de acceso basado en roles, que se utiliza para restringir los permisos de ciertas características de una aplicación web a determinados usuarios. Se autoriza a los usuarios dependiendo de si pertenecen a un grupo o un rol de Azure AD.

Plataforma | Muestra | DESCRIPCIÓN
 -------- | ------------------- | ---------------------
ASP.NET 4.5 | [dotnet-webapp-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Una aplicación web de .NET 4.5 MVC que usa **grupos** de Azure AD para la autorización
ASP.NET 4.5 | [dotnet-webapp-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Una aplicación web de .NET 4.5 MVC que usa **roles** de Azure AD para la autorización

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Aplicaciones demonio (que acceden a las API web con la identidad de la aplicación)

Los siguientes ejemplos muestran aplicaciones de escritorio o web que tienen acceso a Microsoft Graph o a una API web sin usuario (con la identidad de la aplicación).

Aplicación cliente | Plataforma | Flujo y concesión | Llama a Microsoft Graph | Llama a una API web de ASP.NET o de ASP.NET Core 2.0
------------------ | -------- | ---------- | -------------------- | -------------------------
Aplicación demonio (consola)          | .NET/C#  | Credenciales del cliente con secreto de la aplicación o certificado | | [dotnet-daemon](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [dotnet-daemon-certificate-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Aplicación demonio (consola)         | .NET Core/C# | Credenciales del cliente con certificado| | [dotnetcore-daemon-certificate-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
Escritorio            | Java | Credenciales de cliente |   [java-native-headless](https://github.com/azure-samples/active-directory-java-native-headless) |
Aplicación web para ASP.NET  | .NET/C# | Credenciales de cliente |    | [dotnet-webapp-webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>API web

### <a name="web-api-protected-by-azure-active-directory"></a>API web protegida por Azure Active Directory

En el ejemplo siguiente se muestra cómo proteger una API web de node.js con Azure AD.

Plataforma | Muestra | DESCRIPCIÓN
 -------- | ------------------- | ---------------------
Node.js | [node-webapi](https://github.com/Azure-Samples/active-directory-node-webapi) |  API web de NodeJS protegida mediante los tokens de acceso de Azure AD y OAuth 2.0.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>API web llamando a Microsoft Graph o a otra API web

Los ejemplos siguientes muestran una API web que llama a otra API web. El segundo ejemplo muestra cómo controlar el acceso condicional.

 Plataforma |  Llama a Microsoft Graph | Llama a otra API web de ASP.NET o de ASP.NET Core 2.0
 -------- |  --------------------- | -------------------------
ASP.NET 4.5 | [dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) |[dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof)
ASP.NET 4.5 | [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |[dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca)

## <a name="single-page-applications"></a>Aplicación de una sola página

En este ejemplo se muestra cómo escribir una aplicación de una sola página protegida con Azure AD.

 Plataforma |  Llama a Microsoft Graph | Llama a su propia API | Llama a otra API web
 -------- |  --------------------- | ------------------ | ----------------
JavaScript o ASP.NET 4.x |  | [javascript-singlepageapp](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
JavaScript (AngularJS) o ASP.NET 4.x |  | [angularjs-singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |
JavaScript (AngularJS) o ASP.NET 4.x |  |  | [angularjs-singlepageapp-cors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="other-microsoft-graph-samples"></a>Otros ejemplos de Microsoft Graph

Para obtener ejemplos y tutoriales en los que se muestren los diferentes patrones de uso de Microsoft Graph API, incluida la autenticación con Azure AD, consulte [Microsoft Graph Community samples & tutorials](https://github.com/microsoftgraph/msgraph-community-samples) (Ejemplos y tutoriales de la comunidad de Microsoft Graph).

## <a name="see-also"></a>Otras referencias

[Guía del desarrollador de Azure Active Directory](azure-ad-developers-guide.md)

[Conceptos y referencia de Graph API de Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteca auxiliar de Graph API de Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
