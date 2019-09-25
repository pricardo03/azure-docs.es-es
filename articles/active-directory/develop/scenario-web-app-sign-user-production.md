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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd6717c132d32d54c16e7678695a09e35181a057
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086548"
---
# <a name="web-app-that-signs-in-users---move-to-production"></a>Aplicación web que permite iniciar sesión a los usuarios: paso a producción

Ahora que sabe cómo adquirir un token para llamar a las API web, obtenga información sobre cómo pasarla a producción.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

### <a name="calling-web-apis-scenario"></a>Escenario de llamada a API web

Una vez que la aplicación web inicie la sesión de los usuarios, puede llamar a las API web en nombre de los usuarios con sesión iniciada. Las llamadas a las API web desde la aplicación web es el objeto del escenario siguiente:

> [!div class="nextstepaction"]
> [Aplicación web que llama a las API web](scenario-web-app-call-api-overview.md)

### <a name="deep-dive---aspnet-core-web-app-tutorial"></a>En profundidad: Tutorial de una aplicación web de ASP.NET Core

Obtenga información sobre otras formas en que los usuarios pueden iniciar sesión con el tutorial de ASP.NET Core: [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial). Este ejemplo es un tutorial progresivo con código listo para producción para una aplicación web, e incluye la forma de agregar el inicio de sesión con cuentas en:

- su organización,
- varias organizaciones,
- cuentas profesionales o educativas o cuentas personales de Microsoft,
- con [Azure AD B2C](https://aka.ms/aadb2c),
- o en nubes nacionales.

### <a name="sample-code---java-web-app"></a>Código de ejemplo: aplicación web de Java

Obtenga más información sobre la aplicación web de Java en el ejemplo de GitHub: [Aplicación web de Java mediante la que los usuarios inician sesión con la plataforma de identidad de Microsoft y que llama a Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp)
