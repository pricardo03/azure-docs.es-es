---
title: 'Aplicación de página única (paso a producción): Plataforma de identidad de Microsoft'
description: Aprenda a compilar una aplicación de página única (paso a producción)
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075016"
---
# <a name="single-page-application---move-to-production"></a>Aplicación de página única: paso a producción

Ahora que sabe cómo adquirir un token para llamar a las API web, obtenga información sobre cómo pasar a producción.

## <a name="improve-your-app"></a>Mejorar la aplicación

Siga los pasos necesarios para preparar la aplicación para producción.

- [Habilite el registro](msal-logging.md) en su aplicación.

## <a name="test-your-integration"></a>Probar la integración

- Pruebe la integración siguiendo la [lista de comprobación de integración de la Plataforma de identidad de Microsoft](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Pasos siguientes

Aquí tiene algunos otros ejemplos y tutoriales:

- Para profundizar en la guía de inicio rápido de ejemplo que explica el código para iniciar la sesión de los usuarios y obtener un token de acceso para llamar a Microsoft Graph API mediante MSAL.js

    > [!div class="nextstepaction"]
    > [Tutorial de SPA de JavaScript](./tutorial-v2-javascript-spa.md)

- Ejemplo que demuestra cómo obtener tokens para su propia API web de back-end mediante MSAL.js

     > [!div class="nextstepaction"]
     > [SPA con back-end ASP.NET](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2)

- Ejemplo para mostrar cómo usar MSAL.js para iniciar la sesión de los usuarios en una aplicación registrada en Azure AD B2C

    > [!div class="nextstepaction"]
    > [SPA con Azure AD B2C](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)
