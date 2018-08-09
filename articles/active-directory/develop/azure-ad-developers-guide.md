---
title: Azure Active Directory para desarrolladores | Microsoft Docs
description: En este artículo se proporciona una introducción al inicio de sesión de las cuentas profesionales y educativas de Microsoft con Azure Active Directory.
services: active-directory
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 666a677943811af05cd3403eab4887271c1f87b3
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591217"
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory para desarrolladores

Azure Active Directory (Azure AD) es un servicio de identidad en la nube que permite a los desarrolladores compilar aplicaciones que inician la sesión de los usuarios en una cuenta profesional o educativa de Microsoft de forma segura. Azure AD admite tanto a desarrolladores que compilan aplicaciones de línea de negocio (LOB) de inquilino único como desarrolladores que desean desarrollar aplicaciones multiinquilino. Además del inicio de sesión básico, Azure AD también permite aplicaciones llamar tanto a API de Microsoft como [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/overview) como a API personalizadas que se basan en la plataforma de Azure AD. En esta documentación se muestra cómo agregar compatibilidad con Azure AD a una aplicación mediante protocolos estándar del sector, como OAuth2.0 y OpenID Connect.

> [!NOTE]
> La mayoría del contenido de esta página se centra en el punto de conexión de Azure AD v1.0, que solo admite cuentas profesionales o educativas de Microsoft. Si desea iniciar sesión con cuentas personales o consumidor de Microsoft, vea la información sobre el [punto de conexión de Azure AD v2.0](active-directory-appmodel-v2-overview.md). El punto de conexión de Azure AD v2.0 ofrece una experiencia de desarrollo unificada para las aplicaciones que desean iniciar sesión de los usuarios con cuentas de Azure AD (profesionales o educativas) y cuentas de Microsoft personales.

| | |
| --- | --- |
|[Conceptos básicos sobre autenticación](authentication-scenarios.md) | Una introducción a la autenticación con Azure AD. |
|[Tipos de aplicaciones](authentication-scenarios.md#application-types-and-scenarios) | Una introducción a los escenarios de autenticación que se admiten en Azure AD. |      
| | |

## <a name="get-started"></a>Introducción
Las configuraciones guiadas siguientes le conducirán por los pasos necesarios para compilar una aplicación en su plataforma preferida mediante el SDK de la Biblioteca de autenticación de Azure AD Authentication (ADAL). Si busca información acerca del uso de Microsoft Authentication Library (MSAL), consulte la documentación del [Punto de conexión de Azure AD v2.0](active-directory-appmodel-v2-overview.md).

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Aplicaciones móviles y de escritorio](./media/azure-ad-developers-guide/NativeApp_Icon.png)<br />Aplicaciones móviles y de escritorio</center> | [Información general](authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](quickstart-v1-ios.md)<br /><br />[Android](quickstart-v1-android.md) | [.NET (WPF)](quickstart-v1-dotnet.md)<br /><br />[Xamarin](quickstart-v1-xamarin.md) |
| <center>![Aplicaciones web](./media/azure-ad-developers-guide/Web_app.png)<br />Aplicaciones web</center> | [Información general](authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](quickstart-v1-aspnet-webapp.md)<br /><br />[Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) | [Python](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)<br/><br/> [Node.js](quickstart-v1-openid-connect-code.md) |
| <center>![Aplicaciones de una sola página](./media/azure-ad-developers-guide/SPA.png)<br />Aplicaciones de una sola página</center> | [Información general](authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](quickstart-v1-angularjs-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |
| <center>![API web](./media/azure-ad-developers-guide/Web_API.png)<br />API web</center> | [Información general](authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](quickstart-v1-dotnet-webapi.md)<br /><br />[Node.js](quickstart-v1-nodejs-webapi.md) | &nbsp; |
| <center>![Service-to-service](./media/azure-ad-developers-guide/Service_App.png)<br />De servicio a servicio</center> | [Información general](authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](sample-v1-code.md#daemon-applications-accessing-web-apis-with-the-applications-identity)|  |
|  |  |  |  |  |

## <a name="how-to-guides"></a>Guías de procedimientos
Estas guías le conducen por algunas de las tareas más comunes de Azure AD.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Registro de la aplicación](quickstart-v1-integrate-apps-with-azure-ad.md)           | Cómo registrar una aplicación en Azure AD. |
|[Aplicaciones multiinquilino](howto-convert-app-to-be-multi-tenant.md)    | Cómo iniciar la sesión de cualquier cuenta profesional de Microsoft. |
|[Protocolos OAuth y OpenID Connect](v1-protocols-openid-connect-code.md)| Cómo iniciar la sesión de los usuarios y llamar a las API web mediante los protocolos de autenticación de Microsoft. |
|  |  |

## <a name="reference-topics"></a>Temas de referencia
En los siguientes artículos se proporciona información detallada sobre las API, los mensajes de protocolo y los términos que se usan en Azure AD.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Bibliotecas de autenticación (ADAL)](active-directory-authentication-libraries.md)   | Una introducción a las bibliotecas y los SDK que se proporcionan en Azure AD. |
| [Ejemplos de código](sample-v1-code.md)                                  | Una lista de todos los ejemplos de código de Azure AD. |
| [Glosario](developer-glossary.md)                                      | Terminología y definiciones de palabras que se usan en esta documentación. |
|  |  |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
