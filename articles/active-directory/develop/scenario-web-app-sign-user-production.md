---
title: 'Aplicación Web que los usuarios inicia sesión (pasar a producción): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que inician sesión los usuarios (pasar a producción)
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
ms.openlocfilehash: d41ad2518f885bbaa02dda3b01f0c02e9fc1d217
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074716"
---
# <a name="web-app-that-signs-in-users---move-to-production"></a>Aplicación Web que inicie sesión en los usuarios: pasar a producción

Ahora que sabe cómo adquirir un token para llamar a las API web, obtenga información sobre cómo moverla a producción.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

### <a name="calling-web-apis-scenario"></a>Escenario de API web que realiza la llamada

Una vez que la aplicación inicia sesión los usuarios web, puede llamar a las API web en nombre de los usuarios con sesión iniciada. Llamar a web API desde la aplicación web es el objeto de la situación siguiente:

> [!div class="nextstepaction"]
> [Aplicación Web que llama a las API web](scenario-web-app-call-api-overview.md)

### <a name="deep-dive---web-app-tutorial"></a>Profundización: tutorial de aplicación web

Obtenga información sobre otras formas de inicio de sesión de usuarios con el tutorial de ASP.NET Core: [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial). Este es un tutorial progresivo con código de producción listo para una aplicación web, incluida la forma de agregar el inicio de sesión.

<!--- Removed the diagram as it's already shown in the above link to GitHub

![Tutorial overview](media/scenarios/aspnetcore-webapp-tutorial.svg)

--->
