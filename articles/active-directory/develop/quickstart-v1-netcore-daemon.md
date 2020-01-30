---
title: Adquisición de un token y llamada a Microsoft Graph (consola de .NET Core) (v1.0) | Azure
description: Cree una aplicación de demonio de .NET que se integre con Azure AD y que llame a las API protegidas de Azure AD mediante OAuth 2.0
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: jmprieur
ms.reviewer: ryanwi
ms.custom: aaddev
ms.openlocfilehash: bdcf0632110df23cbe91040f41535f51fb06ba44
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703803"
---
# <a name="quickstart-acquire-token-and-call-microsoft-graph-using-console-apps-identity-v10"></a>Inicio rápido: Adquisición de un token y llamada a Microsoft Graph mediante la identidad de la aplicación de consola (v1.0)

La [Plataforma de identidad de Microsoft](v2-overview.md) es una evolución de la plataforma para desarrolladores de Azure Active Directory (Azure AD). Permite a los desarrolladores compilar aplicaciones que inicien sesión en todas las identidades de Microsoft, obtener tokens para llamar a las API de Microsoft como Microsoft Graph, o a otras API que los desarrolladores hayan creado.

La [Biblioteca de autenticación de Microsoft (MSAL)](msal-overview.md) permite que los desarrolladores adquieran tokens desde el punto de conexión de la plataforma de identidad de Microsoft para acceder a API web protegidas. La Biblioteca de autenticación de Active Directory (ADAL) se integra con el punto de conexión de Azure AD para desarrolladores (v1.0), donde MSAL se integra con el punto de conexión de la Plataforma de identidad de Microsoft (v2.0).

## <a name="next-steps"></a>Pasos siguientes

En el caso de las nuevas aplicaciones de demonio de .NET, se recomienda usar la Plataforma de identidad de Microsoft (v 2.0) y MSAL para adquirir tokens y acceder a API web protegidas: [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de consola mediante la identidad de la aplicación](quickstart-v2-netcore-daemon.md).
