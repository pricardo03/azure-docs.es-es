---
title: 'Aplicación móvil que permite iniciar sesión a los usuarios (paso a producción): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que permita iniciar sesión a los usuarios (paso a producción)
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074716"
---
# <a name="web-app-that-signs-in-users---move-to-production"></a>Aplicación web que permite iniciar sesión a los usuarios: paso a producción

Ahora que sabe cómo adquirir un token para llamar a las API web, obtenga información sobre cómo pasarla a producción.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

### <a name="calling-web-apis-scenario"></a>Escenario de llamada a API web

Una vez que la aplicación web inicie la sesión de los usuarios, puede llamar a las API web en nombre de los usuarios con sesión iniciada. Las llamadas a las API web desde la aplicación web es el objeto del escenario siguiente:

> [!div class="nextstepaction"]
> [Aplicación web que llama a las API web](scenario-web-app-call-api-overview.md)

### <a name="deep-dive---web-app-tutorial"></a>Tutorial a fondo para aplicación web

Obtenga información sobre otras formas en que los usuarios pueden iniciar sesión con el tutorial de ASP.NET Core: [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial). Este es un tutorial progresivo con código listo para producción para una aplicación web, e incluye la forma de agregar el inicio de sesión.

<!--- Removed the diagram as it's already shown in the above link to GitHub

![Tutorial overview](media/scenarios/aspnetcore-webapp-tutorial.svg)

--->
