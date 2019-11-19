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
ms.openlocfilehash: 6812f99d8433ef318eca37eb2615d43f4749e944
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73886188"
---
# <a name="configure-your-app-service-app-to-use-azure-ad-login"></a>Configuración de la aplicación de App Service para usar el inicio de sesión de Azure AD

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este artículo se muestra cómo configurar Azure App Service para usar Azure Active Directory (Azure AD) como proveedor de autenticación.

> [!NOTE]
> En este momento, Azure App Service y Azure Functions solo se admiten en Azure AD v1.0. No son compatibles con la [Plataforma de identidad de Microsoft v2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-overview), que incluye bibliotecas de autenticación de Microsoft (MSAL).

Siga estos procedimientos recomendados para configurar la aplicación y la autenticación:

- Asigne a cada aplicación de App Service sus propios permisos y consentimiento.
- Configure cada aplicación de App Service con su propio registro.
- Evite el uso compartido de permisos entre entornos mediante registros de aplicación independientes para ranuras de implementación independientes. Al probar nuevo código, esta práctica puede ayudar a evitar que los problemas afecten a la aplicación de producción.

## <a name="express"> </a>Configuración rápida

1. En [Azure Portal], vaya a la aplicación de App Service.
1. Seleccione **Configuración** > **Autenticación/autorización** en el panel izquierdo y asegúrese de que la opción **Autenticación de App Service** está **Activada**.
1. Seleccione **Azure Active Directory** y luego **Rápida**, en **Modo de administración**.
1. Seleccione **Aceptar** para registrar la aplicación de App Service en Azure Active Directory. Se crea un nuevo registro de la aplicación.

   Si prefiere elegir un registro de aplicación existente:

   1. Elija **Seleccionar una aplicación existente** y busque el nombre de un registro de aplicación creado anteriormente en el inquilino.
   1. Seleccione el registro de aplicación y después **Aceptar**.
   1. A continuación, seleccione **Aceptar** en la página de configuración de Azure Active Directory.

   De forma predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio ni a las API. Debe autorizar a los usuarios en el código de la aplicación.
1. (Opcional) Para restringir el acceso a la aplicación solo a los usuarios autenticados mediante Azure Active Directory, configure **Acción necesaria cuando la solicitud no está autenticada** en **Iniciar sesión con Azure Active Directory**. Al establecer esta funcionalidad, la aplicación requiere que se autentiquen todas las solicitudes. También redirige todo lo que no está autenticado a Azure Active Directory para la autenticación.

    > [!CAUTION]
    > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no interesar en las aplicaciones que tienen una página principal disponible públicamente así como en muchas aplicaciones de página única. Para tales aplicaciones, puede ser preferible **Permitir solicitudes anónimas (ninguna acción)** y que la aplicación inicie manualmente el inicio de sesión. Para más información, consulte [Flujo de autenticación](overview-authentication-authorization.md#authentication-flow).
1. Seleccione **Guardar**.

## <a name="advanced"> </a>Configuración avanzada

Si quiere usar un inquilino de Azure AD diferente del que usa para iniciar sesión en Azure, puede configurar las opciones de la aplicación manualmente. Para completar esta configuración personalizada, necesitará realizar lo siguiente:

1. Crear un registro en Azure AD.
1. Proporcionar algunos detalles de registro a App Service.

### <a name="register"> </a>Creación de un registro de aplicaciones en Azure AD para la aplicación App Service

Para configurar la aplicación de App Service, necesitará la siguiente información:

- Id. de cliente
- Id. de inquilino
- Secreto de cliente (opcional)
- URI de Id. de aplicación

Lleve a cabo los siguiente pasos:

1. Inicie sesión en [Azure Portal] y vaya a la aplicación de App Service. Anote la **Dirección URL** de la aplicación. La usará para configurar el registro de la aplicación de Azure Active Directory.
1. Haga clic en **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.
1. En la página **Register an application** (Registrar una aplicación), escriba el **nombre** del registro de aplicaciones.
1. En **URI de redirección**, seleccione **Web** y escriba la dirección URL de la aplicación de App Service, anexando la ruta de acceso `/.auth/login/aad/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. 
1. Seleccione **Crear**.
1. Una vez creado el registro de la aplicación, copie el **Id. de aplicación (cliente)** y el **Id. de directorio (inquilino)** para usarlos más adelante.
1. Seleccione la **personalización de marca**. En **URL de página principal**, escriba la dirección URL de la aplicación de App Service y seleccione **Guardar**.
1. Seleccione **Expose an API (Exponer una API)**  > **Set (Conjunto)** . Cópiela en la dirección URL de la aplicación de App Service y seleccione **Save** (Guardar).

   > [!NOTE]
   > Este valor es el **URI del identificador de la aplicación** del registro de aplicaciones. Si la aplicación web requiere acceso a una API en la nube, al configurar el recurso de App Service en la nube necesitará el valor de **URI de Id. de aplicación** de la aplicación web. Puede utilizarlo, por ejemplo, si desea que el servicio en la nube conceda acceso explícitamente a la aplicación web.

1. Seleccione **Agregar un ámbito**.
   1. En **Nombre de ámbito**, escriba *user_impersonation*.
   1. En los cuadros de texto, escriba el nombre y la descripción del ámbito de consentimiento que quiere que vean los usuarios en la página de consentimiento. Por ejemplo, escriba *Access my app* (Acceder a mi aplicación). 
   1. Seleccione la opción **Agregar un ámbito**.
1. (Opcional) Para crear un secreto de cliente, seleccione **Certificates & secrets (Certificados y secretos)**  > **New client secret (Nuevo secreto de cliente)**  > **Add (Agregar)** . Copie el valor del secreto del cliente que se muestra en la página. No se volverá a mostrar.
1. (Opcional) Para agregar varios valores en **Direcciones URL de respuesta**, seleccione **Autenticación**.

### <a name="secrets"> </a>Incorporación de información de Azure Active Directory a la aplicación de App Service

1. En [Azure Portal], vaya a la aplicación de App Service. 
1. Seleccione **Configuración > Autenticación/autorización** en el panel izquierdo y asegúrese de que la opción **Autenticación de App Service** está **Activada**.
1. (Opcional) De forma predeterminada, la autenticación de App Service permite el acceso no autenticado a la aplicación. Para aplicar la autenticación de usuario, establezca la **acción necesaria cuando la solicitud no está autenticada**, en **Log in with Azure Active Directory** (Iniciar sesión con Azure Active Directory).
1. En la opción de proveedores de autenticación, seleccione **Azure Active Directory**.
1. En el **modo de administración**, seleccione **Advanced** (Avanzada) y configure la autenticación de App Service de acuerdo con la tabla siguiente:

    |Campo|DESCRIPCIÓN|
    |-|-|
    |Id. de cliente| Use el **identificador de la aplicación (cliente)** del registro de aplicaciones. |
    |Id. del emisor| Use `https://login.microsoftonline.com/<tenant-id>` y reemplace *\<tenant-id>* con el **identificador de directorio (inquilino)** del registro de aplicaciones. |
    |Secreto de cliente (opcional)| Use el secreto de cliente que generó en el registro de la aplicación.|
    |Audiencias de token permitidas| Si se trata de una aplicación en la nube o una aplicación de servidor y quiere permitir tokens de autenticación desde una aplicación web, agregue aquí el valor de **URI de Id. de aplicación** de la aplicación web. |

    > [!NOTE]
    > De forma implícita, se considera que el identificador de **Id. de cliente** es *siempre* un público permitido, independientemente de cómo se haya configurado la opción **Audiencias de token permitidas**.
1. Seleccione **Aceptar** y después **Guardar**.

Ahora está preparado para usar Azure Active Directory para realizar la autenticación en la aplicación de App Service.

## <a name="configure-a-native-client-application"></a>Configuración de una aplicación de cliente nativo

Puede registrar clientes nativos para permitir la autenticación con una biblioteca cliente como la **Biblioteca de autenticación de Active Directory**.

1. En [Azure Portal], seleccione **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.
1. En la página **Register an application** (Registrar una aplicación), escriba el **nombre** del registro de aplicaciones.
1. En **URI de redirección**, seleccione **Cliente público (móvil y escritorio)** y escriba la dirección URL de la aplicación de App Service, anexando la ruta de acceso `/.auth/login/aad/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.
1. Seleccione **Crear**.

    > [!NOTE]
    > Para una aplicación de Windows, use el valor de [SID del paquete](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) como URI en su lugar.
1. Una vez creado el registro de aplicación, copie el valor de **Id. de aplicación (cliente)** .
1. Seleccione **Permisos de API** > **Agregar permiso** > **Mis API**.
1. Seleccione el registro de aplicaciones que creó anteriormente para la aplicación de App Service. Si no ve el registro de aplicación, compruebe que agregó el ámbito **user_impersonation** en [Creación de un registro de aplicaciones en Azure AD para la aplicación App Service](#register).
1. Seleccione **user_impersonation** y después **Agregar permisos**.

Ahora ha configurado una aplicación cliente nativa que puede acceder a la aplicación de App Service.

## <a name="related-content"> </a>Pasos siguientes

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
