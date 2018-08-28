---
title: Tutorial de Python para Azure IoT Edge | Microsoft Docs
description: En este tutorial, se explica cómo se crea un módulo IoT Edge con código Python y cómo se implementa en un dispositivo perimetral.
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 6cf3a721dfd601fc4d4beb122f56b4a4de5fe426
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41924805"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device"></a>Tutorial: Desarrollo e implementación de un módulo de Python para IoT Edge en un dispositivo simulado

Los módulos Azure IoT Edge se pueden usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos IoT Edge. En este tutorial, se detallan los pasos para crear e implementar un módulo de IoT Edge que filtra los datos de sensor. Para realizar el tutorial, utilizará el dispositivo IoT Edge que creó en las guías de inicio rápido para implementar Deploy Azure IoT Edge en un dispositivo simulado con [Windows][lnk-quickstart-win] o [Linux][lnk-quickstart-lin]. En este tutorial, aprenderá a:    

> [!div class="checklist"]
> * Usar Visual Studio Code para crear un módulo de Python para IoT Edge.
> * Utilizar Visual Studio Code y Docker para crear una imagen de Docker y publicarla en el Registro.
> * Implementar el módulo en el dispositivo IoT Edge.
> * Ver datos generados.


El módulo IoT Edge que creó en este tutorial filtra lo datos sobre la temperatura generados por el dispositivo. Solo envía mensajes a los niveles superiores si la temperatura sobrepasa el umbral especificado. Este tipo de análisis perimetral resulta útil para reducir la cantidad de datos que se comunican a la nube y se almacenan en ella. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para [Linux](quickstart-linux.md).
* Los módulos de Python para IoT Edge no admiten dispositivos Windows.

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar en Azure. 

Recursos de desarrollo:

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code.
* [Extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) para Visual Studio Code. 
* [Docker CE](https://docs.docker.com/engine/installation/). 
* [Python](https://www.python.org/downloads/)
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) para instalar paquetes de Python (normalmente, se incluye con la instalación de Python).

>[!Note]
>Asegúrese de que su carpeta `bin` se encuentra en la ruta de acceso de su plataforma. Normalmente `~/.local/` para UNIX y macOS, o `%APPDATA%\Python` en Windows.

## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor
En este tutorial, puede usar la extensión de Azure IoT Edge para VS Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.  

En este tutorial, puede usar cualquier Registro compatible con Docker. Dos de los servicios de Docker Registry más populares que están disponibles en la nube son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Azure Container Registry**.
2. Asigne un nombre al Registro, seleccione una suscripción, elija un grupo de recursos y establezca la SKU en el nivel **Básico**. 
3. Seleccione **Crear**.
4. Una vez que se haya creado el Registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**. 
5. Establezca **Usuario administrador** en **Habilitar**.
6. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. Usará estos valores más adelante en el tutorial. 

## <a name="create-an-iot-edge-module-project"></a>Creación de un proyecto de módulo IoT Edge
En los siguientes pasos creará un módulo de Python para IoT Edge mediante Visual Studio Code y la extensión de Azure IoT Edge.

### <a name="create-a-new-solution"></a>Creación de una nueva solución

Use el paquete **cookiecutter** de Python para crear una plantilla con la que generar la solución. 

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado** para abrir el terminal integrado de VS Code.

2. En el terminal integrado, escriba el siguiente comando para instalar (o actualizar) **cookiecutter**, ya que lo necesitará para crear la plantilla de la solución de IoT Edge en VS Code:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```
   >[!Note]
   >Asegúrese del directorio en el que se va a instalar cookiecutter está en el `Path` de su entorno para que sea posible invocarlo desde un símbolo del sistema.

3. Seleccione **Ver** > **Paleta de comandos** para abrir la paleta de comandos de VS Code. 

4. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión) y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.

5. En la paleta de comandos, escriba el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge) y ejecútelo. En la paleta de comandos, proporcione la siguiente información para crear la solución: 

   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado: **EdgeSolution**.
   3. Elija **Módulo de Python** como la plantilla del módulo. 
   4. Llame al módulo **PythonModule**. 
   5. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión que copió. La cadena final será similar a esta: \<nombre del Registro\>.azurecr.io/pythonmodule.
 
   ![Especificación del repositorio de imágenes de Docker](./media/tutorial-python-module/repository.png)

El área de trabajo de la solución de IoT Edge se carga en la ventana de Visual Studio Code. El área de trabajo de la solución contiene cinco componentes de nivel superior. En este tutorial no se editará el archivo **\.gitignore**. La carpeta **modules** contiene el código de Pyhton del módulo, así como archivos Dockerfile para compilar el módulo como una imagen de contenedor. El archivo **\.env** almacena las credenciales del registro de contenedor. El archivo **deployment.template.json** contiene la información que usa el entorno de ejecución de IoT Edge para implementar módulos en un dispositivo. 

Si no especificó un registro de contenedor al crear la solución y aceptó el valor predeterminado localhost:5000, no tendrá un archivo \.env. 

   ![Área de trabajo de la solución de Python](./media/tutorial-python-module/workspace.png)

### <a name="add-your-registry-credentials"></a>Adición de las credenciales del Registro

El archivo del entorno almacena las credenciales del repositorio de contenedores y las comparte con el entorno de ejecución de IoT Edge. El entorno de ejecución necesita estas credenciales para extraer las imágenes privadas e insertarlas en el dispositivo IoT Edge. 

1. En el explorador de VS Code, abra el archivo .env. 
2. Actualice los campos con los valores de **nombre de usuario** y **contraseña** que ha copiado del Registro de contenedor de Azure. 
3. Guarde este archivo. 

### <a name="update-the-module-with-custom-code"></a>Actualización del módulo con código personalizado

Cada plantilla incluye un código de ejemplo, que toma los datos del sensor simulado del módulo **tempSensor** y los enruta al centro de IoT. En esta sección, agregue el código que expande **PythonModule** para analizar los mensajes antes de enviarlos. 

1. En el explorador de VS Code, abra **módulos**  > **PythonModule** > **main.py**.

2. En la parte superior del archivo **main.py**, importe la biblioteca **json**:

    ```python
    import json
    ```

3. Agregue las variables **TEMPERATURE_THRESHOLD** y **TWIN_CALLBACKS** bajo los contadores globales. El umbral de temperatura establece el valor que debe superar la temperatura de la máquina para que los datos se envíen al centro de IoT.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

4. Reemplace la función **receive_message_callback** por el código siguiente:

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we forward this message to the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print ( "    Data: <<<%s>>> & Size=%d" % (message_text, size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature %s exceeds threshold %s" % (data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

5. Agregue una nueva función llamada **module_twin_callback**. Esta función se invoca cuando se actualicen las propiedades deseadas.

    ```python
    # module_twin_callback is invoked when the module twin's desired properties are updated.
    def module_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print ( "\nTwin callback called with:\nupdateStatus = %s\npayload = %s\ncontext = %s" % (update_state, payload, user_context) )
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print ( "Total calls confirmed: %d\n" % TWIN_CALLBACKS )
    ```

6. En la clase **HubManager**, agregue una nueva línea al método **__init__** para inicializar la función **module_twin_callback** que acaba de agregar:

    ```python
    # Sets the callback when a module twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Guarde este archivo.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior, creó una solución de IoT Edge y agregó código a **PythonModule** para filtrar los mensajes en los que la temperatura registrada por la máquina está por debajo del umbral aceptable. Ahora, tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedor. 

1. Escriba el comando siguiente en el terminal integrado de Visual Studio Code para iniciar sesión en Docker. A continuación, puede insertar la imagen del módulo en Azure Container Registry: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilice el nombre de usuario, la contraseña y el servidor de inicio de sesión que copió de Azure Container Registry en la primera sección. También puede recuperar estos valores en la sección **Claves de acceso** del registro en Azure Portal.

2. En el explorador de VS Code, abra el archivo deployment.template.json en el área de trabajo de la solución de IoT Edge. 

   Este archivo le dice a **$edgeAgent** que implemente dos módulos: **tempSensor**, que simula los datos del dispositivo, y **PythonModule**. El valor **PythonModule.image** se establece en una versión para Linux amd64 de la imagen. Para más información sobre los manifiestos de implementación, consulte esta [descripción acerca de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

   Este archivo también contiene las credenciales del registro. En el archivo de plantilla, el nombre de usuario y la contraseña contienen marcadores de posición. Cuando se genera el manifiesto de implementación, los campos se actualizan con los valores que ha agregado al archivo .env. 

3. Agregue el módulo gemelo de **PythonModule** que se corresponde con el manifiesto de implementación. Inserte el siguiente contenido JSON en la parte inferior de la sección **moduleContent**, después del módulo gemelo **$edgeHub**: 
    ```json
        "PythonModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Guarde este archivo.

5. En el explorador de VS Code, haga clic con el botón derecho en el archivo deployment.template.json y seleccione **Build and Push IoT Edge solution** (Compilar e insertar solución de IoT Edge). 

Cuando le indica a Visual Studio Code que compile la solución, esta herramienta primero toma la información de la plantilla de implementación y genera un archivo deployment.json en una nueva carpeta denominada **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos compilan el código, empaquetan el código Python en contenedores e insertan el código en el registro de contenedor que especificó cuando inicializó la solución. 

Puede ver la dirección completa de la imagen de contenedor con la etiqueta del comando `docker build` que se ejecuta en el terminal integrado de VS Code. La dirección de la imagen se crea a partir de la información del archivo module.json, con el formato \<repositorio\>:\<versión\>-\<plataforma\>. En este tutorial, debería ser similar a registryname.azurecr.io/pythonmodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

En el artículo de la guía de inicio rápido que siguió para configurar el dispositivo de IoT Edge, implementó un módulo mediante Azure Portal. También puede implementar módulos con la extensión Azure IoT Toolkit para Visual Studio Code. Ya tiene un manifiesto de implementación preparado para su escenario, el archivo **deployment.json**. Ahora todo lo que necesita hacer es seleccionar un dispositivo que reciba la implementación.

1. En la paleta de comandos de VS Code, ejecute **Azure IoT Hub: Select IoT Hub**. 

2. Elija la suscripción y la instancia de IoT Hub que contienen el dispositivo IoT Edge que desea configurar. 

3. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

4. Haga clic con el botón derecho en el nombre del dispositivo IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear una implementación para un dispositivo individual). 

   ![Crear una implementación para un dispositivo individual](./media/tutorial-python-module/create-deployment.png)

5. Seleccione el archivo **deployment.json** de la carpeta **config** y, a continuación, haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge). No utilice el archivo deployment.template.json. 

6. Haga clic en el botón Actualizar. Debería ver el nuevo **PythonModule** en ejecución junto con el módulo **TempSensor**, así como **$edgeAgent** y **$edgeHub**. 

## <a name="view-generated-data"></a>Visualización de datos generados

1. Para supervisar los datos que llegan al centro de IoT, seleccione los puntos suspensivos (**...**) y, luego, **Start Monitoring D2C Messages** (Inicio de la supervisión de mensajes de D2C).
2. Para supervisar el mensaje de D2C de un determinado dispositivo, haga clic con el botón derecho en un dispositivo de la lista y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
3. Para dejar de supervisar los datos, ejecute el comando **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: detener la supervisión del mensaje de D2C) en la paleta de comandos. 
4. Para ver o actualizar el módulo gemelo, haga clic con el botón derecho en el módulo de la lista y seleccione **Edit module twin** (Editar módulo gemelo). Para actualizar el módulo gemelo, guarde el archivo JSON gemelo, haga clic con el botón derecho en el área del editor y seleccione **Update Module Twin** (Actualizar módulo gemelo).
5. Para ver los registros de Docker, instale [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para VS Code. Puede encontrar los módulos que se están ejecutando localmente en el explorador de Docker. En el menú contextual, haga clic en **Show Logs** (Mostrar registros) en la vista del terminal integrado. 

## <a name="clean-up-resources"></a>Limpieza de recursos 

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo IoT Edge como dispositivo de prueba. 

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

### <a name="delete-local-resources"></a>Eliminación de recursos locales

Si desea eliminar el runtime de Azure IoT Edge y los recursos relacionados del dispositivo, use los siguientes comandos. 

Quite el entorno de ejecución de Azure IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Cuando se quita el entorno de ejecución de Azure IoT Edge, los contenedores que se han creado se detienen, pero siguen existiendo en el dispositivo. Vea todos los contenedores.

   ```bash
   sudo docker ps -a
   ```

Elimine los contenedores del entorno de ejecución que se crearon en el dispositivo.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Elimine los contenedores adicionales que se muestran en la salida `docker ps` haciendo referencia a los nombres de los contenedores. 

Quite el entorno de ejecución del contenedor.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo IoT Edge que contiene código para filtrar datos sin procesar generados por el dispositivo IoT Edge. Puede continuar con los siguientes tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Implementación de Función de Azure como módulo](tutorial-deploy-function.md)
> [Implementación de Azure Stream Analytics como módulo](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
