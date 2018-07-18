---
title: Depuración de módulos de Functions para Azure IoT Edge | Microsoft Docs
description: Uso de Visual Studio Code para depurar Azure Functions de C# con Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5ee5d368bde0f176442a01ba266791f8a9c2cbc3
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052834"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Uso de Visual Studio Code para desarrollar y depurar Azure Functions para Azure IoT Edge

En este artículo se proporcionan instrucciones detalladas para usar [Visual Studio Code](https://code.visualstudio.com/) para depurar Azure Functions en IoT Edge.

## <a name="prerequisites"></a>requisitos previos
En este artículo se supone que utiliza un equipo o una máquina virtual con Windows o Linux como máquina de desarrollo. El dispositivo de IoT Edge podría ser otro dispositivo físico o una simulación en la máquina de desarrollo.

> [!NOTE]
> En este tutorial de depuración se describe cómo adjuntar un proceso en un contenedor de módulos y depurarlo con VS Code. Los módulos de C# solo se pueden depurar en contenedores linux-amd64. Si no está familiarizado con las funcionalidades de depuración de Visual Studio Code, consulte más información sobre [depuración](https://code.visualstudio.com/Docs/editor/debugging). 

Este artículo usa Visual Studio Code como herramienta de desarrollo principal. Instale VS Code y agregue las extensiones necesarias: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Extensión de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para crear un módulo, necesita .NET para crear la carpeta del proyecto, Docker para compilar la imagen del módulo y un registro de contenedor para guardar esta:
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/) instalado en la máquina de desarrollo. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Puede usar un registro de Docker local para fines de prototipo y pruebas en lugar de un registro en la nube. 

Para probar el módulo en un dispositivo, necesita una instancia de IoT Hub con al menos un dispositivo IoT Edge. Si desea utilizar el equipo como un dispositivo IoT Edge, puede hacerlo siguiendo los pasos descritos en los tutoriales para [Windows](quickstart.md) o [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Creación de una plantilla de solución

Los pasos siguientes muestran cómo crear una solución de IoT Edge que contiene un módulo de C# Function. Cada solución puede contener varios módulos.

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado**.
3. Seleccione **Ver** > **Paleta de comandos**.
4. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución IoT Edge). 

   ![Ejecución de la nueva solución IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Vaya a la carpeta en la que desea crear la nueva solución y haga clic en **Seleccionar carpeta**. 
6. Especifique un nombre para la solución. 
7. Elija **Azure Functions - C#** como plantilla del primer módulo de la solución.
8. Especifique un nombre para el módulo. Elija un nombre que sea único dentro del registro de contenedor. 
9. Especifique el repositorio de imágenes del módulo. VS Code rellena automáticamente el nombre del módulo, por lo que solo tiene que reemplazar **localhost:5000** por la información de su propio registro. Si usa un registro de Docker local para realizar pruebas, localhost puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a **\<nombre del registro\>.azurecr.io**.

VS Code toma la información que le ha proporcionado, crea una solución IoT Edge con un proyecto Function y la carga en una nueva ventana.

En la solución tiene tres elementos: 

* Una carpeta **.vscode** que contiene configuraciones de depuración.
* Una carpeta **modules** que contiene subcarpetas para cada módulo. En la actualidad solo tiene uno, pero podría agregar más a través de la paleta de comandos con el comando **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: agregar módulo IoT Edge).
* Un archivo **.env** enumera las variables de entorno. Si dispone de ACR como registro, en este momento ya tiene el nombre de usuario y la contraseña de ACR en este. 
* Un archivo **deployment.template.json** muestra el nuevo módulo junto con un módulo **tempSensor** de ejemplo que simula los datos que puede usar para realizar pruebas. Para más información sobre el funcionamiento de los manifiestos de implementación, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Compilación de un módulo de función de IoT Edge para fines de depuración
1. Para iniciar la depuración, debe usar el archivo **Dockerfile.amd64.debug** para recompilar la imagen de Docker e implementar de nuevo la solución de Edge. En el explorador de VS Code, navegue al archivo `deployment.template.json`. Para actualizar la dirección URL de la imagen de la función, agregue un `.debug` al final.

    ![Compilar una imagen de depuración](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Recompile la solución. En la paleta de comandos de VS Code, escriba y ejecute el comando **Azure IoT Edge: Build IoT Edge solution** (Azure IoT Edge: compilar solución IoT Edge).
3. En el Explorador de dispositivos de Azure IoT Hub, haga clic con el botón derecho en un identificador de dispositivo de IoT Edge y, a continuación, seleccione **Create deployment for Edge device** (Crear implementación para dispositivo Edge). Seleccione el archivo `deployment.json` en la carpeta `config`. A continuación, puede ver que la implementación se ha creado correctamente con un identificador de implementación en el terminal integrado de VS Code.

Puede comprobar el estado del contenedor en el Explorador de Docker de VS Code o mediante la ejecución del comando `docker images` en el terminal.

## <a name="start-debugging-c-function-in-vs-code"></a>Inicio de la depuración de la función de C# en VS Code
1. VS Code sigue depurando la información de configuración en un archivo `launch.json` ubicado en una carpeta `.vscode` en el área de trabajo. Este archivo `launch.json` se ha generado al crear una nueva solución de IoT Edge. Se actualiza cada vez que agregue un nuevo módulo que admita la depuración. Vaya a la vista de depuración y seleccione el archivo de configuración de depuración correspondiente. El nombre de la opción de depuración debe ser algo así como **ModuleName Remote Debug (.NET Core)** ![Seleccionar configuración de depuración](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Vaya a `run.csx`. Agregue un punto de interrupción en la función.
3. Haga clic en el botón **Iniciar depuración** o presione **F5** y seleccione el proceso que se adjuntará.
4. En la vista de depuración de VS Code, puede ver las variables en el panel izquierdo. 


> [!NOTE]
> En el ejemplo anterior se muestra cómo depurar la función .Net Core IoT Edge en contenedores. Se basa en la versión de depuración de `Dockerfile.amd64.debug`, que incluye VSDBG (el depurador de línea de comandos de .NET Core) en la imagen de contenedor al realizar la compilación. Se recomienda utilizar directamente o personalizar `Dockerfile` sin VSDBG para funciones de IoT Edge listas para entornos de producción cuando termine de depurar las funciones de C#.

## <a name="next-steps"></a>Pasos siguientes

Una vez que tenga el módulo compilado, aprenda a [implementar módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md)
