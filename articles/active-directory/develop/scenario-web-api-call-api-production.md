---
title: 'Web API que llama a las API web (pasar a producción): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo crear una web API que las llamadas de bajada web API (pasar a producción).
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
ms.openlocfilehash: 7de892a0851e556c5046fe4b3691f39b42a9d237
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074761"
---
# <a name="web-api-that-calls-web-apis---move-to-production"></a>Web API que llama a web API: pasar a producción

Una vez que ha obtenido un token para llamar a las API web, puede mover la aplicación a producción.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Más información

Ahora que conoce los conceptos básicos de cómo llamar a las API web desde su propia API web, puede que esté interesado en este tutorial, que describe el código que ha usado para crear una API web protegida llama a las API web.

| Muestra | Plataforma | DESCRIPCIÓN |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | 2.2 API Web ASP.NET Core, el escritorio (WPF) | 2.2 de ASP.NET Core Web API, una llamada a Microsoft Graph, sí se llama desde una aplicación de WPF mediante la plataforma de identidad de Microsoft (v2.0) |
