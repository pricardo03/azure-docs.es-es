---
title: Depuración de módulos de función de Azure para Azure IoT Edge | Microsoft Docs
description: Uso de Visual Studio Code para depurar funciones de Azure de C# con Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: bf2d1af66cc3ecc35dafe3bcd43bf10399d71641
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346722"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Uso de Visual Studio Code para desarrollar y depurar funciones de Azure para Azure IoT Edge

En este artículo se muestra cómo usar [Visual Studio Code (VS Code)](https://code.visualstudio.com/) para depurar las funciones de Azure en Azure IoT Edge.

## <a name="prerequisites"></a>Requisitos previos
En este artículo se da por hecho que usa una máquina virtual que ejecuta Windows o Linux como máquina de desarrollo. El dispositivo IoT Edge puede ser otro dispositivo físico. O bien, puede simular el dispositivo IoT Edge en la máquina de desarrollo.

> [!NOTE]
> En este tutorial de depuración se demuestra cómo asociar un proceso en un contenedor de módulos y depurarlo con VS Code. Solo se pueden depurar funciones de C# en contenedores Linux-amd64. Si no está familiarizado con las funcionalidades de depuración de Visual Studio Code, consulte más información sobre [depuración](https://code.visualstudio.com/Docs/editor/debugging). 

Este artículo usa Visual Studio Code como herramienta de desarrollo principal. Instale VS Code. A continuación, agregue las extensiones necesarias: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Extensión de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para crear un módulo, necesita .NET para crear la carpeta del proyecto, Docker para compilar la imagen del módulo y un registro de contenedor para guardar esta:
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download).
* [Docker Community Edition](https://docs.docker.com/install/) en la máquina de desarrollo 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Puede usar un registro de Docker local con fines de prototipo y prueba en lugar de un registro en la nube. 

Para probar el módulo en un dispositivo, necesita una instancia de IoT Hub con al menos un dispositivo IoT Edge. Para usar el equipo como un dispositivo IoT Edge, siga los pasos descritos en el inicio rápido para [Windows](quickstart.md) o [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Creación de una plantilla de solución

Siga estos pasos para crear una solución de IoT Edge con un módulo de función de C#. Cada solución puede tener varios módulos.

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado**.
3. Seleccione **Ver** > **Paleta de comandos**.
4. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: nueva solución de IoT Edge). 

   ![Ejecución de la nueva solución de IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Seleccione la carpeta en la que desea crear la solución. Elija **Select folder** (Seleccionar carpeta). 
6. Escriba un nombre para la solución. 
7. Elija **Azure Functions - C#** como plantilla del primer módulo de la solución.
8. Escriba un nombre para el módulo. Elija un nombre que sea único dentro del registro de contenedor. 
9. Especifique el repositorio de imágenes del módulo. VS Code rellena automáticamente el nombre del módulo con **localhost:5000**. Reemplácelo por su propia información de registro. Si usa un registro de Docker local para realizar pruebas, **localhost** puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a **\<nombre del registro\>.azurecr.io**.

VS Code toma la información que le ha proporcionado, crea una solución de IoT Edge con un proyecto de Azure Functions y luego la carga en una nueva ventana.

Hay cuatro elementos dentro de la solución: 

* Una carpeta **.vscode** que contiene configuraciones de depuración.
* Una carpeta **modules** que contiene subcarpetas para cada módulo. En este momento, solo tiene uno. Sin embargo, podría agregar más mediante la paleta de comandos con el comando **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: agregar módulo IoT Edge).
* Un archivo **.env** enumera las variables de entorno. Si Azure Container Registry es su registro, tendrá en él un nombre de usuario y una contraseña de Azure Container Registry. 

   >[!NOTE]
   >Solo se crea el archivo de entorno si proporciona un repositorio de imágenes para el módulo. Si aceptó los valores predeterminados de localhost para probar y depurar localmente, no es necesario declarar las variables de entorno. 

* Un archivo **deployment.template.json** muestra el nuevo módulo junto con un módulo **tempSensor** de ejemplo que simula los datos que puede usar para las pruebas. Para más información sobre cómo funcionan los manifiestos de implementación, consulte [cómo usar los manifiestos de implementación para implementar módulos y establecer rutas](module-composition.md).

## <a name="develop-your-module"></a>Desarrollo de su módulo

El código de función de Azure predeterminado que se incluye con la solución se encuentra en **módulos** > **\<nombre del módulo\>** > **EdgeHubTrigger-Csharp** > **run.csx**. El módulo y el archivo deployment.template.json se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo para iniciar la prueba sin tocar el código. El módulo se compila para tomar simplemente la entrada de un origen (en este caso, el módulo tempSensor que simula datos) y canalizarlo a IoT Hub. 

Cuando esté listo para personalizar la plantilla de función de Azure con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como seguridad, administración de dispositivos y confiabilidad. 

## <a name="build-your-module-for-debugging"></a>Compilación de un módulo para la depuración
1. Para iniciar la depuración, use **Dockerfile.amd64.debug** para recompilar la imagen de Docker e implementar de nuevo la solución de Edge. En el explorador de VS Code, vaya al archivo `deployment.template.json`. Para actualizar la dirección URL de la imagen de la función, agregue `.debug` al final.

    ![Compilar una imagen de depuración](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Recompile la solución. En la paleta de comandos de VS Code, escriba y ejecute el comando **Azure IoT Edge: Build IoT Edge solution** (Azure IoT Edge: compilar solución IoT Edge).
3. En el Explorador de dispositivos de Azure IoT Hub, haga clic con el botón derecho en un identificador de dispositivo de IoT Edge y, a continuación, seleccione **Create deployment for Edge device** (Crear implementación para dispositivo Edge). Seleccione el archivo `deployment.json` de la carpeta `config`. Verá que la implementación se ha creado correctamente con un identificador de implementación en el terminal integrado de VS Code.

Puede comprobar el estado del contenedor en el Explorador de Docker de VS Code o mediante la ejecución del comando `docker images` en el terminal.

## <a name="start-debugging-c-functions-in-vs-code"></a>Inicio de la depuración de funciones de C# en VS Code
1. VS Code sigue depurando la información de configuración en un archivo `launch.json` ubicado en una carpeta `.vscode` en el área de trabajo. Este archivo `launch.json` se ha generado al crear una nueva solución de IoT Edge. Se actualiza cada vez que agregue un nuevo módulo que admita la depuración. Vaya a la vista de depuración. Seleccione el archivo de configuración de depuración correspondiente. El nombre de la opción de depuración debe ser similar a to **ModuleName Remote Debug (.NET Core)**.

   ![Selección de la configuración de depuración](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Vaya a `run.csx`. Agregue un punto de interrupción en la función.
3. Seleccione **Iniciar depuración** o presione **F5**. Seleccione el proceso al que desea asociarlo.
4. En la vista de depuración de VS Code, verá las variables en el panel izquierdo. 


> [!NOTE]
> En el ejemplo anterior se muestra cómo depurar las funciones de IoT Edge para .Net Core en contenedores. Se basa en la versión de depuración de `Dockerfile.amd64.debug`, que incluye VSDBG (el depurador de línea de comandos de .NET Core) en la imagen de contenedor al realizar la compilación. Se recomienda usar directamente o personalizar `Dockerfile` sin VSDBG para funciones de IoT Edge listas para producción después de depurar las funciones de C#.

## <a name="next-steps"></a>Pasos siguientes

Después de compilar el módulo, aprenda cómo [implementar módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md).

Para desarrollar módulos para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
