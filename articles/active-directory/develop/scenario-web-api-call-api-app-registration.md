---
title: 'Registro de una API web que llama a las API web: Plataforma de identidad de Microsoft | Azure'
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
ms.openlocfilehash: cb5f4763e13935b99564bfcb6d8b6e7f463ed59e
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74919807"
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
