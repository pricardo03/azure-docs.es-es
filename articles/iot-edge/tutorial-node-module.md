---
title: Tutorial de Node.js de Azure IoT Edge | Microsoft Docs
description: En este tutorial se muestra cómo crear un módulo de IoT Edge con código de Node.js e implementarlo en un dispositivo perimetral.
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: cdcd30ea29c5c7066a6ae05f64b5bf0720572599
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061271"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-to-your-simulated-device"></a>Tutorial: Desarrollo e implementación de un módulo de Node.js de IoT Edge en su dispositivo simulado

Los módulos de IoT Edge se pueden usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos de IoT Edge. Este tutorial le guía a través de la creación e implementación de un módulo de IoT Edge que filtra los datos de sensor. Usará el dispositivo de IoT Edge simulado que ha creado en los tutoriales sobre cómo implementar Azure IoT Edge en un dispositivo simulado de [Windows] [ lnk-tutorial1-win] o [Linux] [ lnk-tutorial1-lin]. En este tutorial, aprenderá a:    

> [!div class="checklist"]
> * Uso de Visual Studio Code para crear un módulo de Node.js de IoT Edge
> * Usar Visual Studio Code y Docker para crear una imagen de Docker y publicarla en el Registro 
> * Implementar el módulo en el dispositivo IoT Edge
> * Visualización de datos generados


El módulo IoT Edge que se crea en este tutorial filtra los datos de temperatura generados por el dispositivo. Solo envía mensajes al siguiente nivel si la temperatura es superior al umbral especificado. Este tipo de análisis perimetral resulta útil para reducir la cantidad de datos que se comunican a la nube y se almacenan en ella. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de empezar.

## <a name="prerequisites"></a>requisitos previos

* El dispositivo Azure IoT Edge que creó en la guía de inicio rápido para [Linux](quickstart-linux.md) o [dispositivos Windows](quickstart.md).
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensión de Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker](https://docs.docker.com/engine/installation/) en el mismo equipo que tiene Visual Studio Code. La Community Edition (CE) es suficiente para este tutorial. 
* [Node.js y npm](https://nodejs.org) npm se distribuye con Node.js, lo que significa que cuando se descarga Node.js, automáticamente se instala npm en el equipo.

## <a name="create-a-container-registry"></a>Creación de un registro de contenedor
En este tutorial, puede usar la extensión de Azure IoT Edge para VS Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Inserte luego esta imagen en un **Registro** que almacena y administra las imágenes. Por último, implemente la imagen en el Registro para que se ejecute en el dispositivo de IoT Edge.  

Puede usar cualquier registro compatible con Docker para este tutorial. Dos servicios populares del Registro de Docker disponibles en la nube son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial se usa Azure Container Registry. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Azure Container Registry**.
2. Asigne un nombre al Registro, elija una suscripción, elija un grupo de recursos y establezca la SKU en el nivel **Básico**. 
3. Seleccione **Crear**.
4. Una vez creado el registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**. 
5. Cambie **Usuario administrador** a **Habilitar**.
6. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. Los usará más adelante en el tutorial. 

## <a name="create-an-iot-edge-module-project"></a>Crear un proyecto de módulo IoT Edge
En los siguientes pasos, puede ver cómo crear un módulo de Node.js de IoT Edge mediante Visual Studio Code y la extensión de Azure IoT Edge.

### <a name="create-a-new-solution"></a>Crear una solución nueva

Use **npm** para crear una plantilla de solución de Node.js que se puede compilar a partir de él. 

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado** para abrir el terminal integrado de VS Code.

2. En el terminal integrado, escriba el siguiente comando para instalar **yeoman** y el generador para el módulo de Node.js de Azure IoT Edge: 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. Seleccione **View** > **Command Palette** (Ver > Paleta de comandos) para abrir la paleta de comandos de VS Code. 

3. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión) y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.

4. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución IoT Edge). En la paleta de comandos, proporcione la siguiente información para crear la solución: 

   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado **EdgeSolution**.
   3. Elija **Módulo de Node.js** como la plantilla del módulo. 
   4. Llame al módulo **NodeModule**. 
   5. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión que copió. La cadena final se parece a **\<nombre del registro\>.azurecr.io/nodemodule**.
 
La ventana de Visual Studio Code carga el área de trabajo de la solución de IoT Edge. Hay una carpeta **.vscode**, una carpeta de **módulos**, un archivo **.env** y un archivo de plantilla de manifiesto de implementación.

### <a name="add-your-registry-credentials"></a>Adición de las credenciales del registro

El archivo de entorno almacena las credenciales del repositorio de contenedores y las comparte con el entorno de ejecución de IoT Edge. El entorno de ejecución necesita estas credenciales para extraer sus imágenes privadas al dispositivo IoT Edge. 

1. En el explorador de VS Code, abra el archivo **.env**. 
2. Actualice los campos con los valores de **nombre de usuario** y **contraseña** que ha copiado del registro de contenedor de Azure. 
3. Guarde este archivo. 

### <a name="update-the-module-with-custom-code"></a>Actualización del módulo con código personalizado

Cada plantilla viene con un código de ejemplo incluido, que toma los datos del sensor simulado del módulo **tempSensor** y los encamina a la instancia de IoT Hub. En esta sección, agregue código para que NodeModule analice los mensajes antes de enviarlos. 

1. En el explorador de VS Code, abra **módulos** > **NodeModule** > **app.js**.

5. Agregue una variable de umbral de temperatura por debajo de los módulos de nodo necesarios. El umbral de temperatura establece el valor que debe superar la temperatura medida para que los datos se envíen a IoT Hub.

    ```javascript
    var temperatureThreshold = 30;
    ```

6. Reemplace la función `PipeMessage` por la función `FilterMessage`.
    
    ```javascript
    // This function filters out messages that report temperatures below the temperature threshold.
    // It also adds the MessageType property to the message with the value set to Alert.
    function filterMessage(client, inputName, msg) {
        client.complete(msg, printResultFor('Receiving message'));
        if (inputName === 'input1') {
            var message = msg.getBytes().toString('utf8');
            var messageBody = JSON.parse(message);
            if (messageBody && messageBody.machine && messageBody.machine.temperature && messageBody.machine.temperature > temperatureThreshold) {
                console.log(`Machine temperature ${messageBody.machine.temperature} exceeds threshold ${temperatureThreshold}`);
                var outputMsg = new Message(message);
                outputMsg.properties.add('MessageType', 'Alert');
                client.sendOutputEvent('output1', outputMsg, printResultFor('Sending received message'));
            }
        }
    }

    ```

7. Reemplace el nombre de la función `pipeMessage` por `filterMessage` en la función `client.on()`.

    ```javascript
    client.on('inputMessage', function (inputName, msg) {
        filterMessage(client, inputName, msg);
        });
    ```

8. Copie el siguiente fragmento de código en la función de devolución de llamada `client.open()`, después `client.on()` dentro de la instrucción `else`. Esta función se invoca cuando se actualicen las propiedades deseadas.

    ```javascript
    client.getTwin(function (err, twin) {
        if (err) {
            console.error('Error getting twin: ' + err.message);
        } else {
            twin.on('properties.desired', function(delta) {
                if (delta.TemperatureThreshold) {
                    temperatureThreshold = delta.TemperatureThreshold;
                }
            });
        }
    });
    ```

9. Guarde este archivo.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior se ha creado una solución de IoT Edge y se ha agregado código a NodeModule que va a filtrar los mensajes donde la temperatura de la máquina informada está por debajo del umbral aceptable. Ahora tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedores. 

1. Inicie sesión en Docker escribiendo el siguiente comando en el terminal integrado de Visual Studio Code, para que pueda insertar la imagen del módulo en el ACR: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilice el nombre de usuario, la contraseña y el servidor de inicio de sesión que ha copiado de Azure Container Registry en la primera sección. O recupérelos de nuevo desde la sección **Claves de acceso** del registro en Azure Portal.

2. En el explorador de VS Code, abra el archivo **deployment.template.json** en el área de trabajo de la solución de IoT Edge. 

   Este archivo indica `$edgeAgent` para implementar dos módulos: **tempSensor**, que simula los datos del dispositivo, y **NodeModule**. El valor `NodeModule.image` se establece en una versión Linux amd64 de la imagen. Para aprender más sobre manifiestos de implementación, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

   Este archivo también contiene las credenciales del registro. En el archivo de plantilla, el nombre de usuario y la contraseña se rellenan con los marcadores de posición. Al generar el manifiesto de implementación, los campos se actualizan con los valores que se ha agregado a **.env**. 

4. Agregue el módulo gemelo NodeModule al manifiesto de implementación. Inserte el siguiente contenido JSON en la parte inferior de la sección `moduleContent`, después del módulo gemelo `$edgeHub`: 
    ```json
        "NodeModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```
5. Guarde este archivo.
6. En el explorador de VS Code, haga clic con el botón derecho en el archivo **deployment.template.json** y seleccione **Build IoT Edge solution** (Compilar solución de IoT Edge). 

Cuando indica a Visual Studio Code que cree su solución, primero toma la información de la plantilla de implementación y genera un archivo `deployment.json` en una nueva carpeta **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos construyen el código, empaquetan el código de Node.js en contenedores y lo insertan en el registro de contenedores que ha especificado cuando inicializó la solución. 

Puede ver la dirección de la imagen de contenedor completa con la etiqueta en el comando `docker build` que se ejecuta en la terminal integrada de VS Code. La dirección de la imagen se crea a partir de la información del archivo `module.json`, con el formato **\<repositorio\>:\<versión\>-\<plataforma\>**. Para este tutorial, debería ser similar a **registryname.azurecr.io/nodemodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

Podría usar Azure Portal para implementar el módulo de Node.js en un dispositivo IoT Edge se ha hecho en las guías de inicio rápido, pero también puedes implementar y supervisar módulos desde Visual Studio Code. En las secciones siguientes se usa la extensión Azure IoT Edge para VS Code que se enumeró en los requisitos previos. Instálela ahora si aún no lo ha hecho. 

1. Para abrir la paleta de comandos de VS Code, seleccione **View** > **Command Palette** (Ver > Paleta de comandos).

2. Busque y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión). Siga las instrucciones para iniciar sesión en la cuenta de Azure. 

3. En la paleta de comandos, busque y ejecute el comando **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: seleccionar IoT Hub). 

4. Seleccione la suscripción que contiene su centro de IoT y, después, seleccione el centro de IoT al que desea acceder.

5. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

6. Haga clic con el botón derecho en el nombre del dispositivo IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear implementación para el dispositivo IoT Edge). 

7. Vaya a la carpeta de solución que contiene NodeModule. Abra la carpeta **config** y seleccione el archivo **deployment.json**. Haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge).

8. Actualice la sección de **dispositivos de Azure IoT Hub**. Debería ver el nuevo **NodeModule** en ejecución junto con el módulo **TempSensor** y el **$edgeAgent** y **$edgeHub**. 


## <a name="view-generated-data"></a>Visualización de datos generados

1. Para supervisar los datos que llegan al centro de IoT, haga clic en **...** y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
2. Para supervisar el mensaje de D2C para un dispositivo específico, haga clic con el botón derecho en el dispositivo en la lista y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
3. Para dejar de supervisar datos, use el comando **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: detener la supervisión del mensaje de D2C) en la paleta de comandos. 
4. Para ver o actualizar el módulo gemelo, haga clic con el botón derecho en el módulo en la lista y seleccione **Edit module twin** (Editar módulo gemelo). Para actualizar el módulo gemelo, guarde el archivo JSON gemelo y haga clic con el botón derecho en el área del editor y seleccione **Update Module Twin** (Actualizar módulo gemelo).
5. Para ver los registros de Docker, instale [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para VS Code y encuentre los módulos en ejecución de manera local en el explorador de Docker. En el menú contextual, haga clic en **Show Logs** (Mostrar registros) en la vista del terminal integrado. 

## <a name="clean-up-resources"></a>Limpieza de recursos 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Si va a seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar.

En caso contrario, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo para evitar gastos. 

> [!IMPORTANT]
> La eliminación de los recursos de Azure y del grupo de recursos es un proceso irreversible. Una vez eliminados, el grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. Si ha creado una instancia de IoT Hub en un grupo de recursos ya existente que contiene recursos que desea conservar, puede eliminar solo esa instancia en lugar de eliminar todo el grupo de recursos.
>

Para eliminar solo la instancia de IoT Hub, ejecute el siguiente comando usando el nombre del centro y el nombre del grupo de recursos:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Para eliminar un grupo de recursos entero por el nombre:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com) y haga clic en **Grupos de recursos**.

2. Escriba el nombre del grupo de recursos que contiene la instancia de IoT Hub en el cuadro de texto **Filtrar por nombre...**. 

3. A la derecha del grupo de recursos de la lista de resultados, haga clic en **...** y, a continuación, en **Eliminar grupo de recursos**.

4. Se le pedirá que confirme la eliminación del grupo de recursos. Escriba otra vez el nombre del grupo de recursos para confirmar y haga clic en **Eliminar**. Transcurridos unos instantes, el grupo de recursos y todos los recursos que contiene se eliminan.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, creó un módulo IoT Edge que contiene código para filtrar datos sin procesar generados por el dispositivo IoT Edge. Puede continuar con cualquiera de los siguientes tutoriales para obtener información sobre otras formas en que Azure IoT Edge puede ayudarle a transformar datos en perspectivas empresariales en el límite.

> [!div class="nextstepaction"]
> [Deploy Azure Function as a module (Implementar Azure Functions como módulo)](tutorial-deploy-function.md)
> [Deploy Azure Stream Analytics as a module (Implementar Azure Stream Analytics como módulo)](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
