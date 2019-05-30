---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: a5bde1a56bf6a1f5fca4b775c7c8e9bb7477eb6b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240266"
---
1. Abra el proyecto de cliente descargados mediante Xcode.

2. Vaya a la [portal Azure](https://portal.azure.com/) y vaya a la aplicación móvil que ha creado. En el `Overview` hoja, busque la dirección URL que es el punto de conexión público para la aplicación móvil. Por ejemplo, será el nombre del sitio para el nombre de mi aplicación "test123" https://test123.azurewebsites.net.

3. Para proyectos Swift, abra el archivo `ToDoTableViewController.swift` en esta carpeta - ZUMOAPPNAME/ZUMOAPPNAME/ToDoTableViewController.swift. Es el nombre de la aplicación `ZUMOAPPNAME`.

4. En `viewDidLoad()` método, reemplace `ZUMOAPPURL` parámetro con el punto de conexión público anterior.

    `let client = MSClient(applicationURLString: "ZUMOAPPURL")`

    se convierte en
    
    `let client = MSClient(applicationURLString: "https://test123.azurewebsites.net")`
    
5. Para proyectos de Objective-C, abra el archivo `QSTodoService.m` en esta carpeta - ZUMOAPPNAME/ZUMOAPPNAME. Es el nombre de la aplicación `ZUMOAPPNAME`.

6. En `init` método, reemplace `ZUMOAPPURL` parámetro con el punto de conexión público anterior.

    `self.client = [MSClient clientWithApplicationURLString:@"ZUMOAPPURL"];`

    se convierte en
    
    `self.client = [MSClient clientWithApplicationURLString:@"https://test123.azurewebsites.net"];`

7. Presione el botón **Ejecutar** para compilar el proyecto e iniciar la aplicación en el simulador de iOS.

8. En la aplicación, haga clic en el icono con el signo más ( **+** ), escriba texto que tenga sentido, como *Completar el tutorial* y, luego, haga clic en el botón para guardar. Esto envía una solicitud POST al back-end de Azure implementado anteriormente. El back-end inserta los datos de la solicitud en la tabla SQL TodoItem y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista.

   ![Aplicación de inicio rápido que se ejecuta en iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.com/
