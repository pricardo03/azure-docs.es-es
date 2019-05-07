---
title: 'Aplicación Web que llama a web API (información general): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que llama a web API (información general)
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
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076306"
---
# <a name="scenario-web-app-that-calls-web-apis"></a>Escenario: Aplicación Web que llama a las API web

Obtenga información sobre cómo compilar una aplicación web de inicio de sesión a los usuarios en la plataforma de identidad de Microsoft y llama a las API web en nombre del usuario con sesión iniciada.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

Este escenario supposes que ha pasado a través de la situación siguiente:

> [!div class="nextstepaction"]
> [Aplicación Web que los usuarios inicia sesión](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Información general

Agregar autenticación a la aplicación Web, que puede, por tanto, los usuarios inicien sesión y llama a una API web en nombre del usuario con sesión iniciada.

![Aplicación Web que llama a las API web](./media/scenario-webapp/web-app.svg)

Las aplicaciones Web que llama a las API web:

- Son aplicaciones de cliente confidencial.
- por este motivo ha registrado un secreto (contraseña de la aplicación o certificado) con Azure AD. Este secreto es pasado durante la llamada a Azure AD para obtener un token

## <a name="specifics"></a>Características específicas

> [!NOTE]
> Agregar inicio de sesión a una aplicación Web no utiliza las bibliotecas MSAL tal como se trata sobre cómo proteger la aplicación Web. Protección de las bibliotecas se logra mediante bibliotecas denominadas Middleware. Esto era el objeto del escenario anterior [inicio de sesión a los usuarios a una aplicación Web](scenario-web-app-sign-user-overview.md)
>
> Al llamar a las API web desde una aplicación Web, deberá obtener tokens de acceso para estas API web. Puede usar las bibliotecas MSAL para adquirir estos tokens.

La experiencia de principio a fin de desarrolladores para este escenario tiene, por lo tanto, los aspectos específicos como:

- Durante la [registro de la aplicación](scenario-web-app-call-api-app-registration.md), deberá proporcionar uno, o varios (si se implementa la aplicación en varias ubicaciones) responder URI, secretos, o certificados deben compartirse con Azure AD.
- El [configuración de la aplicación](scenario-web-app-call-api-app-configuration.md) tiene que proporcionar credenciales de cliente como compartido con Azure AD durante el registro de aplicación

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-web-app-call-api-app-registration.md)
