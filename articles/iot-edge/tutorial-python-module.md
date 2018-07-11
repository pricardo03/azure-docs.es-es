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
ms.openlocfilehash: 42af2b5ec6b591929f37afebe6546d61b8a3a02a
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082855"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device"></a>Tutorial: Desarrollo e implementación de un módulo de Python para IoT Edge en un dispositivo simulado

Los módulos de IoT Edge se pueden usar para implementar código que, a su vez, implemente una lógica de negocios directamente en los dispositivos de IoT Edge. En este tutorial, se detallan los pasos para crear e implementar un módulo de IoT Edge que filtra los datos de sensor. Para realizar el tutorial, utilizará el dispositivo IoT Edge que creó en las guías de inicio rápido para implementar Deploy Azure IoT Edge en un dispositivo simulado con [Windows][lnk-quickstart-win] o [Linux][lnk-quickstart-lin]. En este tutorial, aprenderá lo siguiente:    

> [!div class="checklist"]
> * Uso de Visual Studio Code para crear un módulo de Python para IoT Edge
> * Uso de Visual Studio Code y Docker para crear una imagen de Docker y publicarla en el Registro 
> * Implementación del módulo en el dispositivo IoT Edge
> * Consulta de los datos generados


El módulo IoT Edge que creó en este tutorial filtra lo datos sobre la temperatura generados por el dispositivo. Solo envía mensajes a los niveles superiores si la temperatura sobrepasa el umbral especificado. Este tipo de análisis perimetral resulta útil para reducir la cantidad de datos que se comunican a la nube y se almacenan en ella. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de empezar.


## <a name="prerequisites"></a>Requisitos previos

* El dispositivo Azure IoT Edge que creó en la guía de inicio rápido para [Linux](quickstart-linux.md).

   >[!Note]
   >Los módulos de Python para Azure IoT Edge no son compatibles con dispositivos Windows o ARM. 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensión de Python para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 
* [Docker](https://docs.docker.com/engine/installation/): debe estar instalado en el mismo equipo que Visual Studio Code. La versión Community Edition (CE) es suficiente para este tutorial. 
* [Python](https://www.python.org/downloads/)
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) para instalar paquetes de Python (normalmente, se incluye con la instalación de Python).

## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor
En este tutorial, puede usar la extensión de Azure IoT Edge para VS Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.  

En este tutorial, puede usar cualquier Registro compatible con Docker. Dos de los servicios de Registro de Docker más populares que están disponibles en la nube son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Azure Container Registry**.
2. Asigne un nombre al Registro, seleccione una suscripción, elija un grupo de recursos y establezca la SKU en el nivel **Básico**. 
3. Seleccione **Crear**.
4. Una vez que se haya creado el Registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**. 
5. Establezca **Usuario administrador** en **Habilitar**.
6. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. Los usará más adelante en el tutorial. 

## <a name="create-an-iot-edge-module-project"></a>Creación de un proyecto de módulo IoT Edge
En los procedimientos siguientes, se explica cómo puede crear un módulo de Python para IoT Edge utilizando Visual Studio Code y la extensión de Azure IoT Edge.

### <a name="create-a-new-solution"></a>Creación de una nueva solución

Use el paquete **cookiecutter** de Python para crear una plantilla con la que generar la solución. 

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado** para abrir el terminal integrado de VS Code.

2. En el terminal integrado, escriba el siguiente comando para instalar (o actualizar) **cookiecutter**, ya que lo necesitará para crear la plantilla de la solución de Edge en VS Code:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Seleccione **Ver** > **Paleta de comandos** para abrir la paleta de comandos de VS Code. 

4. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión) y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.

5. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución IoT Edge). En la paleta de comandos, proporcione la siguiente información para crear la solución: 

   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado: **EdgeSolution**.
   3. Elija **Módulo de Python** como la plantilla del módulo. 
   4. Llame al módulo **PythonModule**. 
   5. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión que copió. La cadena final será similar a esta: **\<nombre del Registro\>.azurecr.io/pythonmodule**.
 
El área de trabajo de la solución de IoT Edge se carga en la ventana de Visual Studio Code. Hay una carpeta de **módulos**, un archivo con la plantilla del manifiesto de implementación y un archivo **.env**. 

### <a name="add-your-registry-credentials"></a>Adición de las credenciales del Registro

El archivo del entorno almacena las credenciales del repositorio de contenedores y las comparte con el entorno de ejecución de IoT Edge. El entorno de ejecución necesita estas credenciales para extraer las imágenes privadas e insertarlas en el dispositivo IoT Edge. 

1. En el explorador de VS Code, abra el archivo **.env**. 
2. Actualice los campos con los valores de **nombre de usuario** y **contraseña** que ha copiado del Registro de contenedor de Azure. 
3. Guarde este archivo. 

### <a name="update-the-module-with-custom-code"></a>Actualización del módulo con código personalizado

Cada plantilla viene con un código de ejemplo incluido, que toma los datos del sensor simulado del módulo **tempSensor** y los enruta a IoT Hub. En esta sección, agregue el código que expande PythonModule para analizar los mensajes antes de enviarlos. 

1. En el explorador de VS Code, abra **módulos**  > **PythonModule** > **main.py**.

2. En la parte superior de **main.py**, importe la biblioteca `json`.

    ```python
    import json
    ```

3. Agregue las variables `TEMPERATURE_THRESHOLD` y `TWIN_CALLBACKS` en los contadores globales. El umbral de temperatura establece el valor que debe superar la temperatura de la máquina para que los datos se envíen a IoT Hub.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

4. Reemplace la función `receive_message_callback` por el código siguiente:

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we will forward this message onto the "output1" queue.
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

5. Agregue una nueva función llamada `module_twin_callback`. Esta función se invocará cuando se actualicen las propiedades deseadas.

    ```python
    # module_twin_callback is invoked when twin's desired properties are updated.
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

6. En la clase `HubManager`, agregue una línea nueva al método `__init__` para inicializar la función `module_twin_callback` recién agregada.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Guarde este archivo.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior, creó una solución de IoT Edge y agregó código al módulo de Python que va a filtrar los mensajes en los que la temperatura registrada por la máquina está por debajo del umbral aceptable. Ahora, tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedor. 

1. Inicie sesión en Docker. Para ello, escriba el siguiente comando en el terminal integrado de Visual Studio Code a fin de que pueda insertar la imagen del módulo en el ACR: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilice el nombre de usuario, la contraseña y el servidor de inicio de sesión que copió de Azure Container Registry en la primera sección. También puede recuperarlos de nuevo en la sección **Claves de acceso** del registro mediante Azure Portal.

2. En el explorador de VS Code, abra el archivo **deployment.template.json** en el área de trabajo de la solución de IoT Edge. 

   Este archivo le dice a `$edgeAgent` que implemente dos módulos: **tempSensor**, que simula los datos del dispositivo, y **PythonModule**. El valor `PythonModule.image` se establece en una versión para Linux amd64 de la imagen. Para más información sobre los manifiestos de implementación, consulte esta [descripción acerca de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

   Este archivo también contiene las credenciales del registro. En el archivo de plantilla, el nombre de usuario y la contraseña contienen marcadores de posición. Cuando se genera el manifiesto de implementación, los campos se actualizan con los valores que se han agregado a **.env**. 

3. Agregue el módulo gemelo de PythonModule que se corresponde con el manifiesto de implementación. Inserte el siguiente contenido JSON en la parte inferior de la sección `moduleContent`, después del módulo gemelo `$edgeHub`: 
    ```json
        "PythonModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Guarde este archivo.

5. En el explorador de VS Code, haga clic con el botón derecho en el archivo **deployment.template.json** y seleccione **Build IoT Edge solution** (Compilar solución de IoT Edge). 

Cuando le indica a Visual Studio Code que compile la solución, esta herramienta primero toma la información de la plantilla de implementación y genera un archivo `deployment.json` en una nueva carpeta **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos compilan el código, empaquetan el código de Python en contenedores y lo insertan en el registro de contenedores que especificó cuando inicializó la solución. 

Puede ver la dirección completa de la imagen de contenedor con la etiqueta del comando `docker build` que se ejecuta en el terminal integrado de VS Code. La dirección de la imagen se crea a partir de la información del archivo `module.json`, con el formato **\<repositorio\>:\<versión\>-\<plataforma\>**. En este tutorial, debería ser similar a **registryname.azurecr.io/pythonmodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

Al igual que en las guías de inicio rápido, puede utilizar Azure Portal para implementar el módulo de Python en un dispositivo IoT Edge; sin embargo, también puede implementar y supervisar módulos desde Visual Studio Code. En las secciones siguientes, se usa la extensión Azure IoT Edge para VS Code que se indicó en los requisitos previos. Instálela ahora si aún no lo ha hecho. 

1. Para abrir la paleta de comandos de VS Code, seleccione **Ver** > **Paleta de comandos**.

2. Busque y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión). Siga las instrucciones para iniciar sesión en la cuenta de Azure. 

3. En la paleta de comandos, busque y ejecute el comando **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: seleccionar IoT Hub). 

4. Seleccione la suscripción que contiene la instancia de IoT Hub y, después, el centro de IoT al que desea acceder.

5. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

6. Haga clic con el botón derecho en el nombre del dispositivo IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear implementación para el dispositivo IoT Edge). 

7. Vaya a la carpeta de la solución que contiene PythonModule. Abra la carpeta **config** y seleccione el archivo **deployment.json**. Haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge).

8. Actualice la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Debería ver el nuevo **PythonModule** en ejecución junto con el módulo **TempSensor**, así como **$edgeAgent** y **$edgeHub**. 

## <a name="view-generated-data"></a>Consulta de los datos generados

1. Para supervisar los datos que llegan al centro de IoT, haga clic en **...** y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
2. Para supervisar el mensaje de D2C de un determinado dispositivo, haga clic con el botón derecho en un dispositivo de la lista y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
3. Para dejar de supervisar los datos, ejecute el comando **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: detener la supervisión del mensaje de D2C) en la paleta de comandos. 
4. Para ver o actualizar el módulo gemelo, haga clic con el botón derecho en el módulo de la lista y seleccione **Edit module twin** (Editar módulo gemelo). Para actualizar el módulo gemelo, guarde el archivo JSON gemelo y haga clic con el botón derecho en el área del editor y seleccione **Update Module Twin** (Actualizar módulo gemelo).
5. Para ver los registros de Docker, instale [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para VS Code y busque en el explorador de Docker los módulos que se ejecutan localmente. En el menú contextual, haga clic en **Show Logs** (Mostrar registros) en la vista del terminal integrado. 

## <a name="clean-up-resources"></a>Limpieza de recursos 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Si va a seguir con el siguiente artículo recomendado, puede conservar los recursos y las configuraciones que ya ha creado para volverlos a utilizar.

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

> [!IMPORTANT]
> La eliminación de los recursos de Azure y del grupo de recursos es un proceso irreversible. Una vez eliminados, el grupo de recursos y todos los recursos que contiene se suprimirán permanentemente. Tenga cuidado de no eliminar por accidente el grupo de recursos o los recursos equivocados. Si ha creado una instancia de IoT Hub en un grupo de recursos existente que contiene recursos que desea conservar, elimine exclusivamente el recurso de IoT Hub que desee en lugar de eliminar todo el grupo de recursos.
>

Para eliminar únicamente la instancia de IoT Hub, ejecute el siguiente comando utilizando el nombre del centro y el nombre del grupo de recursos:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Para eliminar todo el grupo de recursos utilizando su nombre:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Grupos de recursos**.

2. Escriba el nombre del grupo de recursos que contiene la instancia de IoT Hub en el cuadro de texto **Filtrar por nombre...** 

3. A la derecha del grupo de recursos de la lista de resultados, haga clic en **...** y después en **Eliminar grupo de recursos**.

4. Se le pedirá que confirme la eliminación del grupo de recursos. Escriba otra vez el nombre del grupo de recursos para confirmar y haga clic en **Eliminar**. Transcurridos unos instantes, el grupo de recursos y todos los recursos que contiene se eliminan.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo IoT Edge que contiene código para filtrar los datos sin procesar generados por el dispositivo IoT Edge. Puede continuar con cualquiera de los siguientes tutoriales para conocer otras formas en las que Azure IoT Edge puede ayudarle a transformar los datos perimetrales en conocimientos empresariales.

> [!div class="nextstepaction"]
> [Implementación de Azure Function como módulo](tutorial-deploy-function.md)
> [Implementación de Azure Stream Analytics como módulo](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
