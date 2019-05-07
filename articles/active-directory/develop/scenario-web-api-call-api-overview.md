---
title: 'API Web que llama a nivel inferior web API (información general): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo crear una web API que web de bajada llamadas API (información general).
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
ms.openlocfilehash: 497134b7f3cc535f7b3f180db13cd04ef56787db
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075406"
---
# <a name="scenario-web-api-that-calls-web-apis"></a>Escenario: Web API que llama a las API web

Aprenda todo lo que necesita para crear una API web que llama a las API web.

## <a name="prerequisites"></a>Requisitos previos

Este escenario, la API web protegida que llama a web API, se basa en el escenario de "Proteger una API web". Para obtener más información sobre este escenario básico, consulte [API Web protegida mediante - escenario](scenario-protected-web-api-overview.md) primero.

## <a name="overview"></a>Información general

- Un cliente (web, escritorio, móvil o una página de aplicación) - no se representan en el diagrama siguiente: llama a una API web protegida y proporciona un token de portador JWT en el encabezado Http "Autorización".
- Valida el token de la API web protegida y usa la MSAL `AcquireTokenOnBehalfOf` método a petición (de Azure AD) otro token para que sea posible, sí, llamar a un segundo web API (denominada la API web de bajada) en nombre del usuario.
- La API web protegida usa este token para llamar a una API de nivel inferior. También puede llamar a `AcquireTokenSilent`posterior para solicitar tokens para otras API de nivel inferior (pero todavía en nombre del mismo usuario). `AcquireTokenSilent` actualiza el token cuando sea necesario.

![Una llamada a una API web de API Web](media/scenarios/web-api.svg)

## <a name="specifics"></a>Características específicas

La parte del registro de aplicación relacionados con los permisos de API es clásica. Configuración de la aplicación implica el uso del flujo de on-behalf-of de OAuth 2.0 para intercambiar el token de portador JWT en un token para una API de nivel inferior. Este token se agrega a la caché de tokens, que está disponible en los controladores de la API web y puede adquirir un token de forma silenciosa para llamar a las API de nivel inferiores.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-web-api-call-api-app-registration.md)
