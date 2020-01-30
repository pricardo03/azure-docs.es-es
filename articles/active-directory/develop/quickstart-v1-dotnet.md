---
title: Inicio de sesión de usuarios & llamada a las instancias de Microsoft Graph API desde el escritorio de .NET (WPF) | Azure
description: Aprenda a crear una aplicación de escritorio de Windows .NET que se integra con Azure AD para el inicio de sesión y llama a las API protegidas de Azure AD mediante OAuth 2.0.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c35e44dff321c4a2f568855dacd827001a57f64a
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703820"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Inicio rápido: Inicio de sesión por los usuarios y llamada a Microsoft Graph API desde un escritorio de .NET

La [Plataforma de identidad de Microsoft](v2-overview.md) es una evolución de la plataforma para desarrolladores de Azure Active Directory (Azure AD). Permite a los desarrolladores compilar aplicaciones que inicien sesión en todas las identidades de Microsoft, obtener tokens para llamar a las API de Microsoft como Microsoft Graph, o a otras API que los desarrolladores hayan creado.

La [Biblioteca de autenticación de Microsoft (MSAL)](msal-overview.md) permite que los desarrolladores adquieran tokens desde el punto de conexión de la plataforma de identidad de Microsoft para acceder a API web protegidas. La Biblioteca de autenticación de Active Directory (ADAL) se integra con el punto de conexión de Azure AD para desarrolladores (v1.0), donde MSAL se integra con el punto de conexión de la Plataforma de identidad de Microsoft (v2.0).

En el caso de las nuevas aplicaciones de escritorio, se recomienda usar la Plataforma de identidad de Microsoft (v 2.0) y MSAL para adquirir tokens y acceder a API web protegidas: [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de escritorio de Windows](quickstart-v2-windows-desktop.md)
