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
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 991709ee635872e33dc89dcededc7f6ac3b28ea3
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48842841"
---
## <a name="test-your-app"></a>Prueba de la aplicación

1. Implemente el código en el dispositivo o emulador.

2. Pruebe a iniciar sesión con una cuenta de Azure Active Directory (cuenta profesional o educativa) o una cuenta Microsoft (live.com, outlook.com). 

    ![Prueba de la aplicación](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Introducción del nombre de usuario y contraseña](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="consent-to-your-app"></a>Consentimiento para su aplicación
La primera vez que un usuario inicia sesión en su aplicación, se le pedirá que dé su consentimiento a los permisos que necesita su aplicación, como se muestra aquí: 

![Consentimiento para el acceso a la aplicación](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="success"></a>¡Éxito!
Después de iniciar sesión y dar su consentimiento, la aplicación mostrará la respuesta desde Microsoft Graph API. Esta llamada específica es al punto de conexión **/me** y devuelve el [perfil de usuario](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_get). Para obtener una lista de otros puntos de conexión de Microsoft Graph, consulte la [documentación para desarrolladores de Microsoft Graph API](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="scopes-and-delegated-permissions"></a>Ámbitos y permisos delegados

Microsoft Graph API requiere el ámbito *User.Read* para leer el perfil de un usuario. Este ámbito se agrega automáticamente en todas las aplicaciones registradas en el Portal de registro de aplicaciones. Otras API requerirán ámbitos adicionales. Por ejemplo, Microsoft Graph API requiere el ámbito *Calendars.Read* para mostrar los calendarios del usuario. 

Para acceder a los calendarios del usuario, agregue el permiso delegado *Calendars.Read* a la información del registro de la aplicación. A continuación, agregue el ámbito *Calendars.Read* a la llamada `acquireTokenSilent`. 

>[!NOTE]
>A los se les puede pedir consentimiento adicional si cambia el registro de la aplicación.

<!--end-collapse-->

[!INCLUDE [Help and support](active-directory-develop-help-support-include.md)]
