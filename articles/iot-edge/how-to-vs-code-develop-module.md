---
title: Desarrollo y depuración de módulos para Azure IoT Edge | Microsoft Docs
description: Uso de Visual Studio Code para desarrollar, compilar y depurar un módulo de Azure IoT Edge mediante C#, Python, Node.js, Java o C
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 463ab617051bf97bb3b1c38ed431c4b6936a9c90
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118700"
---
# <a name="use-visual-studio-code-to-develop-and-debug-modules-for-azure-iot-edge"></a>Uso de Visual Studio Code para desarrollar y depurar módulos para Azure IoT Edge

Puede convertir la lógica de negocios en módulos de Azure IoT Edge. En este artículo se muestra cómo usar Visual Studio Code como herramienta principal para desarrollar y depurar módulos.

## <a name="prerequisites"></a>Requisitos previos

Puede usar un equipo o máquina virtual con Windows, macOS o Linux como equipo de desarrollo. Un dispositivo IoT Edge puede ser otro dispositivo físico.

Para los módulos escritos en C#, Node.js o Java, hay dos maneras de depurar el módulo en Visual Studio Code: Puede agregar un proceso a un contenedor de módulo o iniciar el código del módulo en modo de depuración. En el caso de los módulos escritos en Python o C, solo se pueden depurar mediante la asociación a un proceso en contenedores de Linux amd64.

> [!TIP]
> Si no está familiarizado con las funcionalidades de depuración de Visual Studio Code, consulte más información sobre [depuración](https://code.visualstudio.com/Docs/editor/debugging).

Instale [Visual Studio Code](https://code.visualstudio.com/) y después agregue las siguientes extensiones:

- [Herramientas de Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
- [Extensión de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- Extensiones de Visual Studio específicas del lenguaje en el que esté desarrollando:
  - C#, incluyendo Azure Functions: [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  - Python: [Extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - Java: [Paquete de extensiones de Java para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
  - C: [Extensión de C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

También deberá instalar algunas herramientas específicas del lenguaje adicionales para desarrollar el módulo:

- C#, incluido Azure Functions: [SDK de .NET Core 2.1](https://www.microsoft.com/net/download)

- Python: [Python](https://www.python.org/downloads/) y [Pip](https://pip.pypa.io/en/stable/installing/#installation) para instalar paquetes de Python (normalmente, se incluye con la instalación de Python). Una vez que Pip está instalado, instale el paquete **Cookiecutter** con el siguiente comando:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

- Node.js: [Node.js](https://nodejs.org). También le interesará instalar [Yeoman](https://www.npmjs.com/package/yo) y el [generador de módulo Node.js de Azure IoT Edge](https://www.npmjs.com/package/generator-azure-iot-edge-module).

- Java: [Java SE Development Kit 10](https://aka.ms/azure-jdks) y [Maven](https://maven.apache.org/). Deberá [establecer la `JAVA_HOME`variable de entorno](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) para que apunte a la instalación de JDK.

Para compilar e implementar la imagen del módulo, necesita Docker para compilar la imagen del módulo y un registro de contenedor para guardarla:

- [Docker Community Edition](https://docs.docker.com/install/) en la máquina de desarrollo

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

    > [!TIP]
    > Puede usar un registro de Docker local con fines de prototipo y prueba en lugar de un registro en la nube.

A menos que esté desarrollando el módulo en C, también necesitará la [herramienta de desarrollo de Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/) basada en Python con el fin de configurar el entorno de desarrollo local para depurar, ejecutar y probar la solución de IoT Edge. Si aún no lo ha hecho, instale [Python (2.7 o 3.6) y Pip](https://www.python.org/), y después **iotedgehubdev** mediante la ejecución de este comando en el terminal.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

> [!NOTE]
> Para probar el módulo en un dispositivo, necesitará una instancia de IoT Hub con al menos un dispositivo IoT Edge. Para usar el equipo como un dispositivo IoT Edge, siga los pasos descritos en el inicio rápido para [Linux](quickstart-linux.md) o [Windows](quickstart.md). Si ejecuta el demonio de IoT Edge en el equipo de desarrollo, es posible que deba detener EdgeHub y EdgeAgent antes de ir al paso siguiente.

## <a name="create-a-new-solution-template"></a>Creación de una plantilla de solución

En los siguientes pasos se muestra cómo crear un módulo de IoT Edge en su lenguaje de desarrollo preferido (incluido Azure Functions, escrito en C#) mediante Visual Studio Code y Herramientas de Azure IoT. Primero debe crear una solución y, a continuación, generar el primer módulo de esa solución. Cada solución puede contener varios módulos.

1. Seleccione **Ver** > **Paleta de comandos**.

1. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Nueva solución de IoT Edge).

   ![Ejecución de la nueva solución IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

1. Vaya a la carpeta en la que desea crear la solución y después haga clic en **Seleccionar carpeta**.

1. Escriba un nombre para la solución.

1. Seleccione una plantilla de módulo para que su lenguaje de desarrollo preferido sea el primer módulo de la solución.

1. Escriba un nombre para el módulo. Elija un nombre que sea único dentro del registro de contenedor.

1. Proporcione el nombre del repositorio de imágenes del módulo. Visual Studio Code completa automáticamente el nombre del módulo como **localhost:5000 /<el nombre del módulo\>**. Reemplácelo por su propia información de registro. Si usa un registro de Docker local para realizar pruebas, **localhost** puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a ***\<nombre del registro\>*.azurecr.io**. Reemplace solo la sección **localhost:5000** de la cadena para que el resultado final se parezca a **\<* nombre del registro*\>.azurecr.io/*\<nombre del módulo\>***.

   ![Especificación del repositorio de imágenes de Docker](./media/how-to-develop-csharp-module/repository.png)

Visual Studio Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana.

Hay cuatro elementos dentro de la solución:

- Una carpeta **.vscode** que contiene configuraciones de depuración.

- Una carpeta **modules** que contiene subcarpetas para cada módulo. En este momento, solo tiene uno. Sin embargo, podría agregar más en la paleta de comandos con el comando **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: agregar módulo IoT Edge).

- Un archivo **.env** enumera las variables de entorno. Si Azure Container Registry es su registro, tendrá en él un nombre de usuario y una contraseña de Azure Container Registry.

  > [!NOTE]
  > Solo se crea el archivo de entorno si proporciona un repositorio de imágenes para el módulo. Si aceptó los valores predeterminados de localhost para probar y depurar localmente, no es necesario declarar las variables de entorno.

- Un archivo **deployment.template.json** muestra el nuevo módulo junto con un módulo **tempSensor** de ejemplo que simula los datos que puede usar para las pruebas. Para más información sobre cómo funcionan los manifiestos de implementación, consulte [cómo usar los manifiestos de implementación para implementar módulos y establecer rutas](module-composition.md).

## <a name="add-additional-modules"></a>Agregar módulos adicionales

Para agregar módulos adicionales a la solución, ejecute el comando **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: Agregar módulo IoT Edge) desde la paleta de comandos. También puede hacer clic en la carpeta **módulos** o el archivo `deployment.template.json` en la vista Explorador de Visual Studio Code y después seleccionar **Agregar módulo IoT Edge**.

## <a name="develop-your-module"></a>Desarrollo de su módulo

El código del módulo predeterminado que se incluye con la solución se encuentra en la siguiente ubicación:

- Azure Function (C#): **módulos > *&lt;el nombre del módulo&gt;* > *&lt;el nombre del módulo&gt;*.cs**
- C#: **módulos > *&lt;el nombre del módulo&gt;* > Program.cs**
- Python: **módulos > *&lt;el nombre del módulo&gt;* > main.py**
- Node.js: **módulos > *&lt;el nombre del módulo&gt;* > app.js**
- Java: **módulos > *&lt;el nombre del módulo&gt;* > src > main > java > com > edgemodulemodules > App.java**
- C: **módulos > *&lt;el nombre del módulo&gt;* > main.c**

El módulo y el archivo deployment.template.json se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo para iniciar la prueba sin tocar el código. El módulo se compila para tomar simplemente la entrada de un origen (en este caso, el módulo tempSensor que simula datos) y canalizarlo a IoT Hub.

Cuando tenga todo listo para personalizar la plantilla de C con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como seguridad, administración de dispositivos y confiabilidad.

## <a name="debug-a-module-without-a-container-c-nodejs-java"></a>Depurar un módulo sin un contenedor (C#, Node.js, Java)

Si está desarrollando en C#, Node.js o Java, el módulo requiere el uso de un objeto **ModuleClient** en el código del módulo predeterminado para poder iniciar, ejecutar y enrutar los mensajes. También usará el canal de entrada predeterminado **input1** para realizar una acción cuando el módulo reciba los mensajes.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>Configuración del simulador de IoT Edge para la solución de IoT Edge

En el equipo de desarrollo, puede iniciar un simulador de IoT Edge en lugar de instalar el demonio de seguridad de IoT Edge de forma que pueda ejecutar la solución de IoT Edge.

1. En el explorador de dispositivos del lado izquierdo, haga clic con el botón derecho en el identificador de dispositivo IoT Edge y después seleccione **Setup IoT Edge Simulator** (Configurar el simulador de IoT Edge) para iniciar el simulador con la cadena de conexión del dispositivo.
1. Puede ver que el simulador de IoT Edge se ha configurado correctamente leyendo la información de progreso en el terminal integrado.

### <a name="set-up-iot-edge-simulator-for-single-module-app"></a>Configuración del simulador de IoT Edge para la aplicación de módulo único

Para configurar e iniciar el simulador, ejecute el comando **Azure IoT Edge: Start IoT Edge Hub Simulator for Single Module** (Azure IoT Edge: Iniciar el simulador de centro de IoT Edge para módulo único) desde la paleta de comandos de Visual Studio Code. Cuando se le pida, use el valor **input1** del código de módulo predeterminado (o el valor equivalente de su código) como el nombre de entrada para la aplicación. El comando desencadena la CLI **iotedgehubdev** y después inicia el simulador de IoT Edge y un contenedor de módulo de herramienta para pruebas. Puede ver las siguientes salidas en el terminal integrado si el simulador se ha iniciado correctamente en modo de módulo único. También puede ver un comando `curl` para ayudar a enviar el mensaje. Lo usará más adelante.

   ![Configuración del simulador de IoT Edge para la aplicación de módulo único](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Puede usar la vista Explorador de Docker en Visual Studio Code para ver el estado de ejecución del módulo.

   ![Estado del módulo simulador](media/how-to-develop-csharp-module/simulator-status.png)

   El contenedor **edgeHubDev** es el núcleo del simulador de IoT Edge local. Puede ejecutarse en el equipo de desarrollo sin el demonio de seguridad de IoT Edge y proporcionar la configuración del entorno para la aplicación de módulo nativa o los contenedores de módulo. El contenedor de **entrada** expone las API REST para ayudar a que los mensajes lleguen al canal de entrada de destino en el módulo.

### <a name="debug-module-in-launch-mode"></a>Depuración del módulo en el modo de inicio

1. Prepare el entorno de depuración según los requisitos de su lenguaje de desarrollo, establezca un punto de interrupción en el módulo y seleccione la configuración de depuración para usar:
   - **C#**
     - En el terminal integrado de Visual Studio Code, cambie el directorio a la carpeta ***&lt;nombre del módulo&gt;*** y después ejecute el siguiente comando para compilar la aplicación de .Net Core.

       ```cmd
       dotnet build
       ```

     - Abra el archivo `program.cs` y agregue un punto de interrupción.

     - Vaya a la vista Depuración de Visual Studio Code seleccionando **Vista > Depurar**. Seleccione la configuración de depuración ***&lt;nombre del módulo&gt;* Local Debug (.NET Core)** en la lista desplegable.

        > [!NOTE]
        > Si su `TargetFramework` de .Net Core no es coherente con la ruta de acceso del programa en `launch.json`, deberá actualizar manualmente la ruta de acceso del programa en `launch.json` para que coincida con el `TargetFramework` en el archivo .csproj para que Visual Studio Code pueda iniciar correctamente este programa.

   - **Node.js**
     - En el terminal integrado de Visual Studio Code, cambie el directorio a la carpeta ***&lt;nombre del módulo&gt;*** y después ejecute el siguiente comando para instalar paquetes Node.

       ```cmd
       npm install
       ```

     - Abra el archivo `app.js` y agregue un punto de interrupción.

     - Vaya a la vista Depuración de Visual Studio Code seleccionando **Vista > Depurar**. Seleccione la configuración de depuración ***&lt;nombre del módulo&gt;* Local Debug (Node.js)** en la lista desplegable.
   - **Java**
     - Abra el archivo `App.java` y agregue un punto de interrupción.

     - Vaya a la vista Depuración de Visual Studio Code seleccionando **Vista > Depurar**. Seleccione la configuración de depuración ***&lt;nombre del módulo&gt;* Local Debug (Java)** en la lista desplegable.

1. Haga clic en **Iniciar depuración** o presione **F5** para iniciar la sesión de depuración.

1. En el terminal integrado de Visual Studio Code, ejecute el siguiente comando para enviar un mensaje de **Hola mundo** al módulo. Este es el comando que se mostró en pasos anteriores cuando configuró el simulador de IoT Edge.

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Si usa Windows, asegúrese de que el shell del terminal integrado de Visual Studio Code es **Git Bash** o **WSL Bash**. No puede ejecutar el comando `curl` en PowerShell ni desde el símbolo del sistema.
   > [!TIP]
   > También puede usar [PostMan](https://www.getpostman.com/) u otras herramientas de API para enviar mensajes en lugar de `curl`.

1. En la vista Depuración de Visual Studio Code, verá las variables en el panel izquierdo.

1. Para detener la sesión de depuración, haga clic en el botón Detener o presione **MAYÚS + F5** y después ejecute **Azure IoT Edge: Stop IoT Edge Simulator** (Azure IoT Edge: Detener el simulador de IoT Edge) en la paleta de comandos para detener el simulador y limpiar.

## <a name="debug-in-attach-mode-with-iot-edge-simulator-c-nodejs-java-azure-functions"></a>Depuración en modo de agregación con el simulador de IoT Edge (C#, Node.js, Java, Azure Functions)

La solución predeterminada contiene dos módulos, uno es un módulo de sensor de temperatura simulado y el otro es el módulo de canalización. El sensor de temperatura simulado manda mensajes al módulo de canalización, y después los mensajes se canalizan a IoT Hub. En la carpeta de módulo que se creó, hay varios archivos de Docker para diferentes tipos de contenedor. Use cualquiera de estos archivos que acaban con la extensión **.debug** para compilar el módulo de pruebas.

Actualmente, la depuración en modo de agregación solo es compatible como sigue:

- Los módulos de C#, incluyendo los de Azure Functions, admiten la depuración en contenedores de Linux amd64
- Los módulos de Node.js admiten la depuración en contenedores de Linux amd64 y arm32v7, y en contenedores de Windows amd64
- Los módulos de Java admiten la depuración en contenedores de Linux amd64 y arm32v7

> [!TIP]
> Puede cambiar las opciones de la plataforma predeterminada para la solución de IoT Edge si hace clic en el elemento en la barra de estado de Visual Studio Code.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>Configuración del simulador de IoT Edge para la solución de IoT Edge

En el equipo de desarrollo, puede iniciar un simulador de IoT Edge en lugar de instalar el demonio de seguridad de IoT Edge de forma que pueda ejecutar la solución de IoT Edge.

1. En el explorador de dispositivos del lado izquierdo, haga clic con el botón derecho en el identificador de dispositivo IoT Edge y después seleccione **Setup IoT Edge Simulator** (Configurar el simulador de IoT Edge) para iniciar el simulador con la cadena de conexión del dispositivo.

1. Puede ver que el simulador de IoT Edge se ha configurado correctamente leyendo la información de progreso en el terminal integrado.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Compilación y ejecución del contenedor para la depuración y depuración en modo de agregación

1. Abra el archivo de módulo (`program.cs`, `app.js`, `App.java` o `<your module name>.cs`) y agregue un punto de interrupción.

1. En la vista Explorador de Visual Studio Code, haga clic con el botón derecho en el archivo `deployment.debug.template.json` para la solución y después seleccione **Compilar y ejecutar solución de IoT Edge en el Simulador**. Puede inspeccionar todos los registros del contenedor del módulo en la misma ventana. También puede navegar hasta la vista Docker para observar el estado del contenedor.

   ![Inspección de variables](media/how-to-develop-csharp-module/view-log.png)

1. Vaya a la vista Depuración de Visual Studio Code y seleccione el archivo de configuración de depuración del módulo. El nombre de la opción de depuración debe ser similar a ***&lt;nombre del módulo&gt;* Remote Debug**

1. Haga clic en **Iniciar depuración** o presione **F5**. Seleccione el proceso al que desea asociarlo.

1. En la vista Depuración de Visual Studio Code, verá las variables en el panel izquierdo.

1. Para detener la sesión de depuración, primero haga clic en el botón Detener o presione **MAYÚS + F5**, y después ejecute **Azure IoT Edge: Stop IoT Edge Simulator** (Azure IoT Edge: Detener módulo IoT Edge) desde la paleta de comandos.

> [!NOTE]
> En el ejemplo anterior se muestra cómo depurar módulos de IoT Edge en contenedores. Ha agregado puertos expuestos a la configuración `createOptions` del contenedor del módulo. Una vez finalizada la depuración de los módulos, se recomienda quitar estos puertos expuestos de los módulos de IoT Edge listos para entornos de producción.
>
> Para módulos escritos en C#, incluyendo los de Azure Functions, este ejemplo se basa en la versión de depuración de `Dockerfile.amd64.debug`, que incluye el depurador de línea de comandos de .NET Core (VSDBG) en la imagen de contenedor al realizar la compilación. Una vez finalizada la depuración de los módulos de C#, se recomienda usar directamente Dockerfile sin VSDBG para módulos de IoT Edge listos para producción.

## <a name="debug-a-module-with-iot-edge-runtime"></a>Depuración de un módulo con el entorno de tiempo de ejecución de IoT Edge

En cada carpeta de módulo, hay varios archivos de Docker para diferentes tipos de contenedor. Use cualquiera de estos archivos que acaban con la extensión **.debug** para compilar el módulo de pruebas.

Al depurar los módulos con el entorno de tiempo de ejecución de IoT Edge, los módulos se ejecutan sobre dicho entorno. El dispositivo de IoT Edge y VS Code pueden estar en la misma máquina, o como es más habitual, en distintas máquinas (VS Code se encuentra en el equipo de desarrollo y el entorno de tiempo de ejecución y los módulos de IoT Edge se ejecutan en otro equipo físico). Deben seguirse estos pasos para la sesión de depuración en VS Code.

- Configure el dispositivo de IoT Edge, cree módulos de IoT Edge con el Dockerfile **.debug** e implemente en un dispositivo de IoT Edge. 
- Exponga la dirección IP y el puerto del módulo para que los adjunte el depurador.
- Actualice el archivo `launch.json` para que VS Code pueda adjuntarse al proceso en el contenedor en el equipo remoto.

### <a name="build-and-deploy-your-module-and-deploy-to-iot-edge-device"></a>Compilación e implementación del módulo e implementación en el dispositivo de IoT Edge

1. En Visual Studio Code, abra el archivo `deployment.debug.template.json`, que contiene la versión de depuración de las imágenes del módulo con valores de `createOptions` adecuados establecidos.

1. Si está desarrollando su módulo de Python, siga estos pasos antes de continuar:
   - Abra el archivo `main.py` y agregue este código después de la sección de importación:

      ```python
      import ptvsd
      ptvsd.enable_attach(('0.0.0.0',  5678))
      ```
   - Agregue la siguiente línea de código única a la devolución de llamada que quiera depurar:

      ```python
      ptvsd.break_into_debugger()
      ```

     Por ejemplo, si quiere depurar el método `receive_message_callback`, insertaría esa línea de código tal como se muestra a continuación:

      ```python
      def receive_message_callback(message, hubManager):
          ptvsd.break_into_debugger()
          global RECEIVE_CALLBACKS
          message_buffer = message.get_bytearray()
          size = len(message_buffer)
          print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode ('utf-8'), size) )
          map_properties = message.properties()
          key_value_pair = map_properties.get_internals()
          print ( "    Properties: %s" % key_value_pair )
          RECEIVE_CALLBACKS += 1
          print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
          hubManager.forward_event_to_output("output1", message, 0)
          return IoTHubMessageDispositionResult.ACCEPTED
      ```

1. En la paleta de comandos de Visual Studio Code:
   1. Ejecute el comando **Azure IoT Edge: Build and Push IoT Edge solution (Azure IoT Edge: Compilación e inserción de la solución de IoT Edge)**.

   1. Seleccione el archivo `deployment.debug.template.json` para su solución.

1. En la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) de la vista Explorador de Visual Studio Code:
   1. Haga clic con el botón derecho en un Id. de dispositivo de IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear una implementación para un dispositivo individual).

   1. Vaya a la carpeta **config** de la solución, seleccione el archivo `deployment.debug.amd64.json` y haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge).

Verá que la implementación se ha creado correctamente con un identificador de implementación en el terminal integrado.

Puede comprobar el estado del contenedor mediante la ejecución del comando `docker ps` en el terminal. Si el entorno de tiempo de ejecución de IoT Edge y VS Code se ejecutan en el mismo equipo, también puede comprobar el estado en la vista de Docker de Visual Studio Code.

### <a name="expose-the-ip-and-port-of-the-module-for-the-debugger-to-attach"></a>Exposición de la dirección IP y el puerto del módulo para que los adjunte el depurador

Si los módulos se ejecutan en el mismo equipo que VS Code. Usa localhost para adjuntar el contenedor y ya tiene la configuración de puerto correcta en el Dockerfile **.debug**, el contenedor de módulo CreateOptions y `launch.json`. Puede omitir esta sección. Si los módulos y VS Code se ejecutan en máquinas independientes, siga estos pasos para cada lenguaje.

  - **C#, Función de C#**: [Configure el canal SSH en el equipo de desarrollo y el dispositivo de IoT Edge](https://github.com/OmniSharp/omnisharp-vscode/wiki/Attaching-to-remote-processes), edite el archivo `launch.json` para adjuntar.
  - **Node.js**: Asegúrese de que el módulo esté listo para que los depuradores lo puedan adjuntar y de que se puede acceder al puerto 9229 de la máquina que se va a depurar desde el exterior. Puede comprobarlo abriendo [http://%3cdebuggee-machine-IP%3e:9229/json]http://<IP de máquina que se va a depurar>:9229/json en el equipo del depurador. Esta dirección URL debe mostrar información sobre el Node.js que se desea depurar. Y, a continuación, en la máquina del depurador, abra VS Code, edite el archivo `launch.json` para que el valor de la dirección del perfil "<nombre-módulo> Remote Debug (Node.js)" (o el perfil "<nombre-módulo> Remote Debug [Node js en un contenedor Windows]" si el módulo se está ejecutando como un contenedor Windows) sea la dirección IP de la máquina que se va a depurar.
  - **Java**: Compile un túnel de ssh para el dispositivo de Edge ejecutando `ssh -f <username>@<edgedevicehost> -L 5005:127.0.0.1:5005 -N`; a continuación, edite el archivo `launch.json` para adjuntarlo. Puede obtener más información sobre esta configuración [aquí](https://code.visualstudio.com/docs/java/java-debugging). 
  - **Python**: En el código `ptvsd.enable_attach(('0.0.0.0', 5678))`, cambie 0.0.0.0 a la dirección IP del dispositivo de IoT Edge. Compile, inserte y vuelva a implementar los módulos de IoT Edge. En `launch.json` en el equipo de desarrollo, actualice `"host"` `"localhost"` cambie `"localhost"` por la dirección IP pública de su dispositivo de IoT Edge remoto.


### <a name="debug-your-module"></a>Depuración del módulo

Visual Studio Code sigue depurando la información de configuración en un archivo `launch.json` ubicado en una carpeta `.vscode` en el área de trabajo. Este archivo `launch.json` se ha generado al crear una nueva solución de IoT Edge. Se actualiza cada vez que agregue un nuevo módulo que admita la depuración.

1. En la vista Depuración de Visual Studio Code seleccione el archivo de configuración de depuración del módulo. El nombre de la opción de depuración debe ser similar a ***&lt;nombre del módulo&gt;* Remote Debug**

1. Abra el archivo del módulo para su lenguaje de desarrollo y agregue un punto de interrupción:
   - **C#, Función de C#**: Abra el archivo `Program.cs` y agregue un punto de interrupción.
   - **Node.js**: Abra el archivo `app.js` y agregue un punto de interrupción.
   - **Java**: Abra el archivo `App.java` y agregue un punto de interrupción.
   - **Python**: Abra `main.py` y agregue un punto de interrupción en el método de devolución de llamada que ha agregado la línea `ptvsd.break_into_debugger()`.
   - **C**: Abra el archivo `main.c` y agregue un punto de interrupción.

1. Seleccione **Iniciar depuración** o presione **F5**. Seleccione el proceso al que desea asociarlo.

1. En la vista Depuración de Visual Studio Code, verá las variables en el panel izquierdo.

> [!NOTE]
> En el ejemplo anterior se muestra cómo depurar módulos de IoT Edge en contenedores. Ha agregado puertos expuestos a la configuración `createOptions` del contenedor del módulo. Una vez finalizada la depuración de los módulos, se recomienda quitar estos puertos expuestos de los módulos de IoT Edge listos para entornos de producción.

## <a name="next-steps"></a>Pasos siguientes

Después de compilar el módulo, aprenda cómo [implementar módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md).

Para desarrollar módulos para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
