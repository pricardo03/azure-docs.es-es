---
title: 'API web protegida: Información general | Azure'
description: Obtenga información sobre cómo compilar una API web protegida (información general).
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
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02bd4b84cc7542714f6db45c12c4b5b13a7fb449
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852579"
---
# <a name="scenario-protected-web-api"></a>Escenario: API web protegida

En este escenario, vamos a mostrarle cómo puede exponer una API web y cómo puede protegerla para que solo los usuarios autenticados puedan acceder a la API. Querrá permitir que los usuarios autenticados con cuentas profesionales y educativas, o cuentas personales de Microsoft, usen la API web.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Características específicas

Estos son algunos detalles que tiene que saber para proteger las API web:

- El registro de la aplicación debe exponer al menos un ámbito. La versión del token aceptada por la API web depende de la audiencia de inicio de sesión.
- La configuración del código de la API web debe validar el token que se usa al llamar a la API web.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-protected-web-api-app-registration.md)
