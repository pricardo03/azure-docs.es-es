---
title: 'Aplicación Web que llama a web API (registro de aplicación): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que llama a web API (registro de aplicación)
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
ms.openlocfilehash: 5becdc287f7cad28aa6c7c07ebc107586e9a2b2a
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075196"
---
# <a name="web-app-that-calls-web-apis---app-registration"></a>Aplicación Web que llama a web API: registro de la aplicación

Un web que realiza la llamada de la aplicación de Web API tiene el mismo registro como una aplicación Web de firma de los usuarios. Por lo tanto, deberá seguir las instrucciones de [aplicación Web que inicia sesión los usuarios - registro de aplicación](scenario-web-app-sign-user-app-registration.md)

Sin embargo desde la aplicación Web ahora se llama a web API, se convierte en una aplicación cliente confidencial. Por eso hay un poco de registro adicional necesario: necesita compartir secretos (credenciales de cliente) con la plataforma Microsoft identity.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Permisos de API

Aplicaciones Web, llamar a las API en nombre del usuario con sesión iniciada. Deben solicitar permisos delegados. Para obtener información detallada, consulte [agregar permisos para tener acceso a las API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la aplicación de código](scenario-web-app-call-api-app-configuration.md)
