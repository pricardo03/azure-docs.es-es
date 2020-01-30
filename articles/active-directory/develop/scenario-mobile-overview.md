---
title: Compilación de una aplicación móvil que llama a las API web | Azure
titleSuffix: Microsoft identity platform
description: 'Obtener información sobre cómo compilar una aplicación móvil que llama a las API web: información general'
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 0a33b5bb4fc7220a9cf66f40e9805d3fde9fccef
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702052"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Escenario: Una aplicación móvil que llama a las API web

Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web.

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Cree su primera aplicación móvil y pruebe una guía de inicio rápido.

> [!div class="nextstepaction"]
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de Android](./quickstart-v2-android.md)
>
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de iOS](./quickstart-v2-ios.md)
>
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de Android y Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Información general

Para las aplicaciones móviles es esencial una experiencia de usuario personalizada y sin problemas.  La plataforma de identidad de Microsoft permite a los desarrolladores de aplicaciones móviles crear esa misma experiencia para usuarios de iOS y Android. La aplicación puede iniciar la sesión de los usuarios de Azure Active Directory (Azure AD), los usuarios de cuentas personales de Microsoft y los usuarios de Azure AD B2C, y adquirir tokens para llamar a una API web en su nombre. Para implementar estos flujos, se usará la biblioteca de autenticación de Microsoft (MSAL), que implementa el [flujo de código de autorización de OAuth2.0](v2-oauth2-auth-code-flow.md) estándar del sector.

![Aplicaciones de demonio](./media/scenarios/mobile-app.svg)

Consideraciones para las aplicaciones móviles:

- **La experiencia del usuario es clave**: Permita a los usuarios ver el valor de la aplicación antes de solicitar que inicien sesión y solicite solo los permisos necesarios.
- **Admitir todas las configuraciones de usuario**: Muchos usuarios empresariales móviles se encuentran bajo el acceso condicional y las directivas de cumplimiento del dispositivo. Asegúrese de que admite estos escenarios claves.
- **Implementación del inicio de sesión único (SSO)** : MSAL y la plataforma de identidad de Microsoft simplifican el inicio de sesión único a través del explorador del dispositivo o Microsoft Authenticator (Portal de empresa de Intune en Android).

## <a name="specifics"></a>Características específicas

Tenga en cuenta estas consideraciones al compilar una aplicación móvil en la plataforma de identidad de Microsoft:

- En función de la plataforma, podría ser necesaria una interacción del usuario la primera vez que los usuarios inicien sesión. Por ejemplo, iOS requiere que las aplicaciones muestren interacción del usuario cuando usan el inicio de sesión único por primera vez a través de Microsoft Authenticator (Portal de empresa de Intune en Android).
- En iOS y Android, MSAL podría utilizar un explorador externo (que podría aparecer sobre la aplicación) para iniciar la sesión de los usuarios. Puede personalizar la configuración para que use vistas web en la aplicación en su lugar.
- Nunca use un secreto en una aplicación móvil. Sería accesible para todos los usuarios.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-mobile-app-registration.md)
