---
title: 'Web API que web de bajada de llamadas API (registro de aplicación): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo crear una web API que web de bajada de llamadas API (registro de aplicación)
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
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075391"
---
# <a name="web-api-that-calls-web-apis---app-registration"></a>API que llama a web API: registro de la aplicación Web

Una API web que llama a la API web de bajada tiene el mismo registro como una API web protegida. Por lo tanto, deberá seguir las instrucciones de [API Web protegida mediante - registro de la aplicación](scenario-protected-web-api-app-registration.md).

Sin embargo, desde la aplicación web ahora se llama a web API, se convierte en una aplicación cliente confidencial. Por eso hay información de registro adicional necesaria: la aplicación necesita compartir secretos (credenciales de cliente) con la plataforma Microsoft identity.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Permisos de API

Aplicaciones Web, llamar a las API en nombre del usuario para el que se ha recibido el token de portador. Deben solicitar permisos delegados. Para obtener más información, consulte [agregar permisos para tener acceso a las API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la aplicación de código](scenario-web-api-call-api-app-configuration.md)
