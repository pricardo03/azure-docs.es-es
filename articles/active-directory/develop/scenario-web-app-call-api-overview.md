---
title: 'Aplicación web que llama a las API web (información general): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que llame a API web (información general)
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
ms.openlocfilehash: ce45f11a697b72ebdd0fe01166a70e7b47aa8e9f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65076306"
---
# <a name="scenario-web-app-that-calls-web-apis"></a>Escenario: Aplicación web que llama a las API web

Obtenga información sobre cómo compilar una aplicación web que permita iniciar sesión a los usuarios en la Plataforma de identidad de Microsoft y que llame a las API web en nombre del usuario con sesión iniciada.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

Este escenario supone que ha pasado por el escenario siguiente:

> [!div class="nextstepaction"]
> [Aplicación web que permite iniciar sesión a los usuarios](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Información general

Agrega autenticación a la aplicación web, que permite, por tanto, iniciar sesión a los usuarios y llama a una API web en nombre del usuario con sesión iniciada.

![Aplicación web que llama a las API web](./media/scenario-webapp/web-app.svg)

Las aplicaciones web que llaman a las API web:

- Son aplicaciones cliente confidenciales.
- Por este motivo han registrado un secreto (contraseña de la aplicación o certificado) en Azure AD. Este secreto se pasa durante la llamada a Azure AD para obtener un token.

## <a name="specifics"></a>Características específicas

> [!NOTE]
> Agregar inicio de sesión a una aplicación web no usa las bibliotecas MSAL, ya que esto se trata de proteger la aplicación web. La protección de las bibliotecas se logra mediante bibliotecas denominadas Middleware. Este era el objeto del escenario anterior [Inicio de sesión de usuarios en una aplicación web](scenario-web-app-sign-user-overview.md)
>
> Al llamar a las API web desde una aplicación web, deberá obtener tokens de acceso para estas API web. Puede usar las bibliotecas MSAL para adquirir estos tokens.

La experiencia completa de los desarrolladores para este escenario tiene, por lo tanto, aspectos específicos, por ejemplo:

- Durante el [registro de la aplicación](scenario-web-app-call-api-app-registration.md), deberá proporcionar uno o varios (si implementa la aplicación en varias ubicaciones) URI de respuesta, secretos o certificados que deben compartirse con Azure AD.
- La [configuración de la aplicación](scenario-web-app-call-api-app-configuration.md) tiene que proporcionar las credenciales de cliente tal cual se comparten con Azure AD durante el registro de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-web-app-call-api-app-registration.md)
