---
title: Adición de inicio de sesión con Microsoft a una aplicación web ASP.NET | Microsoft Docs
description: Obtenga información sobre cómo agregar inicio de sesión de Microsoft en una solución ASP.NET con una aplicación basada en un explorador web tradicional mediante el estándar OpenID Connect.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.topic: quickstart
ms.workload: identity
ms.date: 10/25/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.openlocfilehash: 91c697cd11c0d411b7805e22be4cf70fe2a3e763
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703888"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Inicio rápido: Adición de inicio de sesión con Microsoft a una aplicación web ASP.NET

La [Plataforma de identidad de Microsoft](v2-overview.md) es una evolución de la plataforma para desarrolladores de Azure Active Directory (Azure AD). Permite a los desarrolladores compilar aplicaciones que inicien sesión en todas las identidades de Microsoft, obtener tokens para llamar a las API de Microsoft como Microsoft Graph, o a otras API que los desarrolladores hayan creado.

La [Biblioteca de autenticación de Microsoft (MSAL)](msal-overview.md) permite que los desarrolladores adquieran tokens desde el punto de conexión de la plataforma de identidad de Microsoft para acceder a API web protegidas. La Biblioteca de autenticación de Active Directory (ADAL) se integra con el punto de conexión de Azure AD para desarrolladores (v1.0), donde MSAL se integra con el punto de conexión de la Plataforma de identidad de Microsoft (v2.0).

## <a name="next-steps"></a>Pasos siguientes

En el caso de las nuevas aplicaciones web, se recomienda usar la Plataforma de identidad de Microsoft (versión 2.0) y MSAL para adquirir tokens y acceder a API web protegidas. Consulte [Quickstart: incorporación del inicio de sesión con Microsoft a una aplicación web ASP.NET](quickstart-v2-aspnet-webapp.md) para comenzar.
