---
title: Configuración de la autenticación de Twitter
description: Aprenda a configurar la autenticación de Twitter como proveedor de identidades para una aplicación App Service.
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.topic: article
ms.date: 04/19/2018
ms.custom: seodec18
ms.openlocfilehash: b5ec17c18cec8053f0732366c0cc5d0c5003e4de
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74670795"
---
# <a name="configure-your-app-service-app-to-use-twitter-login"></a>Configuración de la aplicación de App Service para usar el inicio de sesión de Twitter

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este artículo se muestra cómo configurar Azure App Service para usar Twitter como proveedor de autenticación.

Para llevar a cabo el procedimiento descrito en este artículo, debe tener una cuenta de Twitter asociada a una dirección de correo electrónico verificada y a un número de teléfono. Para crear una cuenta de Twitter, vaya a [twitter.com].

## <a name="register"></a>Registro de la aplicación con Twitter

1. Inicie sesión en [Azure Portal] y vaya a la aplicación. Copie el valor de **Dirección URL**. Lo usará para configurar la aplicación de Twitter.
1. Vaya al sitio web para [desarrolladores de Twitter], inicie sesión con las credenciales de la cuenta de Twitter y seleccione **Create New App** (Crear nueva aplicación).
1. Escriba el **nombre** y una **descripción** de la nueva aplicación. Pegue la **dirección URL** de la aplicación en el campo **Website** (Sitio web). En el campo **Callback URL** (URL de devolución de llamadas), escriba la dirección URL de la aplicación de App Service, anexando la ruta de acceso `/.auth/login/aad/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Asegúrese de usar el esquema HTTPS.
1. En la parte inferior de la página, lea y acepte los términos. A continuación, haga clic en **Create your Twitter application** (Crear su aplicación de Twitter). Se muestran los detalles de la aplicación.
1. Seleccione la pestaña **Settings** (Configuración), marque **Allow this application to be used to sign in with Twitter** (Permitir que esta aplicación se use para iniciar sesión en Twitter) y, después, seleccione **Update Settings** (Actualizar configuración).
1. Seleccione la pestaña **Claves y tokens de acceso** .

   Anote estos valores:
   - Clave de consumidor (clave de API)
   - Secreto de consumidor (secreto de API)

   > [!NOTE]
   > El secreto de consumidor es una credencial de seguridad importante, por lo que no debe compartirlo con nadie ni distribuirlo con su aplicación.

## <a name="secrets"></a>Adición de información de Twitter a su aplicación

1. Vaya a la aplicación en [Azure Portal].
1. Haga clic en **Configuración** > **Autenticación/autorización** y asegúrese de que la opción **Autenticación de App Service** está **Activada**.
1. Seleccione **Twitter**.
1. Pegue los valores `API Key` y `API Secret` que obtuvo anteriormente.
1. Seleccione **Aceptar**.

   ![Captura de pantalla de la configuración de Twitter para la aplicación móvil][1]

   De forma predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio ni a las API. Debe autorizar a los usuarios en el código de la aplicación.

1. (Opcional) Para restringir el acceso al sitio solo a los usuarios autenticados mediante Twitter, establezca **Acción necesaria cuando la solicitud no está autenticada** en **Twitter**. Al establecer esta funcionalidad, la aplicación requiere que se autentiquen todas las solicitudes. También redirige todas las solicitudes no autenticadas a Twitter para la autenticación.

   > [!CAUTION]
   > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no interesar en las aplicaciones que tienen una página principal disponible públicamente así como en muchas aplicaciones de página única. Para tales aplicaciones, puede ser preferible **Permitir solicitudes anónimas (ninguna acción)** y que la aplicación inicie manualmente la autenticación. Para más información, consulte [Flujo de autenticación](overview-authentication-authorization.md#authentication-flow).

1. Seleccione **Guardar**.

De este modo ya estará listo para usar Twitter para realizar la autenticación en la aplicación.

## <a name="related-content"> </a>Pasos siguientes

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[desarrolladores de Twitter]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[twitter.com]: https://go.microsoft.com/fwlink/p/?LinkID=268287
[Azure Portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
