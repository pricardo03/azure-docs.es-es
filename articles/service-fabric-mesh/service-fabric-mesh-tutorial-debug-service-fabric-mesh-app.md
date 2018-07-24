---
title: 'Tutorial: Depuración de una aplicación web de Azure Service Fabric Mesh | Microsoft Docs'
description: En este tutorial, aprenderá a depurar una aplicación de Azure Service Fabric Mesh en ejecución en el clúster local.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: e39e5861c0606c43a869fe02a7de2dc9b6f489ea
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125660"
---
# <a name="tutorial-debug-a-service-fabric-mesh-web-application"></a>Tutorial: Depuración de una aplicación web de Service Fabric Mesh

Este tutorial es la segunda parte de una serie y le muestra cómo depurar una aplicación web de Azure Service Fabric Mesh en el clúster de desarrollo local.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Ver qué sucede cuando compila una aplicación de Azure Service Fabric Mesh
> * Establecer un punto de interrupción para observar una llamada de servicio a servicio

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Crear una aplicación Service Fabric Mesh](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * Depurar localmente la aplicación
> * [Publicar la aplicación en Azure](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Asegúrese de que se haya [configurado el entorno de desarrollo](service-fabric-mesh-howto-setup-developer-environment-sdk.md), que incluye la instalación del entorno de ejecución de Service Fabric, el SDK, Docker y Visual Studio 2017.

## <a name="download-the-to-do-sample-application"></a>Descarga de la aplicación de ejemplo de tareas pendientes

Si no creó la aplicación de ejemplo de tareas pendientes en la [primera parte de esta serie de tutoriales](service-fabric-mesh-tutorial-create-dotnetcore.md), puede descargarla. En una ventana Comandos, ejecute el comando siguiente para clonar el repositorio de la aplicación de ejemplo en la máquina local.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

La aplicación se encuentra en el directorio `src\todolistapp`.

## <a name="build-and-debug-on-your-local-cluster"></a>Compilación y depuración en un clúster local

Tan pronto como se carga el proyecto, se crea e implementa una imagen de Docker en el clúster local. Este proceso puede tardar unos minutos. Para supervisar el progreso en el panel **Salida** de Visual Studio, establezca la opción **Mostrar salida de:**, de este panel, en **Herramientas de Service Fabric**.

Presione **F5** para compilar y ejecutar el servicio localmente. Cuando se haya ejecutado y depurado el proyecto localmente, Visual Studio:

* Se asegurará de que Docker para Windows se está ejecutando y está configurado para usar Windows como sistema operativo del contenedor.
* Descargue las imágenes de base de Docker que faltan. Esto puede tardar algún tiempo.
* Compile (o recompile) la imagen de Docker que se utiliza para hospedar el proyecto de código.
* Implemente y ejecute el contenedor en el clúster de desarrollo de Service Fabric local.
* Ejecute los servicios y alcance todos los puntos de interrupción que estableció.

Cuando finalice la implementación local y Visual Studio esté ejecutando la aplicación, se abrirá una ventana del explorador con una página web de ejemplo predeterminada.

**Sugerencias de depuración**

* Si recibe el error **No Service Fabric local cluster is running** (No hay ningún clúster local de Service Fabric en ejecución), asegúrese de que se está ejecutando el administrador de clústeres locales del servicio (SLCM) y haga clic con el botón derecho en el icono de SLCM en la barra de tareas y, después, en **Start Local Cluster** (Iniciar clúster local). Una vez que se haya iniciado, vuelva a Visual Studio y presione **F5**.
* Si se produce un error **404** al iniciarse la aplicación, esto probablemente significa que las variables de entorno que están en **service.yaml** son incorrectas. Asegúrese de que `AppName`, `ApiHostPort` y `ServiceName` se han establecido correctamente según las instrucciones descritas en [Establecimiento de variables de entorno](#set-environment-variables).
* Si se producen errores de compilación en **service.yaml**, asegúrese de que se usan espacios, y no tabulaciones, para aplicar sangría a las líneas.

### <a name="debug-in-visual-studio"></a>Depurar en Visual Studio

Al depurar una aplicación de Service Fabric Mesh en Visual Studio se usa un clúster de desarrollo de Service Fabric local. Para ver cómo se recuperan las tareas pendientes desde el servicio back-end, depure en el método OnGet().
1. En el proyecto **WebFrontEnd**, abra **Páginas** > **Index.cshtml** > **Index.cshtml.cs** y establezca un punto de interrupción en el método **Get** (línea 17).
2. En el proyecto **ToDoService**, abra **TodoController.cs** y establezca un punto de interrupción en el método **OnGet** (línea 15).
3. Regrese al explorador y actualice la página. Alcanzará el punto de interrupción del método `OnGet()` del servicio front-end web. Puede inspeccionar la variable `backendUrl` para ver cómo las variables de entorno que definió en el archivo **service.yaml** se combinan en la dirección URL usada para ponerse en contacto con el servicio back-end.
4. Salte la llamada `client.GetAsync(backendUrl).GetAwaiter().GetResult())` (F10) y se alcanzará el punto de interrupción `Get()` del controlador. En este método, puede ver cómo se recupera la lista de tareas pendientes a partir de la lista en memoria.
5. Cuando haya terminado, detenga la depuración del proyecto en Visual Studio presionando **Mayús+F5**.
 
## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Ver qué sucede cuando compila una aplicación de Azure Service Fabric Mesh
> * Establecer un punto de interrupción para observar una llamada de servicio a servicio

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Publicación de una aplicación web de Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)