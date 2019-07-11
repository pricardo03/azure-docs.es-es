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
ms.openlocfilehash: 494f5fb6f2bfdec86cad01a37e57c3ec219a77f9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133412"
---
## <a name="test-your-code"></a>Prueba del código

Para probar el código, use cualquiera de estos entornos.

### <a name="test-with-nodejs"></a>Prueba con Node.js

Si no está utilizando Visual Studio, asegúrese de que el servidor web está activado.

1. Configure el servidor para que escuche un puerto TCP que esté basado en la ubicación del archivo *index.html*. Para Node.js, inicie el servidor web para que escuche el puerto; para ello, ejecute los siguientes comandos desde la carpeta de la aplicación en un símbolo del sistema de la línea de comandos:

    ```bash
    npm install
    node server.js
    ```
1. En el explorador, escriba **http://\<span>\</span>localhost:30662** o **http://\<span>\</span>localhost:{port}** , donde *port* es el puerto de escucha del servidor web. Verá el contenido del archivo *index.html* y el botón **Iniciar sesión**.

### <a name="test-with-visual-studio"></a>Pruebas con Visual Studio

Si usa Visual Studio, seleccione la solución del proyecto y, luego, F5 para ejecutar el proyecto. El explorador se abre en la ubicación http://<span></span>localhost:{port} y el botón **Iniciar sesión** debería verse.

## <a name="test-your-application"></a>Prueba de la aplicación

Cuando el explorador haya cargado el archivo *index.html*, seleccione **Iniciar sesión**. Se le pedirá que inicie sesión con el punto de conexión de la plataforma de identidad de Microsoft:

![La ventana de inicio de sesión en la cuenta de SPA de JavaScript](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Consentimiento para el acceso a la aplicación

La primera vez que inicie sesión en la aplicación, se le pedirá que le dé acceso a su perfil e inicie su sesión:

![La ventana "Permisos solicitados"](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Visualización de los resultados de la aplicación

Una vez iniciada la sesión, la información del perfil de usuario se devuelve en la respuesta de Microsoft Graph API que se muestra en la página.

![Resultados de la llamada a Microsoft Graph API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Más información sobre los ámbitos y permisos delegados

Microsoft Graph API requiere el ámbito *user.read* para leer el perfil del usuario. Este ámbito se agrega automáticamente de forma predeterminada en todas las aplicaciones que se registran en el portal de registro. Otras API de Microsoft Graph, así como las API personalizadas para el servidor back-end, pueden requerir ámbitos adicionales. Por ejemplo, Microsoft Graph API requiere el ámbito *Calendars.Read* para mostrar los calendarios del usuario.

Para tener acceso a los calendarios del usuario en el contexto de una aplicación, agregue el permiso delegado *Calendars.Read* a la información del registro de la aplicación. A continuación, agregue el ámbito *Calendars.Read* a la llamada `acquireTokenSilent`.

>[!NOTE]
>Es posible que se pida al usuario algún consentimiento adicional a medida que aumente el número de ámbitos.

Si una API de back-end no requiere un ámbito (no se recomienda), puede usar *clientId* como ámbito en las llamadas de adquisición de tokens.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
