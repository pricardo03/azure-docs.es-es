---
title: Depuración de módulos de C# para Azure IoT Edge | Microsoft Docs
description: Uso de Visual Studio Code para desarrollar, compilar y depurar un módulo de C# para Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/27/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: abfd65920348bd51a9923d0a7c74f0f980a01540
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567832"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Uso de Visual Studio Code para desarrollar y depurar módulos de C# para Azure IoT Edge

Puede convertir la lógica de negocios en módulos de Azure IoT Edge. En este artículo se muestra cómo usar Visual Studio Code (VS Code) como herramienta principal para desarrollar y depurar módulos de C#.

## <a name="prerequisites"></a>Requisitos previos

Puede usar un equipo o máquina virtual con Windows, macOS o Linux como equipo de desarrollo. Un dispositivo IoT Edge puede ser otro dispositivo físico.

Hay dos formas de depurar un módulo de C# en VS Code. Una es agregar un proceso a un contenedor de módulo y otra es iniciar el código del módulo en modo de depuración. Si no está familiarizado con las funcionalidades de depuración de Visual Studio Code, consulte más información sobre [depuración](https://code.visualstudio.com/Docs/editor/debugging).

En primer lugar, instale Visual Studio Code y, posteriormente, agregue las siguientes extensiones necesarias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Extensión de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para crear un módulo, necesita .NET para crear la carpeta del proyecto, Docker para compilar la imagen del módulo y un registro de contenedor para guardar esta:
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download).
* [Docker Community Edition](https://docs.docker.com/install/) en la máquina de desarrollo 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Puede usar un registro de Docker local con fines de prototipo y prueba en lugar de un registro en la nube. 

Para configurar el entorno local de desarrollo para depurar, ejecutar y probar la solución de IoT Edge, necesita la [herramienta de desarrollo de Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Instale [Python (2.7/3.6) y Pip](https://www.python.org/). A continuación, instale **iotedgehubdev** mediante la ejecución del siguiente comando en el terminal.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Para probar el módulo en un dispositivo, necesita un centro de IoT activo con al menos un identificador de dispositivo IoT Edge creado. Si ejecuta el demonio de IoT Edge en una máquina de desarrollo, es posible que deba detener EdgeHub y EdgeAgent antes de ir al paso siguiente. 

## <a name="create-a-new-solution-with-c-module"></a>Creación de una solución nueva con el módulo de C#

Realice estos pasos para crear un módulo IoT Edge basado en .NET Core 2.1 mediante Visual Studio Code y la extensión de Azure IoT Edge. En primer lugar, cree una solución y, luego, genere el primer módulo en esa solución. Cada solución puede contener más de un módulo. 

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado**.
2. Seleccione **Ver** > **Paleta de comandos**. 
3. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: nueva solución de IoT Edge).

   ![Ejecución de la nueva solución IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

4. Seleccione la carpeta en la que desea crear la solución. Elija **Select folder** (Seleccionar carpeta). 
5. Escriba un nombre para la solución. 
6. Seleccione **C# Module** como la plantilla del primer módulo de la solución.
7. Escriba un nombre para el módulo. Elija un nombre que sea único dentro del registro de contenedor. 
8. Proporcione el nombre del repositorio de imágenes del módulo. VS Code rellena automáticamente el nombre del módulo con **localhost:5000**. Reemplácelo por su propia información de registro. Si usa un registro de Docker local para realizar pruebas, **localhost** puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a **\<nombre del registro\>.azurecr.io**. Reemplace solo la parte de localhost de la cadena, no elimine el nombre del módulo.

   ![Especificación del repositorio de imágenes de Docker](./media/how-to-develop-csharp-module/repository.png)

VS Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana.

   ![Visualización de la solución IoT Edge](./media/how-to-develop-csharp-module/view-solution.png)

Hay cuatro elementos dentro de la solución: 

* Una carpeta **.vscode** que contiene configuraciones de depuración.
* Una carpeta **modules** que contiene subcarpetas para cada módulo. En este momento, solo tiene uno. Sin embargo, podría agregar más en la paleta de comandos con el comando **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: agregar módulo IoT Edge). 
* Un archivo **.env** enumera las variables de entorno. Si Azure Container Registry es su registro, tendrá en él un nombre de usuario y una contraseña de Azure Container Registry. 

   >[!NOTE]
   >Solo se crea el archivo de entorno si proporciona un repositorio de imágenes para el módulo. Si aceptó los valores predeterminados de localhost para probar y depurar localmente, no es necesario declarar las variables de entorno. 

* Un archivo **deployment.template.json** muestra el nuevo módulo junto con un módulo **tempSensor** de ejemplo que simula los datos que puede usar para las pruebas. Para más información sobre cómo funcionan los manifiestos de implementación, consulte [cómo usar los manifiestos de implementación para implementar módulos y establecer rutas](module-composition.md). 

## <a name="develop-your-module"></a>Desarrollo de su módulo

El código del módulo de C# predeterminado que se incluye con la solución se encuentra en **modules** > ** [nombre del módulo] ** > **Program.cs**. El módulo y el archivo deployment.template.json se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo para iniciar la prueba sin tocar el código. El módulo se compila para tomar simplemente la entrada de un origen (en este caso, el módulo tempSensor que simula datos) y canalizarlo a IoT Hub. 

Cuando esté listo para personalizar la plantilla de C# con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como seguridad, administración de dispositivos y confiabilidad. 

La compatibilidad de C# en VS Code está optimizada para el desarrollo multiplataforma de .NET Core. Obtenga más información sobre [cómo trabajar con C# en VS Code](https://code.visualstudio.com/docs/languages/csharp).

## <a name="launch-and-debug-module-code-without-container"></a>Inicio y depuración del código del módulo sin contenedor

El módulo de IoT Edge en C# es una aplicación .Net Core. Y depende del SDK de dispositivo C# IoT de Azure. Como el módulo IoT C# requiere que se inicie y ejecute la configuración del entorno, en el código del módulo predeterminado se inicializa un objeto **ModuleClient** con la configuración del entorno y el nombre de la entrada. También es preciso enviar o enrutar los mensajes a los canales de entrada. El módulo predeterminado de C# solo contiene un canal de entrada y se llama **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>Configuración del simulador de IoT Edge para la aplicación de módulo único

1. Para configurar e iniciar el simulador, en la paleta de comandos de VS Code, escriba y seleccione **Azure IoT Edge: Start IoT Edge Hub Simulator for Single Module** (Azure IoT Edge: Iniciar simulador de centro de IoT Edge para módulo único). También deberá especificar los nombres de entrada para la aplicación de módulo único, escriba **input1** y presione ENTRAR. El comando desencadenará la CLI con **iotedgehubdev** e iniciará el simulador de IoT Edge y un contenedor de módulo de herramienta para pruebas. Puede ver las siguientes salidas en el terminal integrado si el simulador se ha iniciado correctamente en modo de módulo único. También puede ver un comando `curl` para ayudar a enviar el mensaje. Lo usará más adelante.

   ![Configuración del simulador de IoT Edge para la aplicación de módulo único](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Puede ir al explorador de Docker y ver el estado de ejecución del módulo.

   ![Estado del módulo simulador](media/how-to-develop-csharp-module/simulator-status.png)

   El contenedor **edgeHubDev** es el núcleo del simulador de IoT Edge local. Puede ejecutarse en el equipo de desarrollo sin el demonio de seguridad de IoT Edge y proporcionar la configuración del entorno para la aplicación de módulo nativa o los contenedores de módulo. El contenedor de **entrada** expone las API de REST para ayudar a que los mensajes lleguen al canal de entrada de destino en el módulo.

2. En la paleta de comandos de VS Code, escriba y seleccione **Azure IoT Edge: Set Module Credentials to User Settings** (Azure IoT Edge: Establecer credenciales de módulo en la configuración de usuario) para establecer la configuración del entorno del módulo en `azure-iot-edge.EdgeHubConnectionString` y `azure-iot-edge.EdgeModuleCACertificateFile` en la configuración del usuario. Observe que se hace referencia a esta configuración de entorno en **.vscode** > **launch.json** y en la [configuración de usuario de VS Code](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="build-module-app-and-debug-in-launch-mode"></a>Compilación de la aplicación de módulo y depuración en modo de inicio

1. En el terminal integrado, cambie el directorio por la carpeta **CSharpModule** y ejecute el siguiente comando para compilar la aplicación .Net Core.

    ```cmd
    dotnet build
    ```

2. Vaya a `program.cs`. Agregue un punto de interrupción en este archivo.

3. Vaya a la vista de depuración de VS Code: Vista > Depuración. Seleccione la configuración de depuración **ModuleName Local Debug (.NET Core)** en la lista desplegable. 

  ![Entrar en modo de depuración en VS Code](media/how-to-develop-csharp-module/debug-view.png)

4. Haga clic en **Iniciar depuración** o presione **F5**. Se iniciará la sesión de depuración.

   > [!NOTE]
   > Si el elemento `TargetFramework` de .Net Core no es coherente con la ruta de acceso del programa en `launch.json`. Debe actualizar manualmente la ruta de acceso del programa en `launch.json` para respetar el `TargetFramework` en el archivo .csproj, con el fin de que VS Code pueda iniciar correctamente este programa.

5. En el terminal integrado de VS Code, ejecute el siguiente comando para enviar un mensaje de **Hola mundo** al módulo. Este es el comando que se mostró en pasos anteriores cuando se instaló correctamente el simulador de IoT Edge.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Si usa Windows, asegúrese de que el shell de su terminal integrado de VS Code es **Git Bash** o **WSL Bash**. Ejecute el comando `curl` en PowerShell o desde el símbolo del sistema. 
   
   > [!TIP]
   > También puede usar [PostMan](https://www.getpostman.com/) u otras herramientas de API para enviar mensajes en lugar de `curl`.

6. En la vista de depuración de VS Code, verá las variables en el panel izquierdo. 

    ![Inspección de variables](media/how-to-develop-csharp-module/single-module-variables.png)

7. Para detener la sesión de depuración, haga clic en el botón detener o presione **MAYÚS + F5**. En la paleta de comandos de VS Code, escriba y seleccione **Azure IoT Edge: Stop IoT Edge Simulator** (Azure IoT Edge: Detener el simulador de IoT Edge) para detener y limpiar el simulador.

## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Compilación del contenedor de módulo para la depuración y depuración en modo de agregación

La solución predeterminada contiene dos módulos, uno es un módulo de sensor de temperatura simulado y el otro es el módulo de canalización de C#. El sensor de temperatura simulado continúa mandando mensajes al módulo de canalización de C#, y dichos mensajes se canalizan a IoT Hub. En la carpeta de módulo que se creó, hay varios archivos de Docker para diferentes tipos de contenedor. Use cualquiera de estos archivos que acaban con la extensión **.debug** para compilar el módulo de pruebas. Actualmente, los módulos de C# solo admiten la depuración en contenedores de Linux amd64 en el modo de agregación.

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>Configuración del simulador de IoT Edge para la solución de IoT Edge

En la máquina de desarrollo, puede iniciar el simulador de IoT Edge en lugar de instalar el demonio de seguridad de IoT Edge para ejecutar la solución de IoT Edge. 

1. En el explorador de dispositivos del lado izquierdo, haga clic con el botón secundario en el identificador de dispositivo IoT Edge y seleccione **Setup IoT Edge Simulator** (Configurar el simulador de IoT Edge) para iniciar el simulador con la cadena de conexión del dispositivo.

2. Puede ver que el simulador de IoT Edge se ha configurado correctamente en el terminal integrado.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Compilación y ejecución del contenedor para la depuración y depuración en modo de agregación

1. En VS Code, vaya al archivo `deployment.template.json`. Para actualizar la dirección URL de la imagen del módulo de C#, agregue **.debug** al final.

   ![Adición de **.debug** al nombre de imagen](./media/how-to-develop-csharp-module/image-debug.png)

2. Vaya a `program.cs`. Agregue un punto de interrupción en este archivo.

3. En el explorador de archivos de VS Code, seleccione el archivo `deployment.template.json` para su solución y, en el menú contextual, haga clic en **Build and Run IoT Edge solution in Simulator** (Compilar y ejecutar la solución de IoT Edge en el simulador). Puede inspeccionar todos los registros del contenedor del módulo en la misma ventana. También puede navegar hasta el explorador de Docker para observar el estado del contenedor.

   ![Inspección de variables](media/how-to-develop-csharp-module/view-log.png)

4. Vaya a la vista de depuración de VS Code. Seleccione el archivo de configuración de depuración de su módulo. El nombre de la opción de depuración debe ser parecido a **ModuleName Remote Debug (.NET Core)**

   ![Seleccione Configuración](media/how-to-develop-csharp-module/debug-config.png)

5. Seleccione **Iniciar depuración** o presione **F5**. Seleccione el proceso al que desea asociarlo.

6. En la vista de depuración de VS Code, verá las variables en el panel izquierdo.

7. Para detener la sesión de depuración, haga clic en el botón detener o presione **MAYÚS + F5**. En la paleta de comandos de VS Code, escriba y seleccione **Azure IoT Edge: Stop IoT Edge Simulator** (Azure IoT Edge: Detener el simulador de IoT Edge).

    > [!NOTE]
    > En el ejemplo anterior se muestra cómo depurar módulos de IoT Edge para .NET Core en contenedores. Se basa en la versión de depuración de `Dockerfile.debug`, que incluye VSDBG (el depurador de línea de comandos de Visual Studio .NET Core) en la imagen de contenedor al realizar la compilación. Una vez finalizada la depuración de los módulos de C#, se recomienda usar directamente o personalizar `Dockerfile` sin VSDBG para módulos de IoT Edge listos para producción.


## <a name="next-steps"></a>Pasos siguientes

Después de compilar el módulo, aprenda cómo [implementar módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md).

Para desarrollar módulos para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
