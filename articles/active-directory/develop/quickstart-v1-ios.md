---
title: Inicio de sesión y llamada a Microsoft Graph desde iOS | Azure
description: Aprenda a hacer que los usuarios inicien sesión y a llamar a Microsoft Graph API desde una aplicación de iOS.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: quickstart
ms.date: 10/25/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: brandwe
ms.openlocfilehash: 0f612a6f59455162d0acdffc8e05201be0a730d9
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703786"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app-v10"></a>Inicio rápido: inicio de sesión por los usuarios y llamada a Microsoft Graph API desde una aplicación de iOS (v1.0)

La [Plataforma de identidad de Microsoft](v2-overview.md) es una evolución de la plataforma para desarrolladores de Azure Active Directory (Azure AD). Permite a los desarrolladores compilar aplicaciones que inicien sesión en todas las identidades de Microsoft, obtener tokens para llamar a las API de Microsoft como Microsoft Graph, o a otras API que los desarrolladores hayan creado.

La [Biblioteca de autenticación de Microsoft (MSAL)](msal-overview.md) permite que los desarrolladores adquieran tokens desde el punto de conexión de la plataforma de identidad de Microsoft para acceder a API web protegidas. La Biblioteca de autenticación de Active Directory (ADAL) se integra con el punto de conexión de Azure AD para desarrolladores (v1.0), donde MSAL se integra con el punto de conexión de la Plataforma de identidad de Microsoft (v2.0).

En el caso de las nuevas aplicaciones de iOS y macOS, se recomienda usar la Plataforma de identidad de Microsoft (v 2.0) y MSAL para adquirir tokens y acceder a API web protegidas: [Inicio rápido: Inicio de sesión de los usuarios y llamada a Microsoft Graph API desde una aplicación de iOS o macOS](quickstart-v2-ios.md).
