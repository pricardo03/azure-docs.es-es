---
title: Registro de una aplicación en Azure Active Directory B2C | Microsoft Docs
description: Aprenda a registrar su aplicación con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fcebada4ef10c3e0bb298e9308d66ecb37247832
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999242"
---
# <a name="register-an-application-in-azure-active-directory-b2c"></a>Registro de una aplicación en Azure Active Directory B2C

Para crear una [aplicación](active-directory-b2c-apps.md) que acepte registros e inicios de sesión de consumidores, primero deberá registrarla en un inquilino de Azure AD B2C. Este artículo le ayuda a registrar una aplicación en un inquilino de Azure Active Directory (Azure AD) B2C en unos minutos. Cuando haya terminado, la aplicación se habrá registrado para usarse en el inquilino de Azure AD B2C.

## <a name="prerequisites"></a>Requisitos previos

Para obtener su propio inquilino, siga los pasos descritos en [Creación de un inquilino de Azure AD B2C](tutorial-create-tenant.md).

Elija los pasos siguientes en función de su tipo de aplicación:

- [Registro de una aplicación web](#register-a-web-application)
- [Registro de una API web](#register-a-web-api)
- [Registro de una aplicación móvil o nativa](#register-a-mobile-or-native-application)

## <a name="register-a-web-application"></a>Registro de una aplicación web

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione **Aplicaciones** y **Agregar**.
4. Escriba un nombre para la aplicación. Por ejemplo *testapp1*.
5. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
6. En **Dirección URL de respuesta**, escriba el punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. Por ejemplo, se puede establecer para la escucha local en `https://localhost:44316`. Si aún no conoce el número de puerto, puede especificar un valor de marcador de posición y cambiarlo más adelante.
7. Haga clic en **Create**(Crear).

### <a name="create-a-client-secret"></a>Creación de un secreto de cliente

Si la aplicación llama a una API web protegida por Azure AD B2C, deberá crear un secreto de aplicación.

1. Seleccione **Claves** y, luego, haga clic en **Generar clave**. 
2. Seleccione **Guardar** para ver la clave. Anote el valor de la **Clave de la aplicación**. Use el valor como secreto de aplicación en el código de la aplicación.
3. Haga clic en **Acceso de API**, en **Agregar** y seleccione la API web y los ámbitos (permisos).

## <a name="register-a-web-api"></a>Registro de una API web

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione **Aplicaciones** y **Agregar**.
4. Escriba un nombre para la aplicación. Por ejemplo *testapp2*.
5. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
6. En **Dirección URL de respuesta**, escriba el punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. Por ejemplo, se puede establecer para la escucha local en `https://localhost:44316`. Si aún no conoce el número de puerto, puede especificar un valor de marcador de posición y cambiarlo más adelante.
7. En **URI de id. de aplicación**, escriba el identificador usado para la API web. Se genera el identificador URI completo, incluido el dominio. Por ejemplo, `https://contosotenant.onmicrosoft.com/api`.
8. Haga clic en **Create**(Crear).
9. Seleccione **Ámbitos publicados** para agregar más ámbitos según sea necesario. De forma predeterminada, se define el ámbito `user_impersonation`. El ámbito `user_impersonation` proporciona a otras aplicaciones la posibilidad de acceder a esta API en nombre del usuario que ha iniciado la sesión. Si lo desea, se puede quitar el ámbito `user_impersonation`.

## <a name="register-a-mobile-or-native-application"></a>Registro de una aplicación móvil o nativa

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione **Aplicaciones** y **Agregar**.
4. Escriba un nombre para la aplicación. Por ejemplo *testapp3*.
5. En **Incluir aplicación web o API web**, seleccione **No**.
6. En **Incluir cliente nativo**, seleccione **Sí**.
7. En **URI de redirección**, escriba un [URI de redirección con un esquema personalizado](active-directory-b2c-apps.md). Asegúrese de elegir un buen URI de redirección y no incluya caracteres especiales, como caracteres de subrayado.
8. Haga clic en **Create**(Crear).

### <a name="create-a-client-secret"></a>Creación de un secreto de cliente

Si la aplicación llama a una API web protegida por Azure AD B2C, deberá crear un secreto de aplicación.

1. Seleccione **Claves** y, luego, haga clic en **Generar clave**. 
2. Seleccione **Guardar** para ver la clave. Anote el valor de la **Clave de la aplicación**. Use el valor como secreto de aplicación en el código de la aplicación.
3. Haga clic en **Acceso de API**, en **Agregar** y seleccione la API web y los ámbitos (permisos).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las aplicaciones usan tokens de acceso para conceder permisos a las API en [Azure AD B2C: solicitud de tokens de acceso](active-directory-b2c-access-tokens.md)
