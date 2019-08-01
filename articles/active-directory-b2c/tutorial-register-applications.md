---
title: 'Tutorial: Registro de una aplicación en Azure Active Directory B2C'
description: Aprenda a registrar una aplicación web en Azure Active Directory B2C con Azure Portal.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5c46d3153bdc5768836bce198af115f82e8469f3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056285"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>Tutorial: Registro de una aplicación en Azure Active Directory B2C

Para que sus [aplicaciones](active-directory-b2c-apps.md) puedan interactuar con Azure Active Directory (Azure AD) B2C, deben estar registradas en un inquilino que administre. En este tutorial se muestra cómo registrar una aplicación web mediante Azure Portal.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Registro de una aplicación web
> * Creación de un secreto de cliente

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Si aún no ha creado su propio [inquilino de Azure AD B2C](tutorial-create-tenant.md), cree una ahora. Puede usar un inquilino de Azure AD B2C existente.

## <a name="register-a-web-application"></a>Registro de una aplicación web

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione **Aplicaciones** y **Agregar**.
4. Escriba un nombre para la aplicación. Por ejemplo, *webapp1*.
5. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
6. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. Por ejemplo, se puede establecer para la escucha local en `https://localhost:44316`. Si aún no conoce el número de puerto, puede especificar un valor de marcador de posición y cambiarlo más adelante.

    Con fines de prueba (como los de este tutorial) puede establecerlo en `https://jwt.ms`, que muestra el contenido de un token para su inspección. Para este tutorial, establezca la **URL de respuesta** en `https://jwt.ms`.

    La dirección URL de respuesta debe comenzar por el esquema `https`, y todos los valores de dicha dirección deben compartir un único dominio DNS. Por ejemplo, si la aplicación tiene una dirección URL de respuesta de `https://login.contoso.com`, puede agregar a ella como esta dirección URL `https://login.contoso.com/new`. O bien, puede hacer referencia a un subdominio DNS de `login.contoso.com`, tal como `https://new.login.contoso.com`. Si quiere una aplicación con `login-east.contoso.com` y `login-west.contoso.com` como URL de respuesta, debe agregar las URL de respuesta en este orden: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. Puede agregar las dos últimas porque son subdominios de la primera URL de respuesta, `contoso.com`.

7. Haga clic en **Create**(Crear).

## <a name="create-a-client-secret"></a>Creación de un secreto de cliente

Si la aplicación intercambia un código para un token, deberá crear un secreto de aplicación.

1. En la página de **aplicaciones de Azure AD B2C**, seleccione la aplicación que ha creado, por ejemplo, *webapp1*.
2. Seleccione **Claves** y haga clic en **Generar clave**.
3. Seleccione **Guardar** para ver la clave. Anote el valor de la **Clave de la aplicación**. Use este valor como secreto de aplicación en el código de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Registro de una aplicación web
> * Creación de un secreto de cliente

A continuación, aprenda a crear flujos de usuario para permitir que los usuarios se registren, inicien sesión y administren sus perfiles.

> [!div class="nextstepaction"]
> [Creación de flujos de usuario en Azure Active Directory B2C >](tutorial-create-user-flows.md)
