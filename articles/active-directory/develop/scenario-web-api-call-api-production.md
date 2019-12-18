---
title: 'Paso a producción de una API web que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a pasar a producción una API web que llama a las API web.
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
ms.openlocfilehash: 0d59a5b2a74c10e36103713725113cbe8c9cc412
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965176"
---
# <a name="web-api-that-calls-web-apis---move-to-production"></a>API web que llama a las API web: paso a producción

Una vez que haya obtenido un token para llamar a las API web, puede mover la aplicación a producción.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Más información

Ahora que conoce los conceptos básicos de cómo llamar a las API web desde su propia API web, puede que esté interesado en este tutorial, que describe el código que se usa para compilar una API web protegida que llama a otras API web.

| Muestra | Plataforma | DESCRIPCIÓN |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | API web de ASP.NET Core 2.2, escritorio (WPF) | API web de ASP.NET Core 2.2 que llama a Microsoft Graph, a la que se llama desde una aplicación de WPF con la Plataforma de identidad de Microsoft (v2.0) |
