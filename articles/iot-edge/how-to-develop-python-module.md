---
title: Depuración de módulos de Python para Azure IoT Edge | Microsoft Docs
description: Uso de Visual Studio Code para desarrollar, compilar y depurar un módulo de Python para Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/13/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4393d3231ca4f0a813d4586161383bd6d61f6a49
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45552339"
---
# <a name="use-visual-studio-code-to-develop-and-debug-python-modules-for-azure-iot-edge"></a>Uso de Visual Studio Code para desarrollar y depurar módulos de Python para Azure IoT Edge

Puede convertir la lógica de negocios en módulos de Azure IoT Edge. En este artículo se muestra cómo usar Visual Studio Code (VS Code) como herramienta principal para desarrollar y depurar módulos de Python.

## <a name="prerequisites"></a>Requisitos previos
En este artículo se da por hecho que usa una máquina virtual que ejecuta Windows o Linux como máquina de desarrollo. Y puede simular el dispositivo IoT Edge en la máquina de desarrollo con el demonio de seguridad de IoT Edge.

> [!NOTE]
> En este tutorial de depuración se demuestra cómo asociar un proceso en un contenedor de módulos y depurarlo con VS Code. Los módulos de Python solo se pueden depurar en contenedores Linux amd64. Si no está familiarizado con las funcionalidades de depuración de Visual Studio Code, consulte más información sobre [depuración](https://code.visualstudio.com/Docs/editor/debugging). 

En este artículo se usa Visual Studio Code como herramienta de desarrollo principal, así que instale VS Code. A continuación, agregue las extensiones necesarias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensión de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) para Visual Studio Code. 
* [Python](https://www.python.org/downloads/)
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) para instalar paquetes de Python (normalmente, se incluye con la instalación de Python).
* Instale **Cookiecutter** con el siguiente comando
   
    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

Para crear un módulo, necesita Docker para compilar la imagen del módulo y un registro de contenedor para guardar esta:
* [Docker Community Edition](https://docs.docker.com/install/) en la máquina de desarrollo 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Puede usar un registro de Docker local con fines de prototipo y prueba en lugar de un registro en la nube. 

Para probar el módulo en un dispositivo, necesita una instancia de IoT Hub con al menos un dispositivo IoT Edge. Para usar el equipo como dispositivo IoT Edge, siga los pasos descritos en el tutorial de inicio rápido para [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Creación de una plantilla de solución

Siga estos pasos para crear un módulo de IoT Edge basado en el SDK de Azure IoT para Python mediante Visual Studio Code y la extensión de Azure IoT Edge. En primer lugar, cree una solución y, luego, genere el primer módulo en esa solución. Cada solución puede contener más de un módulo. 

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado**.

2. Seleccione **Ver** > **Paleta de comandos**. 

3. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: nueva solución de IoT Edge).

   ![Ejecución de la nueva solución IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

4. Seleccione la carpeta en la que desea crear la solución. Elija **Select folder** (Seleccionar carpeta). 

5. Escriba un nombre para la solución. 

6. Seleccione **Python Module** (Módulo de Python) como plantilla del primer módulo de la solución.

7. Escriba un nombre para el módulo. Elija un nombre que sea único dentro del registro de contenedor. 

8. Proporcione el nombre del repositorio de imágenes del módulo. VS Code rellena automáticamente el nombre del módulo con **localhost:5000**. Reemplácelo por su propia información de registro. Si usa un registro de Docker local para realizar pruebas, **localhost** puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a **\<nombre del registro\>.azurecr.io**. Reemplace solo la parte de localhost de la cadena, no elimine el nombre del módulo. 

   ![Especificación del repositorio de imágenes de Docker](./media/how-to-develop-c-module/repository.png)

VS Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana.

Hay cuatro elementos dentro de la solución: 
* Una carpeta **.vscode** que contiene configuraciones de depuración.
* Una carpeta **modules** que contiene subcarpetas para cada módulo. En este momento, solo tiene uno. Sin embargo, podría agregar más en la paleta de comandos con el comando **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: agregar módulo IoT Edge). 
* Un archivo **.env** enumera las variables de entorno. Si Azure Container Registry es su registro, tendrá en él un nombre de usuario y una contraseña de Azure Container Registry. 

   > [!NOTE]
   > Solo se crea el archivo de entorno si proporciona un repositorio de imágenes para el módulo. Si aceptó los valores predeterminados de localhost para probar y depurar localmente, no es necesario declarar las variables de entorno. 

* Un archivo **deployment.template.json** muestra el nuevo módulo junto con un módulo **tempSensor** de ejemplo que simula los datos que puede usar para las pruebas. Para más información sobre cómo funcionan los manifiestos de implementación, consulte [cómo usar los manifiestos de implementación para implementar módulos y establecer rutas](module-composition.md). 

## <a name="develop-your-module"></a>Desarrollo de su módulo

El código del módulo de Python predeterminado que se incluye con la solución se encuentra en **modules** > [nombre del módulo] > **main.py**. El módulo y el archivo deployment.template.json se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo para iniciar la prueba sin tocar el código. El módulo se compila para tomar simplemente la entrada de un origen (en este caso, el módulo tempSensor que simula datos) y canalizarlo a IoT Hub. 

Cuando esté listo para personalizar la plantilla de Python con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como seguridad, administración de dispositivos y confiabilidad. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Compilación e implementación del módulo para depuración

En cada carpeta de módulo, hay varios archivos de Docker para diferentes tipos de contenedor. Use cualquiera de estos archivos que acaban con la extensión **.debug** para compilar el módulo de pruebas. Actualmente, los módulos de Python solo admiten la depuración en contenedores de Linux amd64. 

1. En VS Code, vaya al archivo `deployment.template.json`. Para actualizar la dirección URL de la imagen del módulo, agregue **.debug** al final.

2. Reemplace el módulo createOptions de Python en el archivo **deployment.template.json** por el siguiente contenido y guarde este archivo: 
    
    ```json
    "createOptions": "{\"ExposedPorts\":{\"5678/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"5678/tcp\":[{\"HostPort\":\"5678\"}]}}}"
    ```

3. Vaya a `main.py`, agregue los códigos siguientes después de la sección de importación
    
    ```python
    import ptvsd
    ptvsd.enable_attach(('0.0.0.0',  5678))
    ```

4. Agregue la siguiente línea de código única a la devolución de llamada que desea depurar

    ```python
    ptvsd.break_into_debugger()
    ```

   Por ejemplo, si desea depurar el método `receive_message_callback`. Puede incluir la línea de código única como la siguiente.

    ```python
    def receive_message_callback(message, hubManager):
        ptvsd.break_into_debugger()
        global RECEIVE_CALLBACKS
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode('utf-8'), size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

2. En la paleta de comandos de VS Code, escriba y ejecute el comando **Azure IoT Edge: Build and Push IoT Edge solution** (Azure IoT Edge: compilar e insertar solución IoT Edge).
3. Seleccione el archivo `deployment.template.json` para la solución en la paleta de comandos. 
4. En Device Explorer de Azure IoT Hub, haga clic con el botón derecho en un identificador de dispositivo IoT Edge. Seleccione **Create Deployment for Single Device** (Crear una implementación para un dispositivo individual). 
5. Abra la carpeta **config** de la solución. A continuación, seleccione el archivo `deployment.json`. Haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge). 

Verá que la implementación se ha creado correctamente con un identificador de implementación en el terminal integrado de VS Code.

Puede comprobar el estado del contenedor en el explorador de Docker de VS Code o mediante la ejecución del comando `docker ps` en el terminal.

## <a name="start-debugging-python-module-in-vs-code"></a>Inicio de la depuración del módulo de Python en VS Code
VS Code sigue depurando la información de configuración en un archivo `launch.json` ubicado en una carpeta `.vscode` en el área de trabajo. Este archivo `launch.json` se ha generado al crear una nueva solución de IoT Edge. Se actualiza cada vez que agregue un nuevo módulo que admita la depuración. 

1. Vaya a la vista de depuración de VS Code. Seleccione el archivo de configuración de depuración de su módulo. El nombre de la opción de depuración debe ser similar a to **ModuleName Remote Debug (Python)**.

2. Vaya a `main.c`. Agregue un punto de interrupción en el método de devolución de llamada donde colocó `ptvsd.break_into_debugger()`.

3. Seleccione **Iniciar depuración** o presione **F5**. Seleccione el proceso al que desea asociarlo.

4. En la vista de depuración de VS Code, verá las variables en el panel izquierdo. 

En el ejemplo anterior se muestra cómo depurar módulos de Python de IoT Edge en contenedores. Agregó puertos expuestos en el contenedor del módulo createOptions. Cuando termine de depurar los módulos de Python, se recomienda quitar estos puertos expuestos para los módulos de IoT Edge listos para producción.

## <a name="next-steps"></a>Pasos siguientes

Después de compilar el módulo, aprenda cómo [implementar módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md).

Para desarrollar módulos para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
