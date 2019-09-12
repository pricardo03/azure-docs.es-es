---
title: Configuración de la autenticación de Google en Azure App Service
description: Obtenga información acerca de cómo configurar la autenticación mediante Google para la aplicación App Service.
services: app-service
documentationcenter: ''
author: cephalin
manager: gwallace
editor: ''
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/02/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: fcbb284a0807ef88c5f40a7c8b65398d45bf73d7
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232137"
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Configuración de la aplicación de App Service para usar el inicio de sesión de Google
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este tema se muestra cómo configurar Azure App Service para usar Google como proveedor de autenticación.

Para llevar a cabo el procedimiento descrito en este tema, debe tener una cuenta de Google asociada a una dirección de correo electrónico verificada. Para crear una cuenta de Google, vaya a [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"></a>Registro de la aplicación con Google
1. Siga la documentación de Google sobre el [inicio de sesión de Google para aplicaciones de servidor](https://developers.google.com/identity/sign-in/web/server-side-flow) para crear un identificador de cliente y un secreto de cliente, con la siguiente información (no es necesario realizar ningún cambio de código):
    - En **Authorized JavaScript Origins** (Orígenes de JavaScript autorizados), use `https://<app-name>.azurewebsites.net` con el nombre de su aplicación en *\<app-name>* .
    - En **Authorized Redirect URI** (URI de redireccionamiento autorizado), use `https://<app-name>.azurewebsites.net/.auth/login/google/callback`.
1. Una vez creados el identificador de cliente y el secreto de cliente, copie sus valores.

    > [!IMPORTANT]
    > El secreto de cliente es una credencial de seguridad importante, No comparta este secreto con nadie ni lo distribuya en una aplicación cliente.


## <a name="secrets"></a>Adición de información de Google a la aplicación
1. En [Azure Portal], vaya a su aplicación de App Service. En el menú izquierdo, seleccione **Autenticación / Autorización**.
2. Si esta característica no está habilitada, mueva el interruptor a la posición de **activada**.
3. Haga clic en **Google**. Pegue los valores de identificador de la aplicación y de secreto de la aplicación que obtuvo previamente y habilite opcionalmente los ámbitos que requiere la aplicación. A continuación, haga clic en **Aceptar**.

   App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio y a las API. Para obtener más información, consulte [Autorización o denegación de usuarios](app-service-authentication-how-to.md#authorize-or-deny-users).
4. (Opcional) Para restringir el acceso al sitio solo a los usuarios autenticados mediante Google, en **Acción necesaria cuando la solicitud no está autenticada**, seleccione **Google**. Esto requiere que todas las solicitudes se autentiquen y que todas las solicitudes no autenticadas se redirijan a Google para la autenticación.

    > [!NOTE]
    > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no ser deseable para las aplicaciones que necesitan una página de inicio disponible públicamente, como muchas aplicaciones de una sola página. Para tales aplicaciones, puede ser preferible **permitir las solicitudes anónimas (sin acción)** y que la aplicación inicie manualmente el inicio de sesión, tal como se describe [aquí](overview-authentication-authorization.md#authentication-flow).
    
5. Haga clic en **Save**(Guardar).

De este modo ya estará listo para usar Google para realizar la autenticación en la aplicación.

## <a name="related-content"></a>Contenido relacionado
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure Portal]: https://portal.azure.com/

