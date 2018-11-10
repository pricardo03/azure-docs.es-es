---
title: Depuración de aplicaciones en un contenedor de Docker local | Microsoft Docs
description: Obtenga información acerca de cómo modificar una aplicación que se ejecuta en un contenedor de Docker local, a actualizar el contenedor mediante la edición y actualización, y a establecer puntos de interrupción para la depuración
services: container-service
author: ghogen
manager: douge
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.topic: article
ms.workload: multiple
ms.date: 09/11/2018
ms.author: ghogen
ms.openlocfilehash: 8844992ffbed2d57ad54a5f0e4a2d0b05c1d277a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968733"
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Depuración de aplicaciones en un contenedor de Docker local
## <a name="overview"></a>Información general
Visual Studio 2017 ofrece una forma coherente de desarrollar en un contenedor de Docker y validar la aplicación localmente.
sin tener que reiniciar el contenedor cada vez que se realiza un cambio en el código.
En este artículo se explica cómo utilizar la característica "Editar y actualizar" para iniciar una aplicación web de ASP.NET Core en un contenedor de Docker local, realizar los cambios necesarios y actualizar el explorador para ver dichos cambios.
También se muestra cómo establecer puntos de interrupción para la depuración.

## <a name="prerequisites"></a>Requisitos previos
Se deben instalar las siguientes herramientas.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) con la carga de trabajo Desarrollo de Web instalada.

Para ejecutar los contenedores de Docker de forma local, se necesita a un cliente Docker local.
Puede usar la versión de [Docker Toolbox](https://www.docker.com/products/docker-toolbox) que requiere que Hyper-V esté deshabilitado, o bien puede usar [Docker para Windows](https://www.docker.com/get-docker), que usa Hyper-V y requiere Windows 10.

Si usa Docker Toolbox, tendrá que [configurar el cliente Docker](vs-azure-tools-docker-setup.md).

## <a name="1-create-a-web-app"></a>1. Creación de una aplicación web
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-edit-your-code-and-refresh"></a>2. Edición del código y actualización
Para iterar rápidamente los cambios, puede iniciar la aplicación dentro de un contenedor, seguir realizando cambios y verlos como lo haría con IIS Express.

1. Establezca la Configuración de soluciones en `Debug` y presione **&lt;CTRL + F5 >** para generar la imagen de Docker y ejecutarla localmente.

    Cuando se haya creado la imagen del contenedor y se esté ejecutando en un contenedor de Docker, Visual Studio iniciará la aplicación web en el explorador predeterminado.
    Si utiliza el explorador Microsoft Edge o aparece algún error, consulte la sección [Solución de problemas](vs-azure-tools-docker-troubleshooting-docker-errors.md) .
2. Vaya a la página Acerca de, que es donde vamos a realizar los cambios.
3. Vuelva a Visual Studio y abra `Views\Home\About.cshtml`.
4. Agregue el siguiente contenido HTML al final del archivo y guarde los cambios.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. Viendo la ventana de salida, cuando se haya completado la compilación de .NET y vea estas líneas, vuelva al explorador y actualice la página Acerca de.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```

6. Se han aplicado los cambios.

## <a name="3-debug-with-breakpoints"></a>3. Depuración con puntos de interrupción
A menudo, será preciso inspeccionar más detalladamente los cambios realizados, para lo que se puede sacar provecho de las características de depuración de Visual Studio.

1. Vuelva a Visual Studio y abra `About.cshtml.cs`
2. Reemplace el contenido del método OnGet() por el siguiente:

   ```cs
       Message = "Your application description page from within a Container";
   ```

3. Establezca un punto de interrupción a la izquierda de la línea de código.
4. Presione **&lt;F5 >** para iniciar la depuración.
5. Navegue hasta la página About para llegar al punto de interrupción.
6. Cambie a Visual Studio para ver el punto de interrupción e inspeccione el valor del mensaje.

   ![][2]

## <a name="summary"></a>Resumen
Con la compatibilidad de Docker en Visual Studio 2017 puede conseguir una productividad similar a la de trabajar de forma local, con el realismo que produce desarrollar en un contenedor de Docker.

## <a name="troubleshooting"></a>solución de problemas
[Solución de problemas de desarrollo de Docker en Visual Studio](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Más información acerca de Docker con Visual Studio, Windows y Azure
* [Desarrollo de contenedores con Visual Studio](/visualstudio/containers): una página del centro de desarrollo de contenedores
* [Integración de Docker para Azure Pipelines](http://aka.ms/dockertoolsforvsts): compilación e implementación de contenedores de Docker
* [Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) : servicios de lenguaje para editar archivos de Docker, a los que próximamente se incorporarán más escenarios de E2E
* [Documentación acerca de los contenedores de Windows](http://aka.ms/containers): información de Windows Server y Nano Server
* [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) - [Documentación de Azure Kubernetes Service](/azure/aks)

## <a name="various-docker-tools"></a>Varias herramientas de Docker
[Some great docker tools (Steve Lasker's blog) (Herramientas excelentes de Docker [blog de Steve])](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Buenos artículos
[Introduction to Microservices (Introducción a Microservicios) de NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Presentaciones
* [Steve Lasker: VS Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Introduction to ASP.NET Core build 2016 - Where You At Demo](https://channel9.msdn.com/Events/Build/2016/B810) (Introducción a ASP.NET Core @ compilación 2016)
* [Developing .NET apps in containers, Channel 9 (Desarrollo de aplicaciones .NET en contenedores, Channel 9)](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
