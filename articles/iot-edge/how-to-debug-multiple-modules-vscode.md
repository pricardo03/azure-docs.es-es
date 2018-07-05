---
title: Depuración de varios módulos para Azure IoT Edge en VS Code | Microsoft Docs
description: Uso de Visual Studio Code para depurar varios módulos con Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 38c66129ac8244d18b0f94c1485c785015e29ab4
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049598"
---
# <a name="use-visual-studio-code-to-debug-multiple-modules-with-azure-iot-edge"></a>Uso de Visual Studio Code para depurar varios módulos con Azure IoT Edge
En este artículo se proporcionan instrucciones detalladas para usar [Visual Studio Code](https://code.visualstudio.com/) para depurar varios módulos en IoT Edge.

## <a name="prerequisites"></a>requisitos previos
Complete el tutorial [Desarrollo de una solución de IoT Edge con varios módulos en Visual Studio Code](tutorial-multiple-modules-in-vscode.md) y asegúrese de que tiene al menos dos módulos en ejecución en el dispositivo IoT Edge.

## <a name="multi-target-and-remote-debugging-in-vs-code"></a>Múltiples destinos y depuración remota en VS Code
Con la extensión de Azure IoT Edge y VS Code, puede adjuntar el proceso de módulo en un contenedor, tanto si el contenedor se ejecuta en el equipo de desarrollo como si lo hace en un dispositivo físico IoT Edge remoto. Con la depuración de varios módulos en ejecución en contenedores, realmente se está asociando más de un proceso en contenedores diferentes. La [depuración de varios destinos](https://code.visualstudio.com/docs/editor/debugging#_multitarget-debugging) de VS Code puede utilizarse para depurar varios módulos.

   > [!TIP]
   > Si el contenedor de módulo se ejecuta en un dispositivo físico IoT Edge remoto, es posible que tenga que configurar la [máquina Docker](https://docs.docker.com/machine/overview/) para que el motor Docker en la máquina de desarrollo pueda comunicarse con los hosts remotos de Docker.

### <a name="build-your-iot-edge-modules-for-debugging-purpose"></a>Creación de módulos de IoT Edge con fines de depuración
1. Para iniciar la depuración de varios módulos, tiene que usar el archivo **Dockerfile.amd64.debug** para recompilar las imágenes de Docker e implementar de nuevo la solución de Edge. En el explorador de VS Code, navegue al archivo `deployment.template.json`. Actualice las direcciones URL de imagen agregando `.debug` al final. Necesita dos imágenes de módulo con `.debug` como mínimo. Si está trabajando en la solución del tutorial anterior, debe tener un módulo de funciones de C# y un módulo de C#. Actualice estas dos direcciones URL de imagen agregando `.debug` al final y guarde este archivo. 
2. Recompile la solución. En la paleta de comandos de VS Code, escriba y ejecute el comando **Azure IoT Edge: Build IoT Edge solution** (Azure IoT Edge: compilar solución IoT Edge).
3. En el Explorador de dispositivos de Azure IoT Hub, haga clic con el botón derecho en un identificador de dispositivo de IoT Edge y, a continuación, seleccione **Create deployment for Edge device** (Crear implementación para dispositivo Edge). Seleccione el archivo `deployment.json` en la carpeta `config`. A continuación, puede ver que la implementación se ha creado correctamente con un identificador de implementación en el terminal integrado de VS Code.

Puede comprobar el estado del contenedor en el Explorador de Docker de VS Code o mediante la ejecución del comando `docker ps` en el terminal.

### <a name="start-debugging-c-function-in-vs-code"></a>Inicio de la depuración de la función de C# en VS Code
1. VS Code sigue depurando la información de configuración en un archivo `launch.json` ubicado en una carpeta `.vscode` en el área de trabajo. Este archivo `launch.json` se ha generado al crear una nueva solución de IoT Edge. Se actualiza cada vez que agregue un nuevo módulo que admita la depuración. Vaya a la vista de depuración y seleccione el archivo de configuración de depuración correspondiente para la depuración remota del módulo de funciones C#.
2. Vaya a `run.csx`. Agregue un punto de interrupción en la función.
3. Haga clic en el botón **Iniciar depuración** o presione **F5** y seleccione el proceso que se adjuntará.
4. En la vista de depuración de VS Code, puede ver las variables en el panel izquierdo. 

### <a name="start-debugging-c-module-at-the-same-time-in-vs-code"></a>Inicio de la depuración del módulo de C# al mismo tiempo en VS Code
1. En la paleta de comandos de VS Code, escriba y ejecute el comando "Workspace: Duplicate Workspace in New Window" (Área de trabajo: duplicar área de trabajo en una nueva ventana). Se inicia una nueva ventana de VS Code con la misma área de trabajo.
2. Vaya a la vista de depuración y seleccione el archivo de configuración de depuración correspondiente para la depuración remota del módulo C#.
3. Vaya a `program.cs`. Agregue un punto de interrupción en módulo C#.
4. Haga clic en el botón **Iniciar depuración** o presione **F5** y seleccione el proceso que se adjuntará.
5. En la vista de depuración de VS Code, puede ver las variables en el panel izquierdo. 

### <a name="see-variables-in-multiple-debugging-windows"></a>Vista de las variables en varias ventanas de depuración
1. Ahora ya tiene al menos dos sesiones de depuración ejecutándose en dos ventanas de VS Code. Se debe alcanzar uno de los puntos de interrupción.
2. Presione `F10` o haga clic en el botón de paso a paso en la **barra de herramientas de depuración**.
3. Se alcanzará el punto de interrupción en otra ventana de VS Code. 
4. Continúe con los dos pasos anteriores, puede ver las variables de varios módulos en varias ventanas de depuración de VS Code.

> [!NOTE]
> El ejemplo anterior muestra cómo depurar varios módulos con Azure IoT Edge. Se basa en la versión de depuración de `Dockerfile.amd64.debug`, que incluye VSDBG (el depurador de línea de comandos de .NET Core) en la imagen de contenedor al realizar la compilación. Se recomienda utilizar directamente o personalizar `Dockerfile` sin VSDBG para funciones de IoT Edge listas para entornos de producción cuando termine de depurar las funciones de C#.

## <a name="next-steps"></a>Pasos siguientes

Una vez que tenga el módulo compilado, aprenda cómo realizar la [Implementación de módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md) 0
