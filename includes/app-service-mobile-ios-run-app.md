---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: f4ba467b6d80c9ccafba0a91c1f04152b92cf869
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140667"
---
1. En el Mac, vaya a [Azure Portal]. Haga clic en **Todos los servicios** > **App Services** > el back-end que acaba de crear. En la configuración de la aplicación móvil, elija su lenguaje preferido:

   - Objective-C &ndash; **Inicio rápido** > **iOS (Objective-C)**
   - Swift &ndash; **Inicio rápido** > **iOS (Swift)**

     En **3. Configurar la aplicación cliente**, haga clic en **Descargar**. Esto descarga un proyecto de Xcode completo configurado previamente para conectarse a su back-end. Abra el proyecto con Xcode.

1. Presione el botón **Ejecutar** para compilar el proyecto e iniciar la aplicación en el simulador de iOS.

1. En la aplicación, haga clic en el icono con el signo más (**+**), escriba texto que tenga sentido, como *Completar el tutorial* y, luego, haga clic en el botón para guardar. Esto envía una solicitud POST al back-end de Azure implementado anteriormente. El back-end inserta los datos de la solicitud en la tabla SQL TodoItem y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista.

   ![Aplicación de inicio rápido que se ejecuta en iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure Portal]: https://portal.azure.com/
