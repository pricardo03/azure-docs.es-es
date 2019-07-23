---
title: 'Aplicación de demonio que llama a API web (llamada a API web): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación de demonio que llama a las API web (llamada a API web)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: aff375f996126d9e8b64361fc0e5673c25d30c19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65076276"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Aplicación de demonio que llama a las API web: llamada a una API web desde la aplicación

Una aplicación de demonio puede llamar a una API web desde una aplicación de demonio de .NET o llamar a varias API web aprobadas previamente.

## <a name="calling-a-web-api-from-a-net-daemon-application"></a>Llamada a una API web desde una aplicación de demonio .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis"></a>Llamada a varias API

Para las aplicaciones de demonio, las API web a las que llame deben estar aprobadas previamente. No habrá ningún consentimiento incremental con las aplicaciones de demonio (no hay ninguna interacción del usuario). El administrador de inquilinos debe dar el consentimiento previo a la aplicación y a todos los permisos de la API. Si quiere llamar a varias API, deberá adquirir un token para cada recurso, cada vez llamando a `AcquireTokenForClient`. MSAL usará la caché de tokens de aplicación para evitar las llamadas de servicio innecesarias.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: paso a producción](./scenario-daemon-production.md)