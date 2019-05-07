---
title: Web que realiza la llamada de la aplicación demonio APIs (web que realiza la llamada API) - plataforma Microsoft identity
description: Obtenga información sobre cómo compilar una aplicación demonio que llama a web API (API web que realiza la llamada)
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
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076276"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Aplicación de demonio que llama a web API: llamar a una API web desde la aplicación

Una aplicación demonio puede llamar a una API web desde una aplicación de demonio de .NET o llamar a varias API de web aprobados previamente.

## <a name="calling-a-web-api-from-a-net-daemon-application"></a>Llamar a una API web desde una aplicación de demonio de .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis"></a>Llamar a varias API

Para las aplicaciones de demonio, las API web que se llama a deben aprobarse previamente. No habrá ningún consentimiento incremental con aplicaciones de demonio (no hay ninguna interacción del usuario). El Administrador de inquilinos debe consentimiento previo de la aplicación y todos los permisos de API. Si desea llamar a varias API, deberá adquirir un token para cada recurso, cada vez que llama a `AcquireTokenForClient`. MSAL utilizará la caché de tokens de aplicación para evitar llamadas innecesarias de servicio.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: pasar a producción](./scenario-daemon-production.md)