---
title: 'Configuración de la autenticación de Facebook: Azure App Service'
description: Obtenga información acerca de cómo configurar la autenticación mediante Facebook para la aplicación de Servicios de aplicaciones.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/06/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: e91d55c29d325301b8ac70ddc63fb408961fbb2c
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742980"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Configuración de la aplicación de App Service para usar el inicio de sesión de Facebook
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este tema se muestra cómo configurar Azure App Service para usar Facebook como proveedor de autenticación.

Para llevar a cabo el procedimiento descrito en este tema, debe tener una cuenta de Facebook asociada a una dirección de correo electrónico verificada y a un número de teléfono móvil. Para crear una cuenta de Facebook, vaya a [facebook.com].

## <a name="register"></a>Registro de la aplicación con Facebook
1. Desplácese hasta el sitio web para [desarrolladores de Facebook] e inicie sesión con las credenciales de su cuenta de Facebook.
3. (Opcional) Si no tienes un Facebook para la cuenta de los desarrolladores, haga clic en **comenzar** y siga los pasos de registro.
4. Haga clic en **mis aplicaciones** > **agregar nueva aplicación**.
5. En **Nombre para mostrar**, escriba un nombre único para la aplicación. Proporcione, también, el **Correo electrónico de contacto** y, a continuación, haga clic en **Create App ID** (Crear id. de aplicación) y complete la comprobación de seguridad. Esto le llevará al panel del desarrollador de la nueva aplicación de Facebook.
6. Haga clic en **panel** > **inicio de sesión de Facebook** > **configurar** > **Web**.
1. En el panel de navegación izquierdo bajo **inicio de sesión de Facebook**, haga clic en **configuración**.
1. En **URI de redireccionamiento OAuth válido**, tipo `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback` y reemplace  *\<app-name >* con el nombre de la aplicación de Azure App Service. Haga clic en **guardar cambios**.
8. En el panel de navegación izquierdo, haga clic en **Configuración** > **Básica**. En el **secreto de la aplicación** , a continuación, haga clic en **mostrar**. Copie los valores de **Id. de aplicación** y **secreto de la aplicación**. Usarlas más adelante para configurar la aplicación de App Service en Azure.
   
   > [!IMPORTANT]
   > El secreto de aplicación es una credencial de seguridad importante, No comparta este secreto con nadie ni lo distribuya en una aplicación cliente.
   > 
   > 
9. La cuenta de Facebook que se utilizó para registrar la aplicación es un administrador de la aplicación. En este momento, solo los administradores pueden iniciar sesión en esta aplicación. Para autenticar otras cuentas de Facebook, haga clic en **revisión de la aplicación** y habilitar **realizar \<your-app-name > public** para habilitar el acceso público general mediante la autenticación de Facebook.

## <a name="secrets"></a>Agregar información de Facebook a la aplicación
1. Inicie sesión en el [Azure Portal] y vaya a la aplicación de App Service. Haga clic en **Configuración** > **Autenticación/autorización** y asegúrese de que **Autenticación de App Service** está **Activada**.
2. Haga clic en **Facebook**, pegue los valores de Id. de aplicación y Secreto de la aplicación que obtuvo anteriormente, opcionalmente habilite los ámbitos que requiera la aplicación y haga clic en **Aceptar**.
   
    ![][0]
   
    De forma predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio y a las API. Debe autorizar a los usuarios en el código de la aplicación.
3. (Opcional) Para restringir el acceso al sitio solo a los usuarios autenticados mediante Facebook, en **Acción necesaria cuando la solicitud no está autenticada**, seleccione **Facebook**. Esto requiere que todas las solicitudes se autentiquen y que todas las solicitudes no autenticadas se redirijan a Facebook para la autenticación.
4. Cuando termine de configurar la autenticación, haga clic en **Guardar**.

De este modo ya estará listo para usar Facebook para realizar la autenticación en la aplicación.

## <a name="related-content"></a>Contenido relacionado
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Desarrolladores de Facebook]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure Portal]: https://portal.azure.com/
