---
title: 'Introducción al escenario de aplicación de página única JavaScript: plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación de página única (Introducción a los escenarios) que se integra la plataforma Microsoft identity.
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
ms.openlocfilehash: 07a21e83f304f3e1acc0ed4033d832dd8e901ac9
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076366"
---
# <a name="scenario-single-page-application"></a>Escenario: Aplicación de una sola página

Aprenda todo lo que necesita para crear una aplicación de página única (SPA).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Puede crear su primera aplicación siguiendo la Guía de inicio rápido de SPA de JavaScript:

> [!div class="nextstepaction"]
> [Inicio rápido: Aplicación de página única](./quickstart-v2-javascript.md)

## <a name="overview"></a>Información general

Muchas aplicaciones web modernas se crean como aplicaciones de una página del lado cliente escritas con JavaScript o un marco de SPA como Angular, Vue.js y React.js. Estas aplicaciones se ejecutan en un explorador web y tienen las características de autenticación diferente que las aplicaciones web tradicionales de lado servidor. La plataforma Microsoft identity permite que las aplicaciones de página única para los usuarios inicien sesión y obtener tokens para tener acceso a servicios back-end o la API web mediante la [flujo implícito de OAuth 2.0](./v2-oauth2-implicit-grant-flow.md). El flujo implícito permite que la aplicación obtener los tokens de identificador para representar al usuario autenticado y también tener acceso a los tokens es necesarios llamar a las API protegidas.

![Aplicación de página única](./media/scenarios/spa-app.svg)

Este flujo de autenticación no incluye escenarios de aplicaciones con marcos de JavaScript de desarrollo multiplataforma como Electron, React Native y así sucesivamente. ya que requieren más funcionalidades para la interacción con las plataformas nativas.

## <a name="specifics"></a>Características específicas

Para habilitar este escenario para la aplicación, son necesarios los siguientes aspectos:

* Registro de la aplicación con Azure AD implica habilitar el flujo implícito y establecer un URI de redireccionamiento a la que se devuelven los tokens.
* Configuración de la aplicación con las propiedades de la aplicación registrada como el identificador de aplicación.
* Mediante la biblioteca MSAL para realizar el flujo de autenticación para iniciar sesión y adquirir tokens.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-spa-app-registration.md)
