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
ms.openlocfilehash: efcc7ed4f80d0e3e9750d19ff95d010052a08b87
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481880"
---
# <a name="web-app-that-signs-in-users-move-to-production"></a>Aplicación web que inicia sesión de usuarios: Paso a producción

Ahora que sabe cómo obtener un token para llamar a las API web, obtenga información sobre cómo pasarla a producción.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

### <a name="scenario-for-calling-web-apis"></a>Escenario para llamar a API web

Una vez que la aplicación web inicie la sesión de los usuarios, puede llamar a las API web en nombre de los usuarios con sesión iniciada. Las llamadas a las API web desde la aplicación web es el objeto del escenario siguiente:

> [!div class="nextstepaction"]
> [Aplicación web que llama a las API web](scenario-web-app-call-api-overview.md)

### <a name="deep-dive-aspnet-core-web-app-tutorial"></a>En profundidad: Tutorial de una aplicación web de ASP.NET Core

Obtenga información sobre otras maneras de iniciar la sesión de los usuarios con este tutorial de ASP.NET Core: 

> [!div class="nextstepaction"]
> [Habilitación de aplicaciones web para iniciar la sesión de los usuarios y llamar a las API con la Plataforma de identidad de Microsoft para desarrolladores](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) 

Este tutorial progresivo incluye código listo para producción para una aplicación web, e incluye la forma de agregar el inicio de sesión con cuentas en:

- su organización,
- varias organizaciones,
- cuentas profesionales o educativas o cuentas personales de Microsoft,
- [Azure AD B2C](https://aka.ms/aadb2c)
- Nubes nacionales

### <a name="sample-code-java-web-app"></a>Ejemplo de código: Aplicaciones web de Java

Obtenga más información sobre la aplicación web de Java en el ejemplo de GitHub: 

> [!div class="nextstepaction"]
> [Aplicación web de Java mediante la que los usuarios inician sesión con la plataforma de identidad de Microsoft y que llama a Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp)
