---
title: 'Compilación de una aplicación de escritorio que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una aplicación de escritorio que llame a API web (información general)
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
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 545012629686e1fe3ece8a48ed852542e09e54fe
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965524"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Escenario: Aplicación de escritorio que llama a las API web

Aprenda a crear una aplicación de escritorio que llame a las API web

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Si aún no lo ha hecho, cree su primera aplicación siguiendo el inicio rápido de escritorio de .NET, el inicio rápido de UWP o el inicio rápido de aplicación nativa de macOS:

> [!div class="nextstepaction"]
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de escritorio de Windows](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de UWP](./quickstart-v2-uwp.md)

> [!div class="nextstepaction"]
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación nativa de macOS](./quickstart-v2-ios.md)

## <a name="overview"></a>Información general

Escribe una aplicación de escritorio y desea que los usuarios inicien sesión en la aplicación y llama a las API web como Microsoft Graph, otras API de Microsoft o su propia API web. Tiene varias posibilidades:

- Puede utilizar la adquisición interactiva de tokens:

  - Si la aplicación de escritorio admite controles gráficos, por ejemplo, si es una aplicación Windows.Form, una aplicación WPF o una aplicación nativa de macOS.
  - O si se trata de una aplicación .NET Core y está de acuerdo en que la interacción de autenticación con Azure AD suceda en el explorador del sistema

- En el caso de las aplicaciones hospedadas en Windows, también es posible que las aplicaciones que se ejecutan en equipos unidos a un dominio de Windows o unidos a AAD adquieran un token de forma silenciosa mediante el uso de la autenticación integrada de Windows.
- Por último, y aunque no es recomendable, puede usar el nombre de usuario y la contraseña en las aplicaciones clientes públicas. Todavía es necesario en algunos escenarios (como DevOps), pero tenga en cuenta que su uso impondrá restricciones a la aplicación. Por ejemplo, no puede iniciar sesión si el usuario necesita realizar la autenticación multifactor (acceso condicional). Además, la aplicación no se beneficiará del inicio de sesión único (SSO).

  También va en contra de los principios de la autenticación moderna y solo se proporciona por motivos de herencia.

  ![Aplicación de escritorio](media/scenarios/desktop-app.svg)

- Si está escribiendo una herramienta de línea de comandos portátil (probablemente una aplicación .NET Core que se ejecuta en Linux o Mac) y acepta que la autenticación se delegue en el explorador del sistema, podrá usar la autenticación interactiva. (.Net Core no proporciona todavía un [explorador web](https://aka.ms/msal-net-uses-web-browser) y, por lo tanto, la autenticación se produce en el explorador del sistema). De lo contrario, la mejor opción en ese caso es usar el flujo de código del dispositivo. Este flujo también se utiliza para las aplicaciones sin un explorador, como las aplicaciones de IoT.

  ![Aplicación sin explorador](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Características específicas

Las aplicaciones de escritorio tienen una serie de particularidades, que dependen principalmente de si la aplicación utiliza la autenticación interactiva o no.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de escritorio: registro de aplicaciones](scenario-desktop-app-registration.md)
