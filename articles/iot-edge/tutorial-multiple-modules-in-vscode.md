---
title: Administración de varios módulos de Azure IoT Edge en VS Code | Microsoft Docs
description: Use Visual Studio Code para desarrollar soluciones de Azure IoT Edge que usan varios módulos.
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 4a624994f93e7a3cbb04db2a0ec0b2b823a12ee7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763041"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code-preview"></a>Desarrollo de una solución de IoT Edge con varios módulos en Visual Studio Code (versión preliminar)

Puede utilizar Visual Studio Code para desarrollar su solución de Azure IoT Edge con varios módulos. En este artículo se muestra cómo crear, actualizar e implementar una solución de IoT Edge que canalice los datos del sensor de un dispositivo simulado de IoT Edge en VS Code. 

## <a name="prerequisites"></a>requisitos previos

Para completar los pasos de este artículo, debe satisfacer los siguientes requisitos previos:

- [Visual Studio Code](https://code.visualstudio.com/)
- La [extensión de Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- La [extensión de C# para Visual Studio Code (con tecnología de OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- El [SDK de .NET Core 2.0](https://www.microsoft.com/net/core#windowscmd)
- La plantilla AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Una instancia de IoT Hub activa con al menos un dispositivo de IoT Edge

También necesita [Docker para VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) con la integración de Device Explorer de Azure IoT Hub para administrar imágenes y contenedores.

## <a name="prepare-your-first-iot-edge-solution"></a>Preparación de su primera solución de IoT Edge

1. En la **paleta de comandos** de VS Code, escriba y ejecute el comando **Edge: New IoT Edge solution**. Seleccione la carpeta del área de trabajo y proporcione el nombre de la solución (el valor predeterminado es EdgeSolution). Cree un módulo de C# (con el nombre **SampleModule**) como el primer módulo de usuario de esta solución. También debe especificar el repositorio de imágenes de Docker para el primer módulo. El repositorio de imágenes predeterminado se basa en un registro de Docker local (**localhost:5000/<first module name>**). Puede cambiarlo a Azure Container Registry o Docker Hub.

   > [!NOTE]
   > Si usa un registro de Docker local, asegúrese de que el registro se está ejecutando. En la ventana de la consola, escriba el siguiente comando:
   > 
   > `docker run -d -p 5000:5000 --restart=always --name registry registry:2`

2. La ventana de Visual Studio Code carga el área de trabajo de la solución de IoT Edge. La carpeta raíz contiene una carpeta de **módulos**, una carpeta **.vscode** y un archivo de plantilla de manifiesto de implementación. Las configuraciones de depuración se encuentran en la carpeta .vscode. Todos los códigos de módulo del usuario son las subcarpetas de la carpeta de módulos. El archivo deployment.template.json es la plantilla de manifiesto de implementación. Algunos de los parámetros de este archivo se analizan desde el archivo module.json, que existe en cada carpeta del módulo.

3. Agregue el segundo módulo a este proyecto de la solución. Escriba y ejecute el comando **Edge: Add IoT Edge module**. Seleccione el archivo de plantilla de implementación que va a actualizar. Seleccione un módulo de **Azure Function - C#** con el nombre **SampleFunction** y su repositorio de imágenes de Docker.

4. Abra el archivo deployment.template.json. Compruebe que el archivo declara tres módulos y el runtime. El mensaje se genera desde el módulo tempSensor. El mensaje se canaliza directamente a través de los módulos SampleModule y SampleFunction y, a continuación, se envía a IoT Hub. 

5. Actualice las rutas a estos módulos con el contenido siguiente:

   ```json
   "routes": {
      "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
      "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
      "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
   },
   ```

6. Guarde este archivo.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Compilación e implementación de la solución de IoT Edge

1. En la **paleta de comandos** de VS Code, escriba y ejecute el comando **Edge: Build IoT Edge solution**. Este comando toma como base el archivo module.json de cada carpeta del módulo, y empieza a compilar, incluir en contenedores e insertar cada imagen de Docker del módulo. A continuación, el comando pasa el valor necesario al archivo deployment.template.json y genera el archivo deployment.json con información de la carpeta config. Puede ver el progreso de compilación en el terminal integrado de VS Code.

2. En Azure IoT Hub **Device Explorer**, haga clic con el botón derecho en un identificador de dispositivo de IoT Edge y, a continuación, seleccione el comando **Create deployment for Edge device**. Seleccione el archivo deployment.json en la carpeta config. A continuación, puede ver que la implementación se ha creado correctamente con un identificador de implementación en el terminal integrado de VS Code.

3. Si va a simular un dispositivo de IoT Edge en la máquina de desarrollo, observará que todos los contenedores de imágenes de módulo se inician en unos minutos.

## <a name="view-the-generated-data"></a>Visualización de los datos generados

1. Para supervisar los datos que llegan a la instancia de IoT Hub, seleccione **Ver** > **Paleta de comandos**. Después, seleccione el comando **IoT: Start monitoring D2C message**. 
2. Para dejar de supervisar datos, use el comando **IoT: Stop monitoring D2C message** de la **paleta de comandos**. 

## <a name="next-steps"></a>Pasos siguientes

Conozca otros escenarios de desarrollo con Azure IoT Edge en Visual Studio Code:

* [Depuración de un módulo de C# en VS Code](how-to-vscode-debug-csharp-module.md)
* [Depuración de una función de C# en VS Code](how-to-vscode-debug-azure-function.md)
