---
title: 'Configuración de la autenticación de Twitter: Azure App Service'
description: Obtenga información acerca de cómo configurar la autenticación mediante Twitter para la aplicación App Service.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: d49b8bf9c62813023c1a1e06e0f8fc0d7809f48d
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232061"
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Configuración de la aplicación de App Service para usar el inicio de sesión de Twitter
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este tema se muestra cómo configurar Azure App Service para usar Twitter como proveedor de autenticación.

Para llevar a cabo el procedimiento descrito en este tema, debe tener una cuenta de Twitter con una dirección de correo electrónico y un número de teléfono verificados. Para crear una cuenta de Twitter, vaya a <a href="https://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"></a>Registro de la aplicación con Twitter
1. Inicie sesión en el [Azure Portal]y vaya a la aplicación. Copie el valor de **Dirección URL**. La usará para configurar la aplicación de Twitter.
2. Vaya al sitio web para [desarrolladores de Twitter] , inicie sesión con las credenciales de la cuenta de Twitter y haga clic en **Crear nueva aplicación**.
3. Escriba el **nombre** y una **descripción** de la nueva aplicación. Pegue la **dirección URL** de la aplicación como valor de **Sitio web**. A continuación, para la **dirección URL de devolución de llamada**, escriba la dirección URL de la aplicación App Service y anexe la ruta de acceso `/.auth/login/aad/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Asegúrese de que está utilizando el esquema HTTPS.
4. En la parte inferior de la página, lea y acepte los términos. A continuación, haga clic en **Crear aplicación de Twitter**. Se muestran los detalles de la aplicación.
5. Haga clic en la pestaña **Configuración**, seleccione **Allow this application to be used to sign in with Twitte** (Permitir que esta aplicación se use para iniciar sesión en Twitter) y, después, haga clic en **Update Settings** (Actualizar configuración).
6. Seleccione la pestaña **Claves y tokens de acceso** . Tome nota de los valores de **Consumer Key (API Key)** [Clave de consumidor (clave de API)] y **Consumer secret (API Secret)** [Secreto de consumidor (secreto de API)].
   
   > [!NOTE]
   > El secreto de consumidor es una credencial de seguridad importante, por lo que no debe compartirlo con nadie ni distribuirlo con su aplicación.
   > 
   > 

## <a name="secrets"></a>Adición de información de Twitter a su aplicación
1. Vuelva al [Azure Portal]y vaya a la aplicación. Haga clic en **Configuración** y luego en **Autenticación/autorización**.
2. Si esta característica no está habilitada, mueva el interruptor a la posición de **activada**.
3. Haga clic en **Twitter**. Pegue los valores de identificador y secreto de la aplicación que obtuvo anteriormente. A continuación, haga clic en **Aceptar**.
   
   ![][1]
   
   De forma predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio y a las API. Debe autorizar a los usuarios en el código de la aplicación.
4. (Opcional) Para restringir el acceso al sitio solo a los usuarios autenticados mediante Twitter, establezca **Acción necesaria cuando la solicitud no está autenticada** en **Twitter**. Esto requiere que todas las solicitudes se autentiquen y que todas las solicitudes no autenticadas se redirijan a Twitter para la autenticación.

> [!NOTE]
> Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no ser deseable para las aplicaciones que necesitan una página de inicio disponible públicamente, como muchas aplicaciones de una sola página. Para tales aplicaciones, puede ser preferible **permitir las solicitudes anónimas (sin acción)** y que la aplicación inicie manualmente el inicio de sesión, tal como se describe [aquí](overview-authentication-authorization.md#authentication-flow).

5. Haga clic en **Save**(Guardar).

De este modo ya estará listo para usar Twitter para realizar la autenticación en la aplicación.

## <a name="related-content"></a>Contenido relacionado
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[desarrolladores de Twitter]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure Portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
