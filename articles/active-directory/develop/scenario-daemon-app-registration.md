---
title: 'Daemon app que realiza la llamada API web (registro de aplicación): la plataforma de identidad de Microsoft'
description: 'Obtenga información sobre cómo compilar una aplicación demonio que llama a web API: registro de la aplicación'
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
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79a355ab226e56a3dde1df5369deda5142d47848
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076246"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Aplicación de demonio que llama a web API: registro de la aplicación

Para una aplicación demonio, aquí es lo que necesita saber al registrar la aplicación.

## <a name="supported-account-types"></a>Tipos de cuenta admitidos

Dado que las aplicaciones de demonio solo tienen sentido en el inquilino de Azure AD, al crear la aplicación necesita elegir:

- cualquier **cuentas en este directorio organizativa solo**. Esta opción es el caso más común, como las aplicaciones de demonio normalmente se escriben los desarrolladores de línea de negocio (LOB).
- o **cuentas en el directorio de cualquier organización**. Realizará esta opción si es un ISV que QUIERE proporcionar una herramienta de utilidad a sus clientes. Necesitará los administradores de inquilinos del cliente para aprobarlo.

## <a name="authentication---no-reply-uri-needed"></a>Es necesario ningún URI de respuesta de autenticación:

En el caso de que se usa la aplicación cliente confidencial **sólo** fluyen de las credenciales del cliente, no es necesario registrar la dirección URL de respuesta. No se necesita para la configuración de aplicación/construcción. El flujo de credenciales de cliente no usarlo.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>Permisos de API - permisos de aplicación y consentimiento del administrador

Una aplicación de demonio solo puede solicitar permisos de aplicación a las API (sin delegar permisos). En el **permiso API** página para el registro de aplicación, después de seleccionar **agregar un permiso** y elige la familia de API, elija **permisos de la aplicación**, y, a continuación, seleccione los permisos

![Permisos de aplicación y consentimiento del administrador](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

Aplicaciones demonio requieren tener un administrador de inquilinos previamente dar su consentimiento a la aplicación que llama a la API web. Este consentimiento se proporciona en el mismo **permiso API** página seleccionando un inquilino admin **conceder consentimiento a *nuestra organización***

Si es un ISV que QUIERE compilar una aplicación de varios inquilinos, ¿desea comprobar la [implementación - caso de aplicaciones de demonio de varios inquilinos](scenario-daemon-production.md#deployment---case-of-multi-tenant-daemon-apps) párrafo.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: configuración del código de aplicación](./scenario-daemon-app-configuration.md)
