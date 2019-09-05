---
title: 'Tutorial: Registro de una aplicación en Azure Active Directory B2C'
description: Aprenda a registrar una aplicación web en Azure Active Directory B2C con Azure Portal.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 08/23/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 651c15c8206f7956bb35520f9c5837cb0c9308f9
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980703"
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
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y **Agregar**.
1. Escriba un nombre para la aplicación. Por ejemplo, *webapp1*.
1. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
1. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. Por ejemplo, se puede establecer para la escucha local en `https://localhost:44316`. Si aún no conoce el número de puerto, puede especificar un valor de marcador de posición y cambiarlo más adelante.

    Con fines de prueba (como los de este tutorial) puede establecerlo en `https://jwt.ms`, que muestra el contenido de un token para su inspección. Para este tutorial, establezca la **URL de respuesta** en `https://jwt.ms`.

    Las siguientes restricciones se aplican a las direcciones URL de respuesta:

    * La dirección URL de respuesta debe comenzar con el esquema `https`.
    * La dirección URL de respuesta distingue mayúsculas de minúsculas. Sus mayúsculas o minúsculas deben coincidir con las de la ruta de acceso de la dirección URL de la aplicación en ejecución. Por ejemplo, si la aplicación incluye como parte de su ruta de acceso `.../abc/response-oidc`, no especifique `.../ABC/response-oidc` en la dirección URL de respuesta. Dado que el explorador web tiene en cuenta las mayúsculas y minúsculas de la ruta de acceso, se pueden excluir las cookies asociadas con `.../abc/response-oidc` si se redirigen a la dirección URL `.../ABC/response-oidc` con mayúsculas y minúsculas no coincidentes.

1. Haga clic en **Create** (Crear) para completar el registro de la aplicación.

## <a name="create-a-client-secret"></a>Creación de un secreto de cliente

Si la aplicación intercambia un código para un token, deberá crear un secreto de aplicación.

1. En la página de **aplicaciones de Azure AD B2C**, seleccione la aplicación que ha creado, por ejemplo, *webapp1*.
1. Seleccione **Claves** y haga clic en **Generar clave**.
1. Seleccione **Guardar** para ver la clave. Anote el valor de la **Clave de la aplicación**. Use este valor como secreto de aplicación en el código de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Registro de una aplicación web
> * Creación de un secreto de cliente

A continuación, aprenda a crear flujos de usuario para permitir que los usuarios se registren, inicien sesión y administren sus perfiles.

> [!div class="nextstepaction"]
> [Creación de flujos de usuario en Azure Active Directory B2C >](tutorial-create-user-flows.md)
