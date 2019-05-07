---
title: 'Protected Web API: Introducción | Azure'
description: Obtenga información sobre cómo crear una API (información general) de web protegido.
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
ms.openlocfilehash: c5bc0075e6604bd1f94531040f2e4a0628e70667
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074896"
---
# <a name="scenario-protected-web-api"></a>Escenario: API web protegida

En este escenario, vamos a mostrarle cómo puede exponer una API web y cómo puede proteger para que los usuarios autenticados solo puede tener acceso a la API. Desea permitir que los usuarios autenticados con tanto trabajo y cuentas educativas o cuentas personales de Microsoft personales usar la API web.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Características específicas

Estos son algunos detalles que necesita saber para proteger las API web:

- El registro de aplicaciones debe exponer al menos un ámbito. La versión del token aceptada por la API web depende de la audiencia de inicio de sesión.
- La configuración del código de la API web debe validar el token que se usa al llamar a la API web.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-protected-web-api-app-registration.md)
