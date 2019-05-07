---
title: API de web - registro de la aplicación protegida | Azure
description: Obtenga información sobre cómo crear una API Web protegida y la información que necesita registrar la aplicación.
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
ms.openlocfilehash: a1d8c8fcf84cd008957fcdb7cd14c4a07d9f3643
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074911"
---
# <a name="protected-web-api---app-registration"></a>API - registro de la aplicación web protegida

En este artículo se explica los detalles de registro de aplicación para una API web protegida.

Consulte [Quickstart: Registrar una aplicación con la plataforma Microsoft identity](quickstart-register-app.md) para conocer los pasos comunes sobre cómo registrar la aplicación.

## <a name="accepted-token-version"></a>Versión del token aceptado

El punto de conexión de plataforma de identidad de Microsoft puede emitir dos tipos de tokens: v1.0 tokens y los tokens de v2.0. Puede aprender más acerca de estos tokens en [tokens de acceso](access-tokens.md). Depende de la versión del token aceptada el **admite tipos de cuenta** eligió al crear la aplicación:

- Si el valor de **admite tipos de cuenta** es **cuentas en cualquier directorio de organización y cuentas personales de Microsoft (por ejemplo, Skype, Xbox, Outlook.com)**, la versión del token aceptada será v2.0.
- En caso contrario, la versión del token aceptada será v2.0.

Una vez que haya creado la aplicación, puede cambiar la versión del token aceptada siguiendo estos pasos:

1. En el portal de Azure, seleccione la aplicación y, a continuación, seleccione el **manifiesto** para la aplicación.
2. En el manifiesto, busque **"accessTokenAcceptedVersion"** y verá que su valor es **2**. Esta propiedad permite a Azure AD saber que la API web acepte tokens de v2.0. Si es **null**, la versión del token aceptada será v1.0.
3. Seleccione **Guardar**.

> [!NOTE]
> Depende de la API web para decidir qué versión de token (v1.0 o v2.0) acepta. Cuando los clientes solicitan un token para su API web mediante el punto de conexión de v2.0 de plataforma de Microsoft identity, obtendrá un token que indica qué versión está aceptada por la API web.

## <a name="no-redirect-uri"></a>Ningún URI de redireccionamiento

Las API Web no es necesario registrar un URI de redirección, como no es de ningún usuario ha iniciado sesión forma interactiva.

## <a name="expose-an-api"></a>Exponer una API

Otra configuración específica para las API web es la API expuesta y los ámbitos expuestos.

### <a name="resource-uri-and-scopes"></a>URI de recurso y ámbitos

Los ámbitos son normalmente del formulario `resourceURI/scopeName`. Para Microsoft Graph, los ámbitos tienen métodos abreviados como `User.Read`, pero esta cadena es simplemente un acceso directo para `https://graph.microsoft.com/user.read`.

Durante el registro de aplicación, necesita definir los parámetros siguientes:

- Un recurso de URI: de forma predeterminada, el portal de registro de aplicación recomienda usar `api://{clientId}`. Este URI de recurso es único, pero no es humano legible. Puede cambiarlo, pero asegúrese de que sea único.
- Uno o varios ámbitos

Los ámbitos también se muestran en la pantalla de consentimiento que se presenta a los usuarios finales que usen su aplicación. Por lo tanto, deberá proporcionar las cadenas correspondientes que describen el ámbito:

- Tal como lo ve el usuario final
- Tal como se muestra por el Administrador de inquilinos, que puede conceder consentimiento del administrador

### <a name="how-to-expose-the-api"></a>Cómo exponer la API

1. Seleccione el **exponer una API** sección en el registro de aplicación, y:
   1. Seleccione **Agregar un ámbito**.
   1. Acepte el URI de Id. de aplicación propuesto (api :// {clientId}) seleccionando **guardar y continuar**.
   1. Escriba los siguientes parámetros:
      - Para **nombre de ámbito**, utilice `access_as_user`.
      - Para **quién puede conceder**, asegúrese de que el **administradores y usuarios** está seleccionada.
      - En **nombre para mostrar del consentimiento del administrador**, tipo `Access TodoListService as a user`.
      - En **descripción del consentimiento del administrador**, tipo `Accesses the TodoListService Web API as a user`.
      - En **nombre para mostrar del consentimiento del usuario**, tipo `Access TodoListService as a user`.
      - En **descripción del consentimiento del usuario**, tipo `Accesses the TodoListService Web API as a user`.
      - Mantener **estado** establecido en **habilitado**.
      - Seleccione **Agregar ámbito**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la aplicación de código](scenario-protected-web-api-app-configuration.md)
