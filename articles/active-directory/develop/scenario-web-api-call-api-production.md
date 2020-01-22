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
ms.openlocfilehash: 1426ad250e18d0132e116162a374120dda2e1200
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044140"
---
# <a name="a-web-api-that-calls-web-apis-move-to-production"></a>API web que llama a API web: Paso a producción

Una vez que haya obtenido un token para llamar a las API web, puede mover la aplicación a producción.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Más información

Ahora que conoce los conceptos básicos de cómo llamar a las API web desde su propia API web, puede que esté interesado en el siguiente tutorial, que describe el código que se usa para compilar una API web protegida que llama a otras API web.

| Muestra | Plataforma | Descripción |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | API web de ASP.NET Core 2.2, escritorio (WPF) | La API web de ASP.NET Core 2.2 llama a Microsoft Graph, a la que se llama desde una aplicación de WPF con la Plataforma de identidad de Microsoft (v2.0). |
