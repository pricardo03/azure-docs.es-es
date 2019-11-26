---
title: 'Aplicación de demonio que llama a las API web (registro de la aplicación): Plataforma de identidad de Microsoft'
description: 'Obtenga información sobre cómo compilar una aplicación de demonio que llame a las API web: registro de la aplicación'
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8d0d88f72cca45280bd76ac7bb9d7a6e0a1d37fb
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175450"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Aplicación de demonio que llama a las API web: registro de la aplicación

Para una aplicación de demonio, esto es lo que tiene que saber al registrar la aplicación.

## <a name="supported-account-types"></a>Tipos de cuenta admitidos

Dado que las aplicaciones de demonio solo tienen sentido en el inquilino de Azure AD, al crear la aplicación tendrá que elegir:

- O bien **Solo las cuentas de este directorio organizativo**. Esta opción es el caso más común, ya que normalmente son desarrolladores de línea de negocio (LOB) quienes escriben aplicaciones de demonio.
- O bien **Cuentas en cualquier directorio organizativo**. Elegirá esta opción si es un ISV que proporciona una herramienta de utilidad a sus clientes. Necesitará a los administradores de inquilinos del cliente para aprobarla.

## <a name="authentication---no-reply-uri-needed"></a>Autenticación: sin necesidad de URI de respuesta

En caso de que la aplicación cliente confidencial use **solo** el flujo de credenciales del cliente, no es necesario registrar el URI de respuesta. No se necesita para la configuración ni construcción de la aplicación. El flujo de credenciales de cliente no la usa.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>Permisos de API: Permisos de aplicación y consentimiento del administrador

Una aplicación de demonio solo puede solicitar permisos de aplicación a las API (no permisos delegados). En la página **API Permission** (Permiso de API) para el registro de la aplicación, después de haber seleccionado **Add a permission** (Agregar un permiso) y elegido la familia de API, elija **Permisos de la aplicación** y, luego, seleccione los permisos.

![Permisos de aplicación y consentimiento del administrador](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

> [!NOTE]
> La API web a la que desea llamar debe definir los **permisos de aplicación (roles de aplicación)** , no los permisos delegados. Para más información sobre cómo exponer una API de este tipo, consulte [API web protegida: Registro de aplicación. Si una aplicación de demonio llama a la API web](scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app).

Las aplicaciones de demonio requieren tener un consentimiento previo del administrador de inquilinos para la aplicación que llama a la API web. Este consentimiento se proporciona en la misma página **API Permission**, cuando un administrador de inquilinos selecciona **Grant admin consent to *nuestra organización*** (Conceder consentimiento del administrador a nuestra organización).

Si es un ISV que compila una aplicación de varios inquilinos, querrá revisar el párrafo [Implementación: caso de aplicaciones de demonio de varios inquilinos](scenario-daemon-production.md#deployment---case-of-multi-tenant-daemon-apps).

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: configuración del código de aplicación](./scenario-daemon-app-configuration.md)
