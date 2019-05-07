---
title: 'Aplicación móvil que llama a web API: Introducción | Plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación móvil que llama a web API (información general)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb0acd1534bab11eb57a7aa0e695f192b5999ed2
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076501"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Escenario: Aplicaciones móviles que las llamadas API web

Aprenda todo lo que necesita para compilar una aplicación móvil que llama a las API web.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Cree su primera aplicación móvil y pruebe un inicio rápido.

> [!div class="nextstepaction"]
> [Inicio rápido: Adquirir un token y llamar a Microsoft Graph API desde una aplicación de Android](./quickstart-v2-android.md)
>
> [Inicio rápido: Adquirir un token y llamar a Microsoft Graph API desde una aplicación de iOS](./quickstart-v2-ios.md)
>
> [Inicio rápido: Adquirir un token y llamar a Microsoft Graph API desde un aplicación de Android y Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Información general

Al compilar una aplicación móvil, es esencial una experiencia de usuario final personalizada y sin problemas.  Plataforma de identidad de Microsoft permite a los desarrolladores móviles hacer esto para iOS y Android a los usuarios. La aplicación puede iniciar sesión en Azure AD, cuenta Microsoft personal y los usuarios de Azure AD B2C y adquirir tokens para llamar a una API web en su nombre. Para implementar estos flujos, vamos a usar la biblioteca de autenticación de Microsoft (MSAL) que implementa el estándar del sector [flujo de código de autorización de OAuth2.0](v2-oauth2-auth-code-flow.md).

![Aplicaciones de demonio](./media/scenarios/mobile-app.svg)

Consideraciones sobre la aplicación móvil:

- ***Experiencia del usuario es clave***: Permitir a los usuarios ver el valor de la aplicación antes de solicitar el inicio de sesión y solicitar solo los permisos necesarios.
- ***Admite todas las configuraciones de usuario***: Muchos usuarios empresariales móviles están bajo el acceso condicional y las directivas de cumplimiento del dispositivo. No olvide admitir estos escenarios claves.
- ***Implementar el inicio de sesión único (SSO)***: MSAL y la plataforma Microsoft identity simplifican habilitar inicio de sesión único a través del explorador del dispositivo o el Microsoft Authenticator (Portal de empresa de Intune en Android).

## <a name="specifics"></a>Características específicas

Al compilar una aplicación móvil en la plataforma Microsoft identity, la experiencia de to-end tiene algunas consideraciones:

- Según la plataforma, iniciar sesión sin ninguna interacción no sea posible en el primer inicio de sesión. iOS, por ejemplo, requiere que las aplicaciones para mostrar la interacción del usuario al obtener la hora de inicio de sesión único la primera a través de Microsoft Authenticator (Portal de empresa de Intune en Android).
- En iOS y Android, MSAL puede usar un exploradores externos (que pueden aparecer encima de la aplicación) para los usuarios inicien sesión. Esto se puede personalizar para utilizar vistas Web en la aplicación en su lugar.
- Nunca use un secreto en una aplicación móvil, estarán disponibles para todos los usuarios.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-mobile-app-registration.md)
