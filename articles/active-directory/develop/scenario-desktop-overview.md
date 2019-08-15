---
title: 'Aplicación de escritorio que llama a las API web (información general): Plataforma de identidad de Microsoft'
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
ms.openlocfilehash: 5288fe57b2f83522b140f65fa62e08f9c63a7af5
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852686"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Escenario: Aplicación de escritorio que llama a las API web

Aprenda a crear una aplicación de escritorio que llame a las API web

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Si aún no lo ha hecho, cree su primera aplicación siguiendo el inicio rápido del escritorio de .NET o el inicio rápido de UWP:

> [!div class="nextstepaction"]
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de escritorio de Windows](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de UWP](./quickstart-v2-uwp.md)

## <a name="overview"></a>Información general

Escribe una aplicación de escritorio y desea que los usuarios inicien sesión en la aplicación y llama a las API web como Microsoft Graph, otras API de Microsoft o su propia API web. Tiene varias posibilidades:

- Puede utilizar la adquisición interactiva de tokens:

  - Si la aplicación de escritorio admite controles gráficos, por ejemplo, si es una aplicación Windows.Form o una aplicación WPF.
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
