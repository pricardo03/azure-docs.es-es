---
title: Depuración de módulos de Node.js para Azure IoT Edge | Microsoft Docs
description: Uso de Visual Studio Code para desarrollar y depurar módulos de Node.js para Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 8032fd2a0150597c55178648511c80233e63a911
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054733"
---
# <a name="develop-and-debug-nodejs-modules-with-azure-iot-edge-for-visual-studio-code"></a>Desarrollo y depuración de módulos de Node.js con Azure IoT Edge para Visual Studio Code

Puede enviar la lógica de negocios para que funcione en el perímetro, convirtiéndola en módulos de Azure IoT Edge. En este artículo se proporcionan instrucciones detalladas para usar Visual Studio Code (VS Code) como la herramienta de desarrollo principal para desarrollar módulos de C#.

## <a name="prerequisites"></a>Requisitos previos
En este artículo se supone que utiliza un equipo o una máquina virtual con Windows o Linux como máquina de desarrollo. El dispositivo de IoT Edge puede ser otro dispositivo físico o una simulación de dicho dispositivo en el equipo de desarrollo.

> [!NOTE]
> En este tutorial de depuración se describe cómo adjuntar un proceso en un contenedor de módulos y depurarlo con VS Code. Puede depurar módulos de Node.js en contenedores de Linux-amd64, Windows y Linux Arm32. Si no está familiarizado con las funcionalidades de depuración de Visual Studio Code, consulte más información sobre [depuración](https://code.visualstudio.com/Docs/editor/debugging). 

Como en este artículo se usa Visual Studio Code como la principal herramienta de desarrollo, instale VS Code y, a continuación, agregue las extensiones necesarias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensión de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para crear un módulo, necesita una instancia de Node.js que incluya npm para crear la carpeta del proyecto, Docker para compilar la imagen del módulo y un registro de contenedor para guardar esta:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   >[!TIP]
   >Puede usar un registro de Docker local para fines de prototipo y pruebas en lugar de un registro en la nube. 

Para probar el módulo en un dispositivo, necesita una instancia de IoT Hub con al menos un dispositivo IoT Edge. Si desea utilizar el equipo como un dispositivo IoT Edge, puede hacerlo siguiendo los pasos descritos en los tutoriales para [Windows](quickstart.md) o [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Creación de una plantilla de solución

En los siguientes pasos puede ver cómo crear un módulo IoT Edge basado en .NET Core 2.0 mediante Visual Studio Code y la extensión de Azure IoT Edge. Primero debe crear una solución y, a continuación, generar el primer módulo de esa solución. Cada solución puede contener varios módulos. 

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado**.
2. En el terminal integrado, escriba el siguiente comando para instalar (o actualizar) la versión más reciente de la plantilla de módulo de Azure IoT Edge para Node.js:

   ```cmd/sh
   npm install -g yo generator-azure-iot-edge-module
   ```
3. En el Visual Studio Code, seleccione **Ver** > **Paleta de comandos**. 
4. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución IoT Edge).

   ![Ejecución de la nueva solución IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Vaya a la carpeta en la que desea crear la nueva solución y haga clic en **Seleccionar carpeta**. 
6. Proporcione un nombre para la solución. 
7. Elija **Node.js Module** como la plantilla del primer módulo de la solución.
8. Especifique un nombre para el módulo. Elija un nombre que sea único dentro del registro de contenedor. 
9. Especifique el repositorio de imágenes del módulo. VS Code rellena automáticamente el nombre del módulo, por lo que solo tiene que reemplazar **localhost:5000** por la información de su propio registro. Si usa un registro de Docker local para realizar pruebas, localhost puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a **\<nombre del registro\>.azurecr.io**.

VS Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana.

En la solución tiene tres elementos: 
* Una carpeta **.vscode** que contiene configuraciones de depuración.
* Una carpeta **modules** que contiene subcarpetas para cada módulo. En la actualidad solo tiene uno, pero podría agregar más en la paleta de comandos con el comando **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: agregar módulo IoT Edge). 
* Un archivo **.env** enumera las variables de entorno. Si dispone de ACR como registro, en este momento ya tiene el nombre de usuario y la contraseña de ACR en este. 

   >[!NOTE]
   >Solo se crea el archivo de entorno si proporciona un repositorio de imágenes para el módulo. Si aceptó los valores predeterminados de localhost para probar y depurar localmente, no es necesario declarar las variables de entorno. 

* Un archivo **deployment.template.json** muestra el nuevo módulo junto con un módulo **tempSensor** de ejemplo que simula los datos que puede usar para realizar pruebas. Para más información sobre el funcionamiento de los manifiestos de implementación, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

## <a name="devlop-your-module"></a>Desarrollo del módulo

El código de función de Azure predeterminado que se incluye con la solución se encuentra en **modules** > **\<nombre del módulo\>** > **app.js**. El módulo y el archivo deployment.template.json se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo para iniciar la prueba sin tocar el código. El módulo se compila para tomar simplemente la entrada de un origen (en este caso, el módulo tempSensor que simula datos) y canalizarlo a IoT Hub. 

Cuando esté listo para personalizar la plantilla de Node.js con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como seguridad, administración de dispositivos y confiabilidad. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Compilación e implementación del módulo para depuración

En cada carpeta del módulo, hay varios archivos de Docker para diferentes tipos de contenedor. Puede usar cualquiera de estos archivos que acaban con la extensión **.debug** para compilar el módulo de pruebas. Actualmente, los módulos de C# solo admiten la depuración en contenedores de Linux amd64.

1. En VS Code, vaya al archivo `deployment.template.json`. Reemplace el módulo de createOptions de Node.js en el archivo **deployment.template.json** por el siguiente contenido y guarde este archivo: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

2. En la paleta de comandos de VS Code, escriba y ejecute el comando **Azure IoT Edge: Build IoT Edge solution** (Azure IoT Edge: compilar solución IoT Edge).
3. Seleccione el archivo `deployment.template.json` para la solución en la paleta de comandos. 
4. En el Explorador de dispositivos de Azure IoT Hub, haga clic con el botón derecho en un identificador de dispositivo IoT Edge y, a continuación, seleccione **Create deployment for IoT Edge device** (Crear implementación para dispositivo IoT Edge). 
5. Abra la carpeta **config** de la solución y, a continuación, seleccione el archivo `deployment.json`. Haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge). 

A continuación, puede ver que la implementación se ha creado correctamente con un identificador de implementación en el terminal integrado de VS Code.

Puede comprobar el estado del contenedor en el Explorador de Docker de VS Code o ejecutando el comando `docker ps` en el terminal.

## <a name="start-debugging-nodejs-module-in-vs-code"></a>Inicio de la depuración del módulo de Node.js en VS Code

VS Code sigue depurando la información de configuración en un archivo `launch.json` ubicado en una carpeta `.vscode` en el área de trabajo. Este archivo `launch.json` se ha generado al crear una nueva solución de IoT Edge. Se actualiza cada vez que agregue un nuevo módulo que admita la depuración. 

1. Vaya a la vista de depuración de VS Code y seleccione el archivo de configuración de depuración del módulo.

2. Vaya a `app.js`. Agregue un punto de interrupción en este archivo.

3. Haga clic en el botón **Iniciar depuración** o presione **F5** y seleccione el proceso que se adjuntará.

4. En la vista de depuración de VS Code, puede ver las variables en el panel izquierdo. 

En el ejemplo anterior se muestra cómo depurar módulos de IoT Edge en Node.js en contenedores. Agregó puertos expuestos en el contenedor del módulo createOptions. Una vez finalizada la depuración de los módulos de Node.js, se recomienda quitar estos puertos expuestos de los módulos de IoT Edge listos para entornos de producción.

## <a name="next-steps"></a>Pasos siguientes

Una vez que tenga el módulo compilado, aprenda a [implementar módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md)

Para desarrollar módulos para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
