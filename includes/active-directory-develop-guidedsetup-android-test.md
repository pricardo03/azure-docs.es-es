---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: c8e5e4f826d7835a1fd38d1db5bfeab19b679b30
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203756"
---
## <a name="test-your-app"></a>Prueba de la aplicación

1. Implemente el código en el dispositivo o emulador.
2. Intente iniciar sesión con una cuenta de Azure Active Directory (cuenta profesional o educativa) o la cuenta de Microsoft (live.com, outlook.com). 

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

> [!NOTE]
> A los se les puede pedir consentimiento adicional si cambia el registro de la aplicación.

<!--end-collapse-->

[!INCLUDE [Help and support](active-directory-develop-help-support-include.md)]
