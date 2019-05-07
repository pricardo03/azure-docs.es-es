---
title: 'Aplicación de página única (registro de aplicación): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación de página única (registro de aplicación)
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
ms.openlocfilehash: b1faf4669dca2aaaf3f873e66f859473ccd99f10
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074836"
---
# <a name="single-page-application---app-registration"></a>Aplicación de página única - registro de aplicación

Esta página explican los detalles de registro de aplicación para una aplicación de página única (SPA).

Siga los pasos para [registrar una nueva aplicación con la plataforma Microsoft identity](quickstart-register-app.md)y seleccione las cuentas admitidas para la aplicación. El escenario SPA puede admitir la autenticación con cuentas en su organización o cualquier organización y las cuentas personales de Microsoft.

A continuación, obtenga información sobre los aspectos específicos del registro de aplicación que se aplican a las aplicaciones de página única.

## <a name="register-a-redirect-uri"></a>Registrar un URI de redirección

El flujo implícito envía los tokens en una redirección a la aplicación de una página que se ejecuta en un explorador web. Por lo tanto, es un requisito importante para registrar una redirección URI donde la aplicación puede recibir los tokens. Asegúrese de que la redirección que URI coincide exactamente con el URI de la aplicación.

En el [portal Azure](https://go.microsoft.com/fwlink/?linkid=2083908), vaya a la aplicación registrada, en el **autenticación** página de la aplicación, seleccione la **Web** plataforma y escriba el valor de la URI de redirección de la aplicación en el **URI de redireccionamiento** campo.

## <a name="enable-the-implicit-flow"></a>Habilitar el flujo implícito

En el mismo **autenticación** página, en **configuración avanzada**, también debe habilitar la **concesión implícita**. Si la aplicación solo está realizando el inicio de sesión de usuarios y obtener tokens de identificador, es suficiente para permitir **los tokens de identificador** casilla de verificación.

Si la aplicación también necesita acceso a los tokens para llamar a API, asegúrese de habilitar la **tokens de acceso** casilla de verificación. Para obtener más información, consulte [los tokens de identificador](./id-tokens.md) y [tokens de acceso](./access-tokens.md).

## <a name="api-permissions"></a>Permisos de API

Aplicaciones de una página pueden llamar a las API en nombre del usuario con sesión iniciada. Deben solicitar permisos delegados. Para obtener más información, consulte [agregar permisos para tener acceso a las API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la aplicación de código](scenario-spa-app-configuration.md)
