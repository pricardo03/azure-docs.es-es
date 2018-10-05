---
title: Depuración de módulos de Node.js para Azure IoT Edge | Microsoft Docs
description: Uso de Visual Studio Code para desarrollar y depurar módulos de Node.js para Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/21/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a1459e3cbd433e2997ffd822b961ac781a72ca90
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423534"
---
# <a name="use-visual-studio-code-to-develop-and-debug-nodejs-modules-for-azure-iot-edge"></a>Uso de Visual Studio Code para desarrollar y depurar módulos de Node.js para Azure IoT Edge

Puede enviar la lógica de negocios para que funcione en el perímetro, convirtiéndola en módulos de Azure IoT Edge. En este artículo se proporcionan instrucciones detalladas para usar Visual Studio Code (VS Code) como la herramienta de desarrollo principal para desarrollar módulos de Node.js.

## <a name="prerequisites"></a>Requisitos previos
En este artículo se da por hecho que usa un equipo o máquina virtual que ejecuta Windows, macOS o Linux como máquina de desarrollo. El dispositivo IoT Edge puede ser otro dispositivo físico.

> [!NOTE]
> En este artículo de depuración se muestran dos formas habituales para depurar el módulo de Node.js en VS Code. Una es agregar un proceso a un contenedor de módulo, mientras que la otra es iniciar el código del módulo en modo de depuración. Si no está familiarizado con las funcionalidades de depuración de Visual Studio Code, consulte más información sobre [depuración](https://code.visualstudio.com/Docs/editor/debugging).

Como en este artículo se usa Visual Studio Code como la principal herramienta de desarrollo, instale VS Code y, a continuación, agregue las extensiones necesarias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensión de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para crear un módulo, necesita una instancia de Node.js que incluya npm para crear la carpeta del proyecto, Docker para compilar la imagen del módulo y un registro de contenedor para guardar esta:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Puede usar un registro de Docker local para fines de prototipo y pruebas en lugar de un registro en la nube. 

Para configurar el entorno local de desarrollo para depurar, ejecutar y probar la solución de IoT Edge, necesita la [herramienta de desarrollo de Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Instale [Python (2.7/3.6) y Pip](https://www.python.org/). PIP se incluye con el instalador de Python. A continuación, instale **iotedgehubdev** mediante la ejecución del siguiente comando en el terminal.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Para probar el módulo en un dispositivo, necesita un centro de IoT activo con al menos un identificador de dispositivo IoT Edge creado. Si ejecuta el demonio de IoT Edge en una máquina de desarrollo, es posible que deba detener EdgeHub y EdgeAgent antes de ir al paso siguiente. 

## <a name="create-a-new-solution-template"></a>Creación de una plantilla de solución

En los siguientes pasos puede ver cómo crear un módulo de IoT Edge basado en Node.js mediante Visual Studio Code y la extensión de Azure IoT Edge. Primero debe crear una solución y, a continuación, generar el primer módulo de esa solución. Cada solución puede contener varios módulos. 

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
9. Especifique el repositorio de imágenes del módulo. VS Code rellena automáticamente el nombre del módulo, por lo que solo tiene que reemplazar **localhost:5000** por la información de su propio registro. Si usa un registro de Docker local para realizar pruebas, localhost puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a **\<nombre del registro\>.azurecr.io**. Reemplace solo la parte de localhost de la cadena, no elimine el nombre del módulo.

   ![Especificación del repositorio de imágenes de Docker](./media/how-to-develop-node-module/repository.png)

VS Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana.

En la solución tiene tres elementos: 
* Una carpeta **.vscode** que contiene configuraciones de depuración.
* Una carpeta **modules** que contiene subcarpetas para cada módulo. En la actualidad solo tiene uno, pero podría agregar más en la paleta de comandos con el comando **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: agregar módulo IoT Edge). 
* Un archivo **.env** enumera las variables de entorno. Si Azure Container Registry es su registro, tendrá en él un nombre de usuario y una contraseña de Azure Container Registry.

   >[!NOTE]
   >Solo se crea el archivo de entorno si proporciona un repositorio de imágenes para el módulo. Si aceptó los valores predeterminados de localhost para probar y depurar localmente, no es necesario declarar las variables de entorno. 

* Un archivo **deployment.template.json** muestra el nuevo módulo junto con un módulo **tempSensor** de ejemplo que simula los datos que puede usar para realizar pruebas. Para más información sobre el funcionamiento de los manifiestos de implementación, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

## <a name="develop-your-module"></a>Desarrollo de su módulo

El código de Node.js predeterminado que se incluye con la solución se encuentra en **módulos** > [nombre del módulo] > **app.js**. El módulo y el archivo deployment.template.json se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo para iniciar la prueba sin tocar el código. El módulo se compila para tomar simplemente la entrada de un origen (en este caso, el módulo tempSensor que simula datos) y canalizarlo a IoT Hub. 

Cuando esté listo para personalizar la plantilla de Node.js con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como seguridad, administración de dispositivos y confiabilidad. 

Visual Studio Code es compatible con Node.js. Obtenga más información sobre [cómo trabajar con Node.js en VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="launch-and-debug-module-code-without-container"></a>Inicio y depuración del código del módulo sin contenedor

El módulo Node.js de IoT Edge depende del SDK de dispositivo de Node.js de Azure IoT. En el código del módulo predeterminado, se inicializa un **ModuleClient** con la configuración del entorno y el nombre de entrada, lo que significa que el módulo de IoT Edge Node.js requiere iniciar y ejecutar la configuración del entorno y que se envíen o enruten mensajes a los canales de entrada. El módulo predeterminado de Node.js solo contiene un canal de entrada y se llama **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>Configuración del simulador de IoT Edge para la aplicación de módulo único

1. Para configurar e iniciar el simulador, en la paleta de comandos de VS Code, escriba y seleccione **Azure IoT Edge: Start IoT Edge Hub Simulator for Single Module** (Azure IoT Edge: Iniciar simulador de centro de IoT Edge para módulo único). También deberá especificar los nombres de entrada para la aplicación de módulo único, escriba **input1** y presione ENTRAR. El comando desencadenará la CLI con **iotedgehubdev** e iniciará el simulador de IoT Edge y un contenedor de módulo de herramienta para pruebas. Puede ver las siguientes salidas en el terminal integrado si el simulador se ha iniciado correctamente en modo de módulo único. También puede ver un comando `curl` para ayudar a enviar el mensaje. Lo usará más adelante.

   ![Configuración del simulador de IoT Edge para la aplicación de módulo único](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Puede ir al explorador de Docker y ver el estado de ejecución del módulo.

   ![Estado del módulo simulador](media/how-to-develop-csharp-module/simulator-status.png)

   El contenedor **edgeHubDev** es el núcleo del simulador de IoT Edge local. Puede ejecutarse en el equipo de desarrollo sin el demonio de seguridad de IoT Edge y proporcionar la configuración del entorno para la aplicación de módulo nativa o los contenedores de módulo. El contenedor de **entrada** expone las API de REST para ayudar a que los mensajes lleguen al canal de entrada de destino en el módulo.

2. En la paleta de comandos de VS Code, escriba y seleccione **Azure IoT Edge: Set Module Credentials to User Settings** (Azure IoT Edge: Establecer credenciales de módulo en la configuración de usuario) para establecer la configuración del entorno del módulo en `azure-iot-edge.EdgeHubConnectionString` y `azure-iot-edge.EdgeModuleCACertificateFile` en la configuración del usuario. Observe que se hace referencia a esta configuración de entorno en **.vscode** > **launch.json** y en la [configuración de usuario de VS Code](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="debug-nodejs-module-in-launch-mode"></a>Depuración del módulo Node.js en el modo de inicio

1. En el terminal integrado, cambie el directorio por la carpeta **NodeModule** y ejecute el siguiente comando para instalar paquetes Node.

   ```cmd
   npm install
   ```

2. Vaya a `app.js`. Agregue un punto de interrupción en este archivo.

3. Vaya a la vista de depuración de VS Code. Seleccione la configuración de depuración **ModuleName Local Debug (Node.js)**. 

4. Haga clic en **Iniciar depuración** o presione **F5**. Se iniciará la sesión de depuración.

5. En el terminal integrado de VS Code, ejecute el siguiente comando para enviar un mensaje de **Hola mundo** al módulo. Este es el comando que se mostró en pasos anteriores cuando se instaló correctamente el simulador de IoT Edge. Es posible que tenga que crear o cambiar a otro terminal integrado si el actual está bloqueado.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Si usa Windows, asegúrese de que el shell de su terminal integrado de VS Code es **Git Bash** o **WSL Bash**. Ejecute el comando `curl` en PowerShell o desde el símbolo del sistema. 
   
   > [!TIP]
   > También puede usar [PostMan](https://www.getpostman.com/) u otras herramientas de API para enviar mensajes en lugar de `curl`.

6. En la vista de depuración de VS Code, verá las variables en el panel izquierdo. 

7. Para detener la sesión de depuración, haga clic en el botón detener o presione **MAYÚS + F5**. En la paleta de comandos de VS Code, escriba y seleccione **Azure IoT Edge: Stop IoT Edge Simulator** (Azure IoT Edge: Detener el simulador de IoT Edge) para detener y limpiar el simulador.


## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Compilación del contenedor de módulo para la depuración y depuración en modo de agregación

La solución predeterminada contiene dos módulos, uno es un módulo de sensor de temperatura simulado y el otro es el módulo de canalización de Node.js. El sensor de temperatura simulado continúa mandando mensajes al módulo de canalización de Node.js, y después los mensajes se canalizan a IoT Hub. En la carpeta de módulo que se creó, hay varios archivos de Docker para diferentes tipos de contenedor. Use cualquiera de estos archivos que acaban con la extensión **.debug** para compilar el módulo de pruebas. Actualmente, los módulos de Node.js solo admiten la depuración en contenedores de linux-amd64, windows-amd64 y linux-arm32v7.

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>Configuración del simulador de IoT Edge para la solución de IoT Edge

En la máquina de desarrollo, puede iniciar el simulador de IoT Edge en lugar de instalar el demonio de seguridad de IoT Edge para ejecutar la solución de IoT Edge. 

1. En el explorador de dispositivos del lado izquierdo, haga clic con el botón secundario en el identificador de dispositivo IoT Edge y seleccione **Setup IoT Edge Simulator** (Configurar el simulador de IoT Edge) para iniciar el simulador con la cadena de conexión del dispositivo.

2. Puede ver que el simulador de IoT Edge se ha configurado correctamente en el terminal integrado.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Compilación y ejecución del contenedor para la depuración y depuración en modo de agregación

1. En VS Code, vaya al archivo `deployment.template.json`. Para actualizar la dirección URL de la imagen del módulo, agregue **.debug** al final.

2. Reemplace el módulo de createOptions de Node.js en el archivo **deployment.template.json** por el siguiente contenido y guarde este archivo: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

3. Vaya a la vista de depuración de VS Code. Seleccione el archivo de configuración de depuración de su módulo. El nombre de la opción de depuración debe ser similar a **ModuleName Remote Debug (Node.js)** o **ModuleName Remote Debug (Node.js en el contenedor de Windows)**, que depende del tipo de contenedor en la máquina de desarrollo.

4. Seleccione **Iniciar depuración** o presione **F5**. Seleccione el proceso al que desea asociarlo.

5. En la vista de depuración de VS Code, verá las variables en el panel izquierdo.

6. Para detener la sesión de depuración, haga clic en el botón detener o presione **MAYÚS + F5**. En la paleta de comandos de VS Code, escriba y seleccione **Azure IoT Edge: Stop IoT Edge Simulator** (Azure IoT Edge: Detener el simulador de IoT Edge).

> [!NOTE]
> En el ejemplo anterior se muestra cómo depurar módulos de IoT Edge en Node.js en contenedores. Agregó puertos expuestos en el contenedor del módulo createOptions. Una vez finalizada la depuración de los módulos de Node.js, se recomienda quitar estos puertos expuestos de los módulos de IoT Edge listos para entornos de producción.

## <a name="next-steps"></a>Pasos siguientes

Una vez que tenga el módulo compilado, aprenda a [implementar módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md)

Para desarrollar módulos para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
