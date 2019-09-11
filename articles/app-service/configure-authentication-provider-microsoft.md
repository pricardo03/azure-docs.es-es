---
title: 'Configuración de la autenticación de cuenta de Microsoft: Azure App Service'
description: Obtenga información acerca de cómo configurar la autenticación mediante la cuenta Microsoft para la aplicación App Service.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/08/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 0832c1e5f10cdb8e1d7a2edbb88162230ab13401
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70233085"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Configuración de la aplicación de App Service para usar el inicio de sesión de la cuenta Microsoft
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este tema se muestra cómo configurar Azure App Service para usar la cuenta Microsoft como proveedor de autenticación. 

## <a name="register-microsoft-account"></a>Registro de la aplicación con la cuenta de Microsoft
1. Inicie sesión en [Azure Portal] y vaya a la aplicación. 

<!-- Copy your **URL**, which you will use later to configure your app with Microsoft Account. -->
1. Vaya a [**App Registrations**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) (Registros de aplicaciones) e inicie sesión con las credenciales de su cuenta Microsoft, si se le solicita.

1. Haga clic en **New registration** (Nuevo registro) y escriba el nombre de la aplicación.

1. En **Redirect URIs** (Redirigir los URI), seleccione **Web** y escriba `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application`. Reemplace *\<app-domain-name>* con el nombre de dominio de su aplicación.  Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`. 

   > [!NOTE]
   > Use el esquema HTTPS en la URL.

1. Seleccione **Register** (Registrar). 

1. Copie el **id. de la aplicación (cliente)** . Lo necesitará más adelante. 
   
7. En el panel de navegación izquierdo del nuevo registro de aplicaciones, seleccione **Certificates & secrets (Certificados y secretos)**  > **New client secret (Nuevo secreto del cliente)** . Proporcione una descripción, seleccione la duración de validez y seleccione **Add** (Agregar).

1. Copie el valor que aparece en la página **Certificates & secrets** (Certificados y secretos). Una vez que salga de esta página, la contraseña no se volverá a mostrar.

    > [!IMPORTANT]
    > La contraseña es una credencial de seguridad importante. No la comparta con nadie ni la distribuya en una aplicación cliente.

## <a name="secrets"></a>Incorporación de información de la cuenta de Microsoft a la aplicación de App Service
1. En el [Azure Portal], vaya a la aplicación. En el panel de navegación izquierdo, haga clic en **Authentication / Authorization** (Autenticación/Autorización).

2. Si la característica Authentication / Authorization (Autenticación/Autorización) no está habilitada, **actívela**.

3. En **Authentication Providers** (Proveedores de autenticación), seleccione **Microsoft Account** (Cuenta de Microsoft). Pegue el id. de la aplicación (cliente) y el secreto del cliente que obtuvo anteriormente y, opcionalmente, habilite los ámbitos que su aplicación requiera. A continuación, haga clic en **Aceptar**.

    De forma predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio y a las API. Debe autorizar a los usuarios en el código de la aplicación.

4. (Opcional) Para restringir el acceso de los usuarios a la cuenta de Microsoft, configure la **acción necesaria cuando la solicitud no esté autenticada** en **Log in with Microsoft Account** (Iniciar sesión con la cuenta Microsoft). Esto requiere que todas las solicitudes se autentiquen y que todas las solicitudes no autenticadas se redirijan a la cuenta Microsoft para la autenticación.

> [!NOTE]
> Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no ser deseable para las aplicaciones que necesitan una página de inicio disponible públicamente, como muchas aplicaciones de una sola página. Para tales aplicaciones, puede ser preferible **permitir las solicitudes anónimas (sin acción)** y que la aplicación inicie manualmente el inicio de sesión, tal como se describe [aquí](overview-authentication-authorization.md#authentication-flow).

5. Haga clic en **Save**(Guardar).

De este modo ya estará listo para usar la cuenta Microsoft para realizar la autenticación en la aplicación.

## <a name="related-content"></a>Contenido relacionado
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure Portal]: https://portal.azure.com/
