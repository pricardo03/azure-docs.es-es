---
title: Introducción a Xamarin de Azure AD | Microsoft Docs
description: Cree aplicaciones de Xamarin que se integren con Azure AD para el inicio de sesión y la llamada a las API protegidas por Azure AD mediante OAuth.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 26eff9456ba3ee1e2ff5ea41a552b4ec90a4fd1f
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703701"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>Inicio rápido: Creación de una aplicación de Xamarin que se integra con el inicio de sesión de Microsoft

La [Plataforma de identidad de Microsoft](v2-overview.md) es una evolución de la plataforma para desarrolladores de Azure Active Directory (Azure AD). Permite a los desarrolladores compilar aplicaciones que inicien sesión en todas las identidades de Microsoft, obtener tokens para llamar a las API de Microsoft como Microsoft Graph, o a otras API que los desarrolladores hayan creado.

La [Biblioteca de autenticación de Microsoft (MSAL)](msal-overview.md) permite que los desarrolladores adquieran tokens desde el punto de conexión de la plataforma de identidad de Microsoft para acceder a API web protegidas. La Biblioteca de autenticación de Active Directory (ADAL) se integra con el punto de conexión de Azure AD para desarrolladores (v1.0), donde MSAL se integra con el punto de conexión de la Plataforma de identidad de Microsoft (v2.0).

## <a name="next-steps"></a>Pasos siguientes

En el caso de las aplicaciones de Xamarin nuevas, se recomienda usar la Plataforma de identidad de Microsoft (versión 2.0) y MSAL para adquirir tokens y acceder a API web protegidas. Consulta [Integración de la identidad de Microsoft y Microsoft Graph en una aplicación de Xamarin Forms con MSAL](https://github.com/azure-samples/active-directory-xamarin-native-v2#integrate-microsoft-identity-and-the-microsoft-graph-into-a-xamarin-forms-app-using-msal) (sin los pasos opcionales) para comenzar.
