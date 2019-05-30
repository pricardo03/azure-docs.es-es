---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 8d7731480b6239c572d39f52b6a0217d2ac48d25
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240222"
---
1. Navegue hasta el archivo de solución en el proyecto de cliente (.sln) y ábralo con Visual Studio.

2. En Visual Studio, elija la plataforma de la solución (Android, iOS o Windows) en la lista desplegable situada junto a la flecha de inicio. Para seleccionar un dispositivo de implementación o un emulador específicos, haga clic en la lista desplegable de la flecha verde. Puede usar la plataforma Android y el emulador Rippler predeterminados. Para tutoriales más avanzados (por ejemplo, las notificaciones de inserción), deberá seleccionar un dispositivo o emulador compatibles.

3. Abra el archivo `ToDoActivity.java` en esta carpeta - ZUMOAPPNAME/aplicación/src/main/java/com/ejemplo/zumoappname. Es el nombre de la aplicación `ZUMOAPPNAME`.

4. Vaya a la [portal Azure](https://portal.azure.com/) y vaya a la aplicación móvil que ha creado. En el `Overview` hoja, busque la dirección URL que es el punto de conexión público para la aplicación móvil. Por ejemplo, será el nombre del sitio para el nombre de mi aplicación "test123" https://test123.azurewebsites.net.

5. Vaya a la `index.js` archivo ZUMOAPPNAME/www/js/index.js y en `onDeviceReady()` método, reemplace `ZUMOAPPURL` parámetro con el punto de conexión público anterior.

    `client = new WindowsAzure.MobileServiceClient('ZUMOAPPURL');`
    
    se convierte en
    
    `client = new WindowsAzure.MobileServiceClient('https://test123.azurewebsites.net');`
    
6. Presione F5 o haga clic en la flecha verde para compilar y ejecutar su aplicación Cordova. Si ve en el emulador un cuadro de diálogo de seguridad que solicita el acceso a la red, acéptelo.

7. Después de la aplicación se inicia en el dispositivo o emulador, escriba un texto significativo en **escriba el nuevo texto**, tales como *completar el tutorial* y, a continuación, haga clic en el **agregar** botón.

El back-end inserta datos de la solicitud en la tabla TodoItem de SQL Database y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista.

Puede repetir los pasos del 3 al 5 para otras plataformas.