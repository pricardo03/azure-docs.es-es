---
title: 'Aplicación de página única (pasar a producción): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación de página única (pasar a producción)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6a04f5d62ec750cfbe44765e833552bd694654d
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075016"
---
# <a name="single-page-application---move-to-production"></a>Aplicación de página única - pasar a producción

Ahora que sabe cómo adquirir un token para llamar a las API Web, obtenga información sobre cómo pasar a producción.

## <a name="improve-your-app"></a>Mejorar la aplicación

Siga los pasos necesarios para preparar la producción de la aplicación.

- [Habilitar el registro](msal-logging.md) en la aplicación.

## <a name="test-your-integration"></a>Probar la integración

- Probar la integración siguiendo el [lista de comprobación de integración de plataforma de Microsoft identity](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos otros ejemplos y tutoriales:

- Un análisis detallado de la Guía de inicio rápido de ejemplo que explica el código para los usuarios inicien sesión y obtener un token de acceso para llamar a Microsoft Graph API mediante MSAL.js

    > [!div class="nextstepaction"]
    > [Tutorial de SPA de JavaScript](./tutorial-v2-javascript-spa.md)

- Ejemplo que demuestra cómo obtener tokens para su propia API web de back-end mediante MSAL.js

     > [!div class="nextstepaction"]
     > [SPA con un back-end ASP.NET](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2)

- Ejemplo para mostrar cómo usar MSAL.js para iniciar sesión con usuarios en una aplicación registrada con Azure AD B2C

    > [!div class="nextstepaction"]
    > [SPA con B2C de Azure AD](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)
