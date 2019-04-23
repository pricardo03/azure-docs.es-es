---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: c13c9ece54d512adf271532a4ac17876a88d733e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805147"
---
## <a name="test-your-code"></a>Prueba del código

### <a name="test-with-node"></a>Prueba con Node

Si no está utilizando Visual Studio, asegúrese de que el servidor web está activado.

1. Configure el servidor para que escuche un puerto TCP que esté basado en la ubicación del archivo **index.html**. Para Node, inicie el servidor web para que escuche el puerto; para ello, ejecute los siguientes comandos desde la carpeta de la aplicación en un símbolo del sistema de la línea de comandos:

    ```bash
    npm install
    node server.js
    ```
1. Abra el explorador y escriba http://<span></span>localhost:30662 o http://<span></span>localhost:{port}, donde **port** es el puerto que está escuchando el servidor web. Verá el contenido del archivo index.html y el botón **Iniciar sesión**.

<p><!-- -->

### <a name="test-with-visual-studio"></a>Pruebas con Visual Studio

Si usa Visual Studio, asegúrese de seleccionar la solución del proyecto y presione **F5** para ejecutar el proyecto. El explorador se abre en la ubicación http://<span></span>localhost:{port}, donde verá el botón **Iniciar sesión**.

## <a name="test-your-application"></a>Prueba de la aplicación

Cuando el explorador haya cargado el archivo index.html, haga clic en **Iniciar sesión**. Se le pedirá que inicie sesión con el punto de conexión de la plataforma de identidad de Microsoft:

![Inicie sesión en la cuenta de la aplicación SPA de JavaScript](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Consentimiento para el acceso a la aplicación

La primera vez que inicie sesión en la aplicación, también se le pedirá que dé su consentimiento para permitir que la aplicación obtenga acceso al perfil e inicie sesión automáticamente:

![Consentimiento para el acceso a la aplicación](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Visualización de los resultados de la aplicación

Una vez iniciada la sesión, podrá ver la información del perfil de usuario que se devolvió en la respuesta de Microsoft Graph API que se muestra en la página.

![Resultados esperados de la llamada a Microsoft Graph API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Más información sobre los ámbitos y permisos delegados

Microsoft Graph API requiere el ámbito `user.read` para leer un perfil de usuario. Este ámbito se agrega automáticamente de forma predeterminada en todas las aplicaciones que se registran en el portal de registro. Otras API de Microsoft Graph, así como las API personalizadas para el servidor back-end, pueden requerir ámbitos adicionales. Por ejemplo, Microsoft Graph API requiere el ámbito `Calendars.Read` para mostrar los calendarios del usuario.

Para tener acceso a los calendarios del usuario en el contexto de una aplicación, agregue el permiso delegado `Calendars.Read` a la información del registro de la aplicación. A continuación, agregue el ámbito `Calendars.Read` a la llamada `acquireTokenSilent`.

>[!NOTE]
>Es posible que se pida al usuario algún consentimiento adicional a medida que aumente el número de ámbitos.

Si una API de back-end no requiere un ámbito (no se recomienda), puede usar `clientId` como ámbito en las llamadas de adquisición de tokens.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]