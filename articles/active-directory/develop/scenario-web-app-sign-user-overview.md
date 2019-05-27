---
title: 'Aplicación Web que inician sesión los usuarios (información general): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que inician sesión los usuarios (Introducción)
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
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ce534c6eeecba220fd829be829caa679df52055
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65833095"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Escenario: Aplicación web que permite iniciar sesión a los usuarios

Aprenda todo lo que necesita para compilar una aplicación web que los usuarios inicia sesión con la plataforma Microsoft identity.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Si desea crear sus primeras aplicaciones de web (ASP.NET Core) portátiles que inician sesión en los usuarios, siga este tutorial rápido:

> [!div class="nextstepaction"]
> [Inicio rápido: Aplicación web ASP.NET Core que los usuarios inicia sesión](quickstart-v2-aspnet-core-webapp.md)

Si lo prefiere a quedarnos con ASP.NET, pruebe el tutorial siguiente:

> [!div class="nextstepaction"]
> [Inicio rápido: Aplicación web ASP.NET que los usuarios inicia sesión](quickstart-v2-aspnet-webapp.md)

## <a name="overview"></a>Información general

Agregar autenticación a la aplicación web, por lo que puede iniciar sesión en los usuarios. La adición de autenticación permite a la aplicación web tener acceso a información de perfil limitado y, por ejemplo personalizar la experiencia que ofrece a sus usuarios. Aplicaciones Web autentican un usuario en un explorador web. En este escenario, la aplicación web dirige el explorador del usuario para que inicie sesión en Azure AD. Azure AD devuelve una respuesta de inicio de sesión a través el explorador del usuario, que contiene notificaciones sobre el usuario en un token de seguridad. Inicio de sesión de los usuarios aprovechar la [Open ID Connect](./v2-protocols-oidc.md) protocolo estándar sí se simplifica mediante el uso de middleware [bibliotecas](scenario-web-app-sign-user-app-configuration.md#libraries-used-to-protect-web-apps).

![Aplicación inicia sesión de usuarios Web](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

También puede habilitar la aplicación llamar a las API Web en nombre del usuario con sesión iniciada como una segunda fase. Esta fase siguiente es un escenario diferente, que encontrará en [aplicación Web llama a las API Web](scenario-web-app-call-api-overview.md)

> [!NOTE]
> Agregar inicio de sesión a un sitio web app es sobre la protección de la aplicación web y validar un token de usuario, que es lo que **middleware** bibliotecas hacer. Este escenario no requiere aún la bibliotecas de autenticación de Microsoft (MSAL), que son cómo adquirir un token para llamar a las API protegidas. Las bibliotecas de autenticación sólo se incluirán en el escenario de seguimiento cuando la aplicación web necesita llamar a las API web.

## <a name="specifics"></a>Características específicas

- Durante el registro de aplicación, deberá proporcionar una o varias (si se implementa la aplicación en varias ubicaciones) responde a URI. En algunos casos (ASP.NET/ASP.NET núcleos), deberá habilitar el IDToken. Por último deseará configurar un URI de cierre de sesión para que la aplicación reacciona a los usuarios fuera de la firma.
- En el código de la aplicación, deberá proporcionar la entidad a la que se aplicación delegados inicio de sesión web. Es posible que desee personalizar la validación del token (en particular en escenarios de ISV).
- Aplicaciones Web, admiten los tipos de cuenta. Para obtener más información, consulte [admite tipos de cuenta](v2-supported-account-types.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-web-app-sign-user-app-registration.md)
