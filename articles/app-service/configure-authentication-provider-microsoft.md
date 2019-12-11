---
title: Configuración de la autenticación de Microsoft
description: Aprenda a configurar la autenticación de la cuenta Microsoft como proveedor de identidades para una aplicación App Service.
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.topic: article
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: f9158a4094b7d2ec148c2cae85decb3ad959b7c3
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671918"
---
# <a name="configure-your-app-service-app-to-use-microsoft-account-login"></a>Configuración de la aplicación App Service para usar el inicio de sesión de la cuenta de Microsoft

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este tema se muestra cómo configurar Azure App Service para usar la cuenta Microsoft como proveedor de autenticación. 

## <a name="register-microsoft-account"></a>Registro de la aplicación con la cuenta de Microsoft

1. Vaya a [**Registros de aplicaciones**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) en Azure Portal. Si es necesario, inicie sesión con la cuenta de Microsoft.
1. Seleccione **Nuevo registro** y escriba el nombre de la aplicación.
1. En **URI de redirección**, seleccione **Web** y escriba `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application`. Reemplace *\<app-domain-name>* con el nombre de dominio de su aplicación.  Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`. Asegúrese de usar el esquema HTTPS en la dirección URL.

1. Seleccione **Registrar**.
1. Copie el **id. de la aplicación (cliente)** . Lo necesitará más adelante.
1. En el panel izquierdo, seleccione **Certificados y secretos** > **Nuevo secreto de cliente**. Escriba una descripción, seleccione la duración de validez y después seleccione **Agregar**.
1. Copie el valor que aparece en la página **Certificados y secretos**. Una vez que salga de esta página, ya no se volverá a mostrar.

    > [!IMPORTANT]
    > La contraseña es una credencial de seguridad importante. No la comparta con nadie ni la distribuya en una aplicación cliente.

## <a name="secrets"></a>Incorporación de información de la cuenta de Microsoft a la aplicación de App Service

1. Vaya a la aplicación en [Azure Portal].
1. Haga clic en **Configuración** > **Autenticación/autorización** y asegúrese de que la opción **Autenticación de App Service** está **Activada**.
1. En **Authentication Providers** (Proveedores de autenticación), seleccione **Microsoft Account** (Cuenta de Microsoft). Pegue el identificador de la aplicación (cliente) y el secreto de cliente que obtuvo anteriormente. Habilite los ámbitos que necesite la aplicación.
1. Seleccione **Aceptar**.

   App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio ni a las API. Debe autorizar a los usuarios en el código de la aplicación.

1. (Opcional) Para restringir el acceso de los usuarios a la cuenta de Microsoft, configure la **acción necesaria cuando la solicitud no esté autenticada** en **Log in with Microsoft Account** (Iniciar sesión con la cuenta Microsoft). Al establecer esta funcionalidad, la aplicación requiere que se autentiquen todas las solicitudes. También redirige todas las solicitudes no autenticadas a la cuenta de Microsoft para la autenticación.

   > [!CAUTION]
   > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no interesar en las aplicaciones que tienen una página principal disponible públicamente así como en muchas aplicaciones de página única. Para tales aplicaciones, puede ser preferible **Permitir solicitudes anónimas (ninguna acción)** y que la aplicación inicie manualmente la autenticación. Para más información, consulte [Flujo de autenticación](overview-authentication-authorization.md#authentication-flow).

1. Seleccione **Guardar**.

De este modo ya estará listo para usar la cuenta Microsoft para realizar la autenticación en la aplicación.

## <a name="related-content"> </a>Pasos siguientes

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure Portal]: https://portal.azure.com/
