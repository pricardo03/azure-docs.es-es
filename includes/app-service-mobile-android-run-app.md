---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: 63c54f8af91b6b4a76ba49d5e6fc7b3cda9f5b98
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240309"
---
1. Abra el proyecto mediante **Android Studio**, con **Importar proyecto (Eclipse ADT, Gradle, etc.)** . Asegúrese de que realice esta selección de importación para evitar los errores del JDK.

2. Abra el archivo `ToDoActivity.java` en esta carpeta - ZUMOAPPNAME/aplicación/src/main/java/com/ejemplo/zumoappname. Es el nombre de la aplicación `ZUMOAPPNAME`.

3. Vaya a la [portal Azure](https://portal.azure.com/) y vaya a la aplicación móvil que ha creado. En el `Overview` hoja, busque la dirección URL que es el punto de conexión público para la aplicación móvil. Por ejemplo, será el nombre del sitio para el nombre de mi aplicación "test123" https://test123.azurewebsites.net.

4. En `onCreate()` método, reemplace `ZUMOAPPURL` parámetro con el punto de conexión público anterior.
    
    `new MobileServiceClient("ZUMOAPPURL", this).withFilter(new ProgressFilter());` 
    
    se convierte en
    
    `new MobileServiceClient("https://test123.azurewebsites.net", this).withFilter(new ProgressFilter());`
    
5. Presione el botón **Ejecutar "aplicación"** para compilar el proyecto e iniciar la aplicación en el simulador de Android.

4. En la aplicación, escriba un texto significativo, como *Realizar el tutorial* y luego haga clic en el botón "Agregar". Esto envía una solicitud POST al back-end de Azure implementado anteriormente. El back-end inserta los datos de la solicitud en la tabla SQL TodoItem y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista.
    ![Inicio rápido de Android](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)