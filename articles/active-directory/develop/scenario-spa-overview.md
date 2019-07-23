---
title: 'Introducción al escenario de aplicación de página única de JavaScript: Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación de página única (introducción al escenario) que integra la Plataforma de identidad de Microsoft.
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65076366"
---
# <a name="scenario-single-page-application"></a>Escenario: Aplicación de una sola página

Aprenda lo necesario para crear una aplicación de página única (SPA)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Puede crear su primera aplicación siguiendo la guía de inicio rápido de SPA de JavaScript:

> [!div class="nextstepaction"]
> [Inicio rápido: Aplicación de página única](./quickstart-v2-javascript.md)

## <a name="overview"></a>Información general

Muchas aplicaciones web modernas se crean como aplicaciones cliente de una sola página, escritas con JavaScript o un marco de SPA, como Angular, Vue.js y React.js. Estas aplicaciones se ejecutan en un explorador web y tienen características de autenticación diferentes que las aplicaciones web de servidor tradicionales. La Plataforma de identidad de Microsoft permite que las aplicaciones de página única inicien la sesión de los usuarios y obtengan tokens para acceder a los servicios de back-end o a las API web mediante el [flujo implícito de OAuth 2.0](./v2-oauth2-implicit-grant-flow.md). El flujo implícito permite a la aplicación obtener los tokens de identificador para representar al usuario autenticado y también acceder a los tokens que se necesitan para llamar a las API protegidas.

![Aplicación de página única](./media/scenarios/spa-app.svg)

Este flujo de autenticación no incluye escenarios de aplicaciones con marcos de JavaScript de desarrollo multiplataforma, como Electron, React Native, etc., ya que requieren más funcionalidades para la interacción con las plataformas nativas.

## <a name="specifics"></a>Características específicas

Para habilitar este escenario para la aplicación, son necesarios los siguientes aspectos:

* El registro de la aplicación en Azure AD implica habilitar el flujo implícito y establecer un URI de redireccionamiento al que se devuelven los tokens.
* La configuración de la aplicación con las propiedades de la aplicación registrada, como el identificador de aplicación.
* El uso de la biblioteca MSAL para seguir el flujo de autenticación para iniciar sesión y adquirir tokens.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-spa-app-registration.md)
