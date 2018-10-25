---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: d8d7d5649ca1dc215f85f928d111ff1367c60bb5
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988296"
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Prueba de consultas de Microsoft Graph API desde la aplicación iOS

Para ejecutar el código en el simulador, presione **Comando** + **R**.

![Probar la aplicación en el simulador](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Cuando esté listo para realizar la prueba, seleccione **Llamar a Microsoft Graph API**. Cuando se le pida, escriba su nombre de usuario y la contraseña.

### <a name="provide-consent-for-application-access"></a>Consentimiento para el acceso a la aplicación

La primera vez que inicie sesión en la aplicación, también se le pedirá que dé su consentimiento para permitir que la aplicación obtenga acceso al perfil e inicie sesión automáticamente:

![Consentimiento para el acceso a la aplicación](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Visualización de los resultados de la aplicación
Una vez iniciada la sesión, podrá ver la información del perfil de usuario que devolvió la llamada a Microsoft Graph API en la sección **Registro**. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Más información sobre los ámbitos y permisos delegados

Microsoft Graph API requiere el ámbito **user.read** para leer el perfil del usuario. Este ámbito se agrega automáticamente de forma predeterminada en todas las aplicaciones que se registran en el portal de registro. Otras API de Microsoft Graph, así como las API personalizadas para el servidor back-end, pueden requerir ámbitos adicionales. Microsoft Graph API requiere el ámbito **Calendars.read** para mostrar los calendarios del usuario.

Para tener acceso a los calendarios del usuario en el contexto de una aplicación, agregue el permiso delegado **Calendars.Read** a la información del registro de la aplicación. A continuación, agregue el ámbito **Calendars.Read** a la llamada **acquireTokenSilent**.

>[!NOTE]
>Es posible que se pida al usuario algún consentimiento adicional a medida que aumente el número de ámbitos.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
