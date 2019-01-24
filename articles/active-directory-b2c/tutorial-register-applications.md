---
title: 'Tutorial: Registro de aplicaciones en Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información acerca de cómo registrar aplicaciones en Azure Active Directory B2C con Azure Portal.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 01/11/2019
ms.author: davidmu
ms.openlocfilehash: 99ad1bbaa732b1207ead9da8da36f345d4978241
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856033"
---
# <a name="tutorial-register-your-applications-in-azure-active-directory-b2c"></a>Tutorial: Registro de aplicaciones en Azure Active Directory B2C

Para que sus [aplicaciones](active-directory-b2c-apps.md) puedan interactuar con Azure Active Directory (Azure AD) B2C, deben estar registradas en un inquilino que administre. En este tutorial se muestra cómo registrar las aplicaciones que usan Azure Portal.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Registro de una aplicación web
> * Registro de una API web
> * Registro de una aplicación móvil o nativa

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Si aún no ha creado su propio [inquilino de Azure AD B2C](tutorial-create-tenant.md), cree una ahora. Puede usar un inquilino existente.

## <a name="register-a-web-application"></a>Registro de una aplicación web

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.

    ![Cambiar al directorio de suscripción](./media/tutorial-register-applications/switch-directories.png)

2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione **Aplicaciones** y **Agregar**.

    ![Agregar aplicación](./media/tutorial-register-applications/add-application.png)

4. Escriba un nombre para la aplicación. Por ejemplo, *webapp1*.
5. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
6. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. Por ejemplo, puede establecerlo para que escuche localmente en `https://localhost:44316` Si aún no conoce el número de puerto, puede especificar un valor de marcador de posición y cambiarlo más adelante. Con fines de prueba podría establecerlo en `https://jwt.ms`, que muestra el contenido de un token para su inspección. Para este tutorial, lo estableceremos en `https://jwt.ms`. 

    La dirección URL de respuesta debe comenzar por el esquema `https`, y todos los valores de dicha dirección deben compartir un único dominio DNS. Por ejemplo, si la aplicación tiene una dirección URL de respuesta de `https://login.contoso.com`, puede agregar a ella como esta dirección URL `https://login.contoso.com/new`. O bien, puede hacer referencia a un subdominio DNS de `login.contoso.com`, tal como `https://new.login.contoso.com`. Si quiere una aplicación con `login-east.contoso.com` y `login-west.contoso.com` como URL de respuesta, debe agregar las URL de respuesta en este orden: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. Puede agregar las dos últimas porque son subdominios de la primera URL de respuesta, `contoso.com`.

7. Haga clic en **Create**(Crear).

    ![Configuración de las propiedades de la aplicación](./media/tutorial-register-applications/application-properties.png)

### <a name="create-a-client-secret"></a>Creación de un secreto de cliente

Si la aplicación intercambia un código para un token, deberá crear un secreto de aplicación.

1. Seleccione **Claves** y, luego, haga clic en **Generar clave**.

    ![Generar claves](./media/tutorial-register-applications/generate-keys.png)

2. Seleccione **Guardar** para ver la clave. Anote el valor de la **Clave de la aplicación**. Use el valor como secreto de aplicación en el código de la aplicación.

    ![Guardar la clave](./media/tutorial-register-applications/save-key.png)
    
3. Haga clic en **Acceso de API**, en **Agregar** y seleccione la API web y los ámbitos (permisos).

    ![Configurar acceso de API](./media/tutorial-register-applications/api-access.png)

## <a name="register-a-web-api"></a>Registro de una API web

1. Seleccione **Aplicaciones** y **Agregar**.
3. Escriba un nombre para la aplicación. Por ejemplo, *webapi1*.
4. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
5. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. Por ejemplo, se puede establecer para la escucha local en `https://localhost:44316`. Si aún no conoce el número de puerto, puede especificar un valor de marcador de posición y cambiarlo más adelante.
6. En **URI de id. de aplicación**, escriba el identificador usado para la API web. Se genera el identificador URI completo, incluido el dominio. Por ejemplo, `https://contosotenant.onmicrosoft.com/api`.
7. Haga clic en **Create**(Crear).
8. Seleccione la aplicación *webapi1* que creó y, después, seleccione **Ámbitos publicados** para agregar más ámbitos según sea necesario. De forma predeterminada, se define el ámbito `user_impersonation`. El ámbito `user_impersonation` proporciona a otras aplicaciones la posibilidad de acceder a esta API en nombre del usuario que ha iniciado la sesión. Si lo desea, se puede quitar el ámbito `user_impersonation`.

    ![Establecer ámbitos publicados](./media/tutorial-register-applications/published-scopes.png)


## <a name="register-a-mobile-or-native-application"></a>Registro de una aplicación móvil o nativa

1. Seleccione **Aplicaciones** y **Agregar**.
2. Escriba un nombre para la aplicación. Por ejemplo, *nativeapp1*.
3. En **Incluir aplicación web o API web**, seleccione **No**.
4. En **Incluir cliente nativo**, seleccione **Sí**.
5. En **URI de redirección**, escriba un URI de redirección válido con un esquema personalizado. Hay dos aspectos importantes que se deben tener en cuenta al elegir un URI de redirección:

    - **Único**: el esquema del identificador URI de redirección debe ser único para todas las aplicaciones. En el ejemplo `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` es el esquema. Se debe seguir este patrón. Si dos aplicaciones comparten el mismo esquema, el usuario tiene opción de elegir una aplicación. Si el usuario realiza una elección incorrecta, no será posible iniciar sesión.
    - **Completo**: el URI de redirección debe tener un esquema y una ruta de acceso. La ruta de acceso debe contener al menos una barra diagonal después del dominio. Por ejemplo, `//contoso/` sirve y `//contoso` produce un error. Asegúrese de que el URI de redirección no incluya caracteres especiales, como caracteres de subrayado.

6. Haga clic en **Create**(Crear).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Registro de una aplicación web
> * Registro de una API web
> * Registro de una aplicación móvil o nativa

> [!div class="nextstepaction"]
> [Creación de flujos de usuario en Azure Active Directory B2C](tutorial-create-user-flows.md)