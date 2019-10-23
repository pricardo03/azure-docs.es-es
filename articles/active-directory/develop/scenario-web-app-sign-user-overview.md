---
title: 'Aplicación web que permite iniciar sesión a los usuarios (introducción): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que permita iniciar sesión a los usuarios (introducción).
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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68d47d4233aec62ec5f1955e52025b0d55221af8
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72596712"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Escenario: Aplicación web que permite iniciar sesión a los usuarios

Aprenda todo lo que necesita para compilar una aplicación web que permita iniciar sesión a los usuarios con la Plataforma de identidad de Microsoft.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Si quiere crear sus primeras aplicaciones web portátiles (ASP.NET Core) que permitan iniciar sesión a los usuarios, siga este inicio rápido:

> [!div class="nextstepaction"]
> [Inicio rápido: aplicación web ASP.NET Core que permite iniciar sesión a los usuarios](quickstart-v2-aspnet-core-webapp.md)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Si desea saber cómo agregar el inicio de sesión a una aplicación web de ASP.NET heredada, pruebe el siguiente tutorial:

> [!div class="nextstepaction"]
> [Inicio rápido: aplicación web ASP.NET que permite iniciar sesión a los usuarios](quickstart-v2-aspnet-webapp.md)

# <a name="javatabjava"></a>[Java](#tab/java)

Si es un desarrollador de Java, pruebe el siguiente inicio rápido:

> [!div class="nextstepaction"]
> [Inicio rápido: Adición de inicio de sesión con Microsoft a una aplicación web de Java](quickstart-v2-java-webapp.md)

# <a name="pythontabpython"></a>[Python](#tab/python)

Si desarrolla con Python, pruebe lo siguiente:

> [!div class="nextstepaction"]
> [Inicio rápido: Adición del inicio de sesión con Microsoft a una aplicación web de Python](quickstart-v2-python-webapp.md)

---

## <a name="overview"></a>Información general

Agregue autenticación a la aplicación web para que permita iniciar sesión a los usuarios. La adición de autenticación permite a la aplicación web tener acceso a información de perfil limitada y, por ejemplo, personalizar la experiencia que ofrece a sus usuarios. Las aplicaciones web autentican un usuario en un explorador web. En este escenario, la aplicación web dirige el explorador del usuario para que inicie sesión en Azure AD. Azure AD devuelve una respuesta de inicio de sesión a través el explorador del usuario, que contiene notificaciones sobre el usuario en un token de seguridad. El inicio de sesión de los usuarios aprovecha el protocolo estándar [Open ID Connect](./v2-protocols-oidc.md) simplificado mediante el uso de [bibliotecas](scenario-web-app-sign-user-app-configuration.md#libraries-used-to-protect-web-apps) de middleware.

![Aplicación web que permite iniciar sesión a los usuarios](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

En una segunda fase, también puede habilitar la aplicación para llamar a las API web en nombre del usuario con sesión iniciada. Esta fase siguiente es un escenario diferente, que encontrará en [Aplicación web que llama a las API web](scenario-web-app-call-api-overview.md).

> [!NOTE]
> Agregar inicio de sesión a una aplicación web consiste en proteger la aplicación web y validar un token de usuario, que es lo que hacen las bibliotecas de **middleware**. En el caso de .NET, este escenario aún no requiere las bibliotecas de autenticación de Microsoft (MSAL), que se refieren a la adquisición de un token para llamar a las API protegidas. Las bibliotecas de autenticación solo se incluirán en el escenario de seguimiento cuando la aplicación web necesite llamar a las API web.

## <a name="specifics"></a>Características específicas

- Durante el registro de la aplicación, deberá proporcionar uno o varios (si implementa la aplicación en varias ubicaciones) URI de respuesta. En algunos casos (ASP.NET/ASP.NET Core), deberá habilitar el token del identificador. Por último, deberá configurar un URI de cierre de sesión para que la aplicación reaccione ante los usuarios que cierran sesión.
- En el código de la aplicación, deberá proporcionar la autoridad a la que la aplicación web delegada el inicio de sesión. Es posible que quiera personalizar la validación del token (en concreto, en escenarios de ISV).
- Las aplicaciones web admiten cualquier tipo de cuenta. Para obtener más información, consulte [Tipos de cuenta admitidos](v2-supported-account-types.md).

## <a name="next-steps"></a>Pasos siguientes

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Registro de aplicaciones](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=aspnetcore?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Registro de aplicaciones](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Registro de aplicaciones](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Registro de aplicaciones](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=python)

---
