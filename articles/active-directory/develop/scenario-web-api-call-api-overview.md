---
title: 'Compilación de una API web que llama a otras API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una API web que llame a API web de bajada (información general).
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
ms.openlocfilehash: 4b357def86b77d4bbb294e2253dacfbd129998ec
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965133"
---
# <a name="scenario-web-api-that-calls-web-apis"></a>Escenario: API web que llama a las API web

Aprenda a crear una API web que llame a las API web.

## <a name="prerequisites"></a>Requisitos previos

Este escenario, una API web protegida que llama a otras API web, se basa en el escenario de "Proteger una API web". Para más información sobre este escenario básico, consulte primero [Escenario: API web protegida](scenario-protected-web-api-overview.md).

## <a name="overview"></a>Información general

- Un cliente (aplicación web, de escritorio, móvil o de página única) —no se representa en el diagrama siguiente— llama a una API web protegida y proporciona un token de portador JWT en el encabezado HTTP "Autorización".
- La API web protegida valida el token y usa el método MSAL `AcquireTokenOnBehalfOf` para solicitar (a Azure AD) otro token para poder, por sí, llamar a una segunda API web (denominada la API web de bajada) en nombre del usuario.
- La API web protegida usa este token para llamar a una API de bajada. También puede llamar `AcquireTokenSilent`más tarde para solicitar tokens para otras API de bajada (pero todavía en nombre del mismo usuario). `AcquireTokenSilent` actualiza el token cuando sea necesario.

![API web que llama a una API web](media/scenarios/web-api.svg)

## <a name="specifics"></a>Características específicas

La parte del registro de aplicación relacionado con los permisos de la API es clásica. La configuración de la aplicación implica el uso de OAuth 2.0 en nombre del flujo para intercambiar el token de portador JWT con un token para una API de bajada. Este token se agrega a la caché de tokens, que está disponible en los controladores de la API web, y puede adquirir un token de forma silenciosa para llamar a las API de bajada.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-web-api-call-api-app-registration.md)
