---
title: 'API web que llama a API web de bajada (registro de la aplicación): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una API web que llame a API web de bajada (registro de la aplicación).
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
ms.openlocfilehash: fb03869cdea2150b6e922e2d6d81e577c3be02da
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075391"
---
# <a name="web-api-that-calls-web-apis---app-registration"></a>Aplicación API que llama a API web: registro de la aplicación

Una API web que llama a API web de bajada tiene el mismo registro que una API web protegida. Por lo tanto, deberá seguir las instrucciones de [API web protegida: Registro de aplicación](scenario-protected-web-api-app-registration.md).

Sin embargo, dado que la aplicación web ahora llama a las API web, se convierte en una aplicación cliente confidencial. Por eso se exigen pasos adicionales de registro: la aplicación tiene que compartir secretos (credenciales de cliente) con la Plataforma de identidad de Microsoft.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Permisos de API

Las aplicaciones web llaman a las API en nombre del usuario para el que se ha recibido el token de portador. Tienen que solicitar permisos delegados. Para más detalles, consulte [Adición de permisos para acceder a las API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración del código de la aplicación](scenario-web-api-call-api-app-configuration.md)
