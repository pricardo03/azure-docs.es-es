---
title: 'Aplicación de escritorio que llama a web API (información general): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación de escritorio que llama a web API (información general)
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
ms.openlocfilehash: 44d31011ca70bbebaf994b5fb80a45eee8dbde40
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076951"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Escenario: Aplicación de escritorio que llama a las API web

Aprenda todo lo que necesita para compilar una aplicación de escritorio que llama a las API web

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Si no lo ha hecho ya, cree su primera aplicación siguiendo la Guía de inicio rápido de escritorio de .NET o la Guía de inicio rápido UWP:

> [!div class="nextstepaction"]
> [Inicio rápido: Adquirir un token y llamar a Microsoft Graph API desde una aplicación de escritorio de Windows](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Inicio rápido: Adquirir un token y llamar a Microsoft Graph API desde una aplicación para UWP](./quickstart-v2-uwp.md)

## <a name="overview"></a>Información general

Escribir una aplicación de escritorio, y desea iniciar sesión en su aplicación a los usuarios y llamar a web API como Microsoft Graph, otros APIs Microsoft o su propia API web. Tiene varias posibilidades:

- Si su aplicación de escritorio es compatible con los controles de gráficos, por ejemplo si es una aplicación Windows.Form o una aplicación de WPF, puede usar la adquisición de tokens interactiva.
- Para las aplicaciones de Windows hospedado, también es posible que las aplicaciones que se ejecutan en los equipos unidos a un dominio de Windows o unido AAD para adquirir un token en modo silencioso utilizando la autenticación integrada de Windows.
- Por último, y aunque no se recomienda, puede usar el nombre de usuario y contraseña en las aplicaciones de cliente público. Sigue siendo necesario en algunos escenarios (como DevOps), pero tenga en cuenta que usarlo se imponen restricciones en la aplicación. Por ejemplo, no puede iniciar sesión en los usuarios que necesiten para realizar la autenticación multifactor (acceso condicional). También la aplicación no se beneficiará de inicio de sesión único (SSO).

  Es también contra los principios de la autenticación moderna y solo se proporciona por motivos de herencia.

  ![Aplicación de escritorio](media/scenarios/desktop-app.svg)

- Si está escribiendo una herramienta de línea de comandos portable - probablemente una aplicación .NET Core que se ejecutan en Linux o Mac, no podrá usar la autenticación interactiva de ningún (como .NET Core no proporciona un [explorador Web](https://aka.ms/msal-net-uses-web-browser)), ni integrado Autenticación de Windows. En ese caso, la mejor opción es utilizar el flujo de código de dispositivo. Este flujo también se usa para las aplicaciones sin un explorador, como las aplicaciones de iOT

  ![Aplicación browserless](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Características específicas

Las aplicaciones de escritorio tienen un número de particularidades, que depende principalmente de si la aplicación utiliza la autenticación interactiva o no.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de escritorio - registro de aplicación](scenario-desktop-app-registration.md)
