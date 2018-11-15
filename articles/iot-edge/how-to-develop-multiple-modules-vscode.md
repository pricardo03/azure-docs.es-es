---
title: Trabajar con varios módulos de Azure IoT Edge en VS Code | Microsoft Docs
description: Use la extensión de IoT para Visual Studio Code, y así desarrollar a la vez varios módulos para Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: d895e10c6b667a05156b2ce0f83f7776b9ba4f0a
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568767"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code"></a>Desarrollo de una solución de IoT Edge con varios módulos en Visual Studio Code

Puede utilizar Visual Studio Code para desarrollar su solución de Azure IoT Edge con varios módulos. En este artículo se muestra cómo crear, actualizar e implementar una solución de IoT Edge que canalice los datos del sensor de un dispositivo simulado de IoT Edge en VS Code. 

## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de este artículo, debe satisfacer los siguientes requisitos previos:

- [Visual Studio Code](https://code.visualstudio.com/)
- La [extensión de Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- La [extensión de C# para Visual Studio Code (con tecnología de OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- El [SDK de .NET Core 2.1](https://www.microsoft.com/net/download)
- Una instancia de IoT Hub activa con al menos un dispositivo de IoT Edge

También necesita [Docker para VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) con la integración de Device Explorer de Azure IoT Hub para administrar imágenes y contenedores.

## <a name="create-your-iot-edge-solution"></a>Cree su propia solución de IoT Edge

1. En Visual Studio Code, abra el terminal integrado; para ello, seleccione **Ver** > **Terminal integrado**. 

1. En la **paleta de comandos** de VS Code, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge). Seleccione la carpeta del área de trabajo y proporcione el nombre de la solución (el valor predeterminado es EdgeSolution). Cree un módulo de C# (con el nombre **PipeModule**) como el primer módulo de usuario de esta solución. La plantilla predeterminada del módulo de C# es un módulo de canalización, que canaliza directamente los mensajes desde el nivel ascendente al nivel descendente. También debe especificar el repositorio de imágenes de Docker para el primer módulo. El repositorio de imágenes predeterminado se basa en un registro de Docker local (**localhost:5000/<first module name>**). Puede cambiarlo a Azure Container Registry o Docker Hub. 

2. El área de trabajo de la solución de IoT Edge se carga en la ventana de Visual Studio Code. La carpeta raíz contiene una carpeta de **módulos**, una carpeta **.vscode** y un archivo de plantilla de manifiesto de implementación. Las configuraciones de depuración se encuentran en la carpeta .vscode. Todos los códigos de módulo del usuario son las subcarpetas de la carpeta de módulos. El archivo deployment.template.json es la plantilla de manifiesto de implementación. Algunos de los parámetros de este archivo se analizan desde el archivo module.json, que existe en cada carpeta del módulo.

3. Agregue el segundo módulo a este proyecto de la solución. Hay varias maneras de agregar un nuevo módulo a la solución actual. Escriba y ejecute el comando **Azure IoT Edge: Add IoT Edge module** (Azure IoT Edge: agregar el módulo IoT Edge). Seleccione el archivo de plantilla de implementación que va a actualizar. También puede hacer clic con el botón derecho en la carpeta de módulos, o hacer clic con el botón derecho en el archivo deployment.template.json y seleccionar **Add IoT Edge Module** (Agregar el módulo de IoT Edge). A continuación, habrá una lista desplegable para seleccionar el tipo de módulo. Seleccione un módulo de **Azure Functions - C#** con el nombre **PipeFunction** y su repositorio de imágenes de Docker. La plantilla predeterminada del módulo de funciones de C# es un módulo de canalización, que canaliza directamente los mensajes desde el nivel ascendente al nivel descendente.

4. Abra el archivo deployment.template.json. Compruebe que el archivo declara tres módulos y el runtime. El mensaje se genera desde el módulo tempSensor. El mensaje se canaliza directamente a través de los módulos SampleModule y SampleFunction y, a continuación, se envía a IoT Hub. 

5. Actualice las rutas a estos módulos con el contenido siguiente:

   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/PipeModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/PipeModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/PipeFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/PipeFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Guarde este archivo.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Compilación e implementación de la solución de IoT Edge

1. En la **paleta de comandos** de VS Code, escriba y ejecute el comando **Azure IoT Edge: Build IoT Edge solution** (Azure IoT Edge: compilar solución IoT Edge). Este comando toma como base el archivo module.json de cada carpeta del módulo, y empieza a compilar, incluir en contenedores e insertar cada imagen de Docker del módulo. A continuación, el comando pasa el valor necesario al archivo deployment.template.json y genera el archivo deployment.json con información de la carpeta config. Puede ver el progreso de compilación en el terminal integrado de VS Code. 

2. En Azure IoT Hub **Device Explorer**, haga clic con el botón derecho en un identificador de dispositivo de IoT Edge y, a continuación, seleccione el comando **Create deployment for Edge device**. Seleccione el archivo deployment.json en la carpeta config. A continuación, puede ver que la implementación se ha creado correctamente con un identificador de implementación en el terminal integrado de VS Code.

3. Si va a simular un dispositivo de IoT Edge en la máquina de desarrollo, observará que todos los contenedores de imágenes del módulo se inician en unos minutos.

## <a name="view-the-generated-data"></a>Visualización de los datos generados

1. Para supervisar los datos que llegan a la instancia de IoT Hub, seleccione **Ver** > **Paleta de comandos**. Después, seleccione el comando **IoT: Start monitoring D2C message**. 
2. Para dejar de supervisar datos, use el comando **IoT: Stop monitoring D2C message** de la **paleta de comandos**. 

## <a name="next-steps"></a>Pasos siguientes

Conozca otros escenarios de desarrollo con Azure IoT Edge en Visual Studio Code:

* Desarrolle módulos en VS Code con [C#](how-to-develop-csharp-module.md) o [Node.js](how-to-develop-node-module.md).
* Desarrolle Azure Functions en VS Code con [C#](how-to-develop-csharp-function.md).

Para desarrollar módulos para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).