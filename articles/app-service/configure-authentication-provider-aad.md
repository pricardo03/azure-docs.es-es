---
title: 'Configuración de la autenticación de Azure Active Directory: Azure App Service'
description: Obtenga información acerca de cómo configurar la autenticación de Azure Active Directory para la aplicación de App Service.
author: cephalin
services: app-service
documentationcenter: ''
manager: gwallace
editor: ''
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service
ms.workload: web,mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/03/2019
ms.author: cephalin
ms.custom: fasttrack-edit
ms.openlocfilehash: 8de464a00867dd397f28de1dc35cf264244f6905
ms.sourcegitcommit: 86d49daccdab383331fc4072b2b761876b73510e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70743249"
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-sign-in"></a>Configuración de una aplicación de App Service para usar la información de inicio de sesión de Azure Active Directory

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

> [!NOTE]
> En este momento, AAD V2 (incluido MSAL) no se admite para Azure App Service y Azure Functions.
>

En este artículo se muestra cómo configurar Azure App Service para usar Azure Active Directory como proveedor de autenticación.

Se recomienda que configure cada aplicación de App Service con su propio registro, para que tenga sus propios permisos y consentimiento. Considere también la posibilidad de usar registros de aplicaciones para ranuras de implementación independientes. Esta opción evita el uso compartido de permisos entre entornos, de modo que un problema en el código nuevo que esté probando no afectará a la producción.

## <a name="express"> </a>Configuración rápida

1. En [Azure Portal], vaya a su aplicación de App Service. En el panel de navegación izquierdo, seleccione **Autenticación / Autorización**.
2. Si **Autenticación / Autorización** no está habilitado, seleccione **Activar**.
3. Seleccione **Azure Active Directory** y luego **Rápida**, en **Modo de administración**.
4. Seleccione **Aceptar** para registrar la aplicación de App Service en Azure Active Directory. Se crea un nuevo registro de aplicaciones. Si, por el contrario, desea elegir un registro de aplicación existente, haga clic en **Seleccionar una aplicación existente** y busque el nombre de un registro creado de aplicación anteriormente en el inquilino. Haga clic en el registro de aplicación para seleccionarlo y en **Aceptar**. A continuación, haga clic en **Aceptar** en la página de configuración de Azure Active Directory.
De forma predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio y a las API. Debe autorizar a los usuarios en el código de la aplicación.
5. (Opcional) Para restringir el acceso a la aplicación y que solo obtengan acceso los usuarios autenticados mediante Azure Active Directory, configure la **acción necesaria cuando la solicitud no está autenticada** en **Log in with Azure Active Directory** (Iniciar sesión con Azure Active Directory). Esto requiere que todas las solicitudes se autentiquen y que todas las solicitudes no autenticadas se redirijan a Azure Active Directory para la autenticación.

    > [!NOTE]
    > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no ser deseable para las aplicaciones que necesitan una página de inicio disponible públicamente, como muchas aplicaciones de una sola página. Para tales aplicaciones, puede ser preferible **permitir las solicitudes anónimas (sin acción)** y que la aplicación inicie manualmente el inicio de sesión, tal como se describe [aquí](overview-authentication-authorization.md#authentication-flow).
6. Haga clic en **Save**(Guardar).

## <a name="advanced"> </a>Configuración avanzada

También puede proporcionar opciones de configuración de forma manual, si el inquilino de Azure Active Directory que quiere usar es diferente del inquilino con el que inicia sesión en Azure. Para completar la configuración, primero debe crear un registro en Azure Active Directory y luego proporcionar algunos de los detalles de registro a App Service.

### <a name="register"> </a>Creación de un registro de aplicaciones en Azure AD para la aplicación App Service

Al crear manualmente un registro de aplicaciones, tenga en cuenta tres fragmentos de información que necesitará más adelante al configurar la aplicación App Service: el identificador de cliente, el identificador de inquilino y, opcionalmente, el secreto de cliente y el URI del identificador de aplicación.

1. En [Azure Portal], vaya a la aplicación App Service y anote la **dirección URL** de la aplicación. La usará para configurar el registro de la aplicación de Azure Active Directory.
1. En [Azure Portal], en el menú de la izquierda, seleccione **Active Directory** > **App registrations (Registros de aplicaciones)**  > **New registration** (Nuevo registro). 
1. En la página **Register an application** (Registrar una aplicación), escriba el **nombre** del registro de aplicaciones.
1. En **Redirect URI** (Redirección de URI), seleccione **Web** y escriba la dirección URL de la aplicación de App Service y agregue la ruta `/.auth/login/aad/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Seleccione **Crear**.
1. Una vez creado el registro de la aplicación, copie el **identificador de la aplicación (cliente)** y el **identificador de directorio (inquilino)** para usarlos más adelante.
1. Seleccione la **personalización de marca**. En la **dirección URL de la página principal**, escriba la dirección URL de la aplicación App Service y seleccione **Save** (Guardar).
1. Seleccione **Expose an API (Exponer una API)**  > **Set (Conjunto)** . Cópiela en la dirección URL de la aplicación de App Service y seleccione **Save** (Guardar).

    > [!NOTE]
    > Este valor es el **URI del identificador de la aplicación** del registro de aplicaciones. Si por ejemplo quiere tener una aplicación web de front-end para obtener acceso a una API de back-end y quiere que el back-end conceda explícitamente el acceso al front-end, necesitará el **URI del identificador de aplicación** del *front-end* cuando configure el recurso de aplicación de App Service del *back-end*.
1. Seleccione **Agregar un ámbito**. En el **nombre del ámbito**, escriba *user_impersonation*. En los cuadros de texto, escriba el nombre del ámbito de consentimiento y la descripción que quiere que vean los usuarios en la página de consentimiento, como *Access my app* (Acceder a mi aplicación). Cuando haya finalizado, haga clic en **Add scope** (Agregar ámbito).
1. (Opcional) Para crear un secreto de cliente, seleccione **Certificates & secrets (Certificados y secretos)**  > **New client secret (Nuevo secreto de cliente)**  > **Add (Agregar)** . Copie el valor del secreto del cliente que se muestra en la página. Una vez que salga de la misma, no se volverá a mostrar.
1. (Opcional) Para agregar varias **direcciones URL de respuesta**, seleccione la opción de **autenticación** en el menú.

### <a name="secrets"> </a>Incorporación de información de Azure Active Directory a la aplicación de App Service

1. En [Azure Portal], vaya a su aplicación de App Service. En el menú izquierdo, seleccione **Authentication / Authorization** (Autenticación / Autorización). Si la característica Authentication / Authorization (Autenticación/Autorización) no está habilitada, **actívela**. 
1. (Opcional) De forma predeterminada, la autenticación de App Service permite el acceso no autenticado a la aplicación. Para aplicar la autenticación de usuario, establezca la **acción necesaria cuando la solicitud no está autenticada**, en **Log in with Azure Active Directory** (Iniciar sesión con Azure Active Directory).
1. En la opción de proveedores de autenticación, seleccione **Azure Active Directory**.
1. En el **modo de administración**, seleccione **Advanced** (Avanzada) y configure la autenticación de App Service de acuerdo con la tabla siguiente:

    |Campo|DESCRIPCIÓN|
    |-|-|
    |Id. de cliente| Use el **identificador de la aplicación (cliente)** del registro de aplicaciones. |
    |Id. del emisor| Use `https://login.microsoftonline.com/<tenant-id>` y reemplace *\<tenant-id>* con el **identificador de directorio (inquilino)** del registro de aplicaciones. |
    |Secreto de cliente (opcional)| Use el secreto de cliente que generó en el registro de la aplicación.|
    |Audiencias de token permitidas| Si se trata de una aplicación de *back-end* y quiere permitir los tokens de autenticación desde una aplicación front-end, agregue aquí el **URI del identificador de aplicación** del *front-end*. |

    > [!NOTE]
    > De forma implícita, se considera que el identificador de **Id. de cliente** es *siempre* un público permitido, independientemente de cómo se haya configurado la opción **Audiencias de token permitidas**.
1. Seleccione **OK** (Aceptar) y después **Save** (Guardar).

Ahora está preparado para usar Azure Active Directory para realizar la autenticación en la aplicación de App Service.

## <a name="configure-a-native-client-application"></a>Configuración de una aplicación de cliente nativo
Puede registrar clientes nativos si quiere realizar inicios de sesión con una biblioteca de cliente como la **biblioteca de autenticación de Active Directory**.

1. En [Azure Portal], en el menú de la izquierda, seleccione **Active Directory** > **App registrations (Registros de aplicaciones)**  > **New registration** (Nuevo registro). 
1. En la página **Register an application** (Registrar una aplicación), escriba el **nombre** del registro de aplicaciones.
1. En **Redirect URI** (Redirección de URI), seleccione **Public client (mobile & desktop)** (Cliente público [dispositivos móviles y PC]) y escriba la dirección URL de la aplicación de App Service; a continuación, agregue la ruta `/.auth/login/aad/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Seleccione **Crear**.

    > [!NOTE]
    > Para una aplicación de Windows, use el valor de [SID del paquete](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) como URI en su lugar.
1. Una vez creado el registro de aplicaciones, copie el valor del **identificador de la aplicación (cliente)** .
1. En el menú de la izquierda, seleccione **API permissions (Permisos de API)**  > **Add a permission (Agregar un permiso)**  > **My APIs (Mis API)** .
1. Seleccione el registro de aplicaciones que creó anteriormente para la aplicación de App Service. Si no ve el registro de aplicaciones, compruebe que agregó el ámbito **user_impersonation** en la [creación de un registro de aplicaciones en Azure AD para la aplicación de App Service](#register).
1. Seleccione **user_impersonation** y haga clic en **Add permissions** (Agregar permisos).

Ahora ha configurado una aplicación cliente nativa que puede acceder a la aplicación de App Service.

## <a name="related-content"></a>Contenido relacionado

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/configure-authentication-provider-aad/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[alternative method]:#advanced
