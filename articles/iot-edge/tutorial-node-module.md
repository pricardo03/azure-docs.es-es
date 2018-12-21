---
title: 'Tutorial: Creación del módulo personalizado de Node.js: Azure IoT Edge | Microsoft Docs'
description: En este tutorial se muestra cómo crear un módulo de IoT Edge con código de Node.js e implementarlo en un dispositivo perimetral.
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 11/25/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 76bc59b41bf35f6427b1dcb273f3f50c5e70b748
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344064"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-to-your-simulated-device"></a>Tutorial: Desarrollo e implementación de un módulo de Node.js de IoT Edge en su dispositivo simulado

Los módulos de IoT Edge se pueden usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos de IoT Edge. En este tutorial, se detallan los pasos para crear e implementar un módulo de IoT Edge que filtra los datos de sensor. Utilizará el dispositivo de IoT Edge simulado que creó en las guías de inicio rápido. En este tutorial, aprenderá a:    

> [!div class="checklist"]
> * Uso de Visual Studio Code para crear un módulo de Node.js de IoT Edge
> * Uso de Visual Studio Code y Docker para crear una imagen de Docker y publicarla en el Registro 
> * Implementación del módulo en el dispositivo IoT Edge
> * Consulta de los datos generados


El módulo IoT Edge que creó en este tutorial filtra lo datos sobre la temperatura generados por el dispositivo. Solo envía mensajes a los niveles superiores si la temperatura sobrepasa el umbral especificado. Este tipo de análisis perimetral resulta útil para reducir la cantidad de datos que se comunican a la nube y se almacenan en ella. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para dispositivos de [Linux](quickstart-linux.md) o de [Windows](quickstart.md).

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure. 

Recursos de desarrollo:

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 
* [Docker CE](https://docs.docker.com/engine/installation/). 
* [Node.js y npm](https://nodejs.org) El paquete npm se distribuye con Node.js, lo que significa que cuando se descarga Node.js, automáticamente se instala npm en el equipo.

## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor

En este tutorial, puede usar la extensión de Azure IoT Edge para Visual Studio Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.  

Puede usar cualquier registro compatible con Docker para almacenar las imágenes de contenedor. Dos de los servicios de registro de Docker más conocidos son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry. 

Si no dispone de un registro de contenedores, siga estos pasos para crear uno nuevo en Azure:

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Container Registry**.

2. Especifique los siguientes valores para crear un registro de contenedor:

   | Campo | Valor | 
   | ----- | ----- |
   | Nombre de registro | Especifique un nombre único. |
   | Subscription | Seleccione una suscripción en la lista desplegable. |
   | Grupos de recursos | Se recomienda usar el mismo grupo de recursos para todos los recursos de prueba que se crean en las guías de inicio rápido y los tutoriales de IoT Edge. Por ejemplo, **IoTEdgeResources**. |
   | Ubicación | Elija una ubicación cercana a usted. |
   | Usuario administrador | Seleccione **Habilitar**. |
   | SKU | Seleccione **Básica**. | **Terminal**

5. Seleccione **Crear**.

6. Una vez que se haya creado el Registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**. 

7. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. Utilice estos valores más adelante en el tutorial para proporcionar acceso al registro de contenedor. 

## <a name="create-an-iot-edge-module-project"></a>Creación de un proyecto de módulo IoT Edge
En los siguientes pasos, puede ver cómo crear un módulo de Node.js de IoT Edge mediante Visual Studio Code y la extensión de Azure IoT Edge.

### <a name="create-a-new-solution"></a>Creación de una nueva solución

Use **npm** para crear una plantilla de solución de Node.js que se puede compilar a partir de él. 

1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado** para abrir el terminal integrado de VS Code.

2. En el terminal integrado, escriba el siguiente comando para instalar **yeoman** y el generador para el módulo de Node.js de Azure IoT Edge: 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. Seleccione **Ver** > **Paleta de comandos** para abrir la paleta de comandos de VS Code. 

3. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión) y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.

4. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Nueva solución de IoT Edge). Para crear la solución, siga las indicaciones de la paleta de comandos.

   | Campo | Valor |
   | ----- | ----- |
   | Seleccionar carpeta | Elija la ubicación en el equipo de desarrollo en la que VS Code creará los archivos de la solución. |
   | Proporcionar un nombre de la solución | Escriba un nombre descriptivo para la solución o acepte el valor predeterminado **EdgeSolution**. |
   | Seleccionar plantilla del módulo | Elija **Módulo Node.js**. |
   | Proporcionar un nombre de módulo | Llame al módulo **NodeModule**. |
   | Proporcionar repositorio de imágenes de Docker del módulo | Un repositorio de imágenes incluye el nombre del registro de contenedor y el nombre de la imagen de contenedor. La imagen de contenedor se rellena previamente a partir del último paso. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión del registro de contenedor de Azure. Puede recuperar el servidor de inicio de sesión de la página de información general del registro de contenedor en Azure Portal. La cadena final se parece a \<nombre del registro\>.azurecr.io/nodemodule. |
 
   ![Especificación del repositorio de imágenes de Docker](./media/tutorial-node-module/repository.png)

El área de trabajo de la solución de IoT Edge se carga en la ventana de Visual Studio Code. El área de trabajo de la solución contiene cinco componentes de nivel superior. La carpeta **modules** contiene el código de Node.js del módulo, así como archivos Dockerfile para compilar el módulo como una imagen de contenedor. El archivo **\.env** almacena las credenciales del registro de contenedor. El archivo **deployment.template.json** contiene la información que usa el entorno de ejecución de IoT Edge para implementar módulos en un dispositivo. Y el archivo **deployment.debug.template.json** contiene la versión de depuración de los módulos. En este tutorial no se editarán la carpeta **\.vscode** ni el archivo **\.gitignore**. 

Si no especificó un registro de contenedor al crear la solución y aceptó el valor predeterminado localhost:5000, no tendrá un archivo \.env. 

<!--
   ![Node.js solution workspace](./media/tutorial-node-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Adición de las credenciales del Registro

El archivo del entorno almacena las credenciales del repositorio de contenedores y las comparte con el entorno de ejecución de IoT Edge. El entorno de ejecución necesita estas credenciales para extraer las imágenes privadas e insertarlas en el dispositivo IoT Edge. 

1. En el explorador de VS Code, abra el archivo **.env**. 
2. Actualice los campos con los valores de **nombre de usuario** y **contraseña** que ha copiado del Registro de contenedor de Azure. 
3. Guarde este archivo. 

### <a name="update-the-module-with-custom-code"></a>Actualización del módulo con código personalizado

Cada plantilla viene con un código de ejemplo incluido, que toma los datos del sensor simulado del módulo **tempSensor** y los enruta a IoT Hub. En esta sección, agregue código para que NodeModule analice los mensajes antes de enviarlos. 

1. En el explorador de VS Code, abra **módulos** > **NodeModule** > **app.js**.

5. Agregue una variable de umbral de temperatura por debajo de los módulos de nodo necesarios. El umbral de temperatura establece el valor que debe superar la temperatura medida para que los datos se envíen a IoT Hub.

    ```javascript
    var temperatureThreshold = 25;
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

10. En el explorador de VS Code, abra el archivo **deployment.template.json** en el área de trabajo de la solución de IoT Edge. 

   Este archivo indica `$edgeAgent` para implementar dos módulos: **tempSensor**, que simula los datos del dispositivo, y **NodeModule**. La plataforma predeterminada de la instancia de IoT Edge está establecida en **amd64** en la barra de estado de VS Code, lo que significa que **NodeModule** está establecido en la versión Linux amd64 de la imagen. Cambie la plataforma predeterminada en la barra de estado de **amd64** a **arm32v7** o **windows-amd64** si esa es la arquitectura del dispositivo de IoT Edge. Para más información sobre los manifiestos de implementación, consulte esta [descripción acerca de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md). 

   Este archivo también contiene las credenciales del registro. En el archivo de plantilla, el nombre de usuario y la contraseña contienen marcadores de posición. Al generar el manifiesto de implementación, los campos se actualizan con los valores que se ha agregado a **.env**. 

12. Agregue el módulo gemelo NodeModule al manifiesto de implementación. Inserte el siguiente contenido JSON en la parte inferior de la sección `moduleContent`, después del módulo gemelo `$edgeHub`: 

   ```json
       "NodeModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Adición de un módulo gemelo a una plantilla de implementación](./media/tutorial-node-module/module-twin.png)

13. Guarde este archivo.


## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior se ha creado una solución de IoT Edge y se ha agregado código a NodeModule que va a filtrar los mensajes donde la temperatura de la máquina informada está por debajo del umbral aceptable. Ahora tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedores. 

1. Inicie sesión en Docker. Para ello, escriba el siguiente comando en el terminal integrado de Visual Studio Code a fin de que pueda insertar la imagen del módulo en el ACR: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilice el nombre de usuario, la contraseña y el servidor de inicio de sesión que copió de Azure Container Registry en la primera sección. También puede recuperarlos de nuevo en la sección **Claves de acceso** del registro mediante Azure Portal.

2. En el explorador de VS Code, haga clic con el botón derecho en el archivo **deployment.template.json** y seleccione **Build and Push IoT Edge solution** (Compilar e insertar solución de IoT Edge). 

Cuando le indica a Visual Studio Code que compile la solución, esta herramienta primero toma la información de la plantilla de implementación y genera un archivo `deployment.json` en una nueva carpeta **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos construyen el código, empaquetan el código de Node.js en contenedores y lo insertan en el registro de contenedores que ha especificado cuando inicializó la solución. 

Puede ver la dirección de la imagen de contenedor completa con la etiqueta en el comando `docker build` que se ejecuta en la terminal integrada de VS Code. La dirección de la imagen se crea a partir de la información del archivo `module.json`, con el formato **\<repositorio\>:\<versión\>-\<plataforma\>**. Para este tutorial, debería ser similar a **registryname.azurecr.io/nodemodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

En el artículo de la guía de inicio rápido que siguió para configurar el dispositivo de IoT Edge, implementó un módulo mediante Azure Portal. También puede implementar módulos con la extensión Azure IoT Hub Toolkit (anteriormente, extensión Azure IoT Toolkit) para Visual Studio Code. Ya tiene un manifiesto de implementación preparado para su escenario, el archivo **deployment.json**. Ahora todo lo que necesita hacer es seleccionar un dispositivo que reciba la implementación.

1. En la paleta de comandos de VS Code, ejecute **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: seleccione IoT Hub). 

2. Elija la suscripción y la instancia de IoT Hub que contienen el dispositivo IoT Edge que desea configurar. 

3. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

4. Haga clic con el botón derecho en el nombre del dispositivo IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear una implementación para un dispositivo individual). 

   ![Crear una implementación para un dispositivo individual](./media/tutorial-node-module/create-deployment.png)

5. Seleccione el archivo **deployment.json** de la carpeta **config** y, a continuación, haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge). No utilice el archivo deployment.template.json. 

6. Haga clic en el botón Actualizar. Debería ver el nuevo **NodeModule** en ejecución junto con el módulo **TempSensor** y el **$edgeAgent** y **$edgeHub**. 


## <a name="view-generated-data"></a>Visualización de datos generados

Una vez aplicado el manifiesto de implementación al dispositivo de IoT Edge, el entorno de ejecución de IoT Edge del dispositivo recopila la información de implementación nueva y comienza a ejecutarse con ella. Los módulos que se ejecuten en el dispositivo y que no están incluidos en el manifiesto de implementación se detienen. Los módulos que falten en el dispositivo se inician. 

Puede ver el estado del dispositivo de IoT Edge con la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) del explorador de Visual Studio Code. Expanda los detalles del dispositivo para ver una lista de los módulos implementados y en ejecución. 

En el propio dispositivo de IoT Edge puede ver el estado de los módulos de implementación mediante el comando `iotedge list`. Debería ver cuatro módulos: los dos módulos personalizados del entorno de ejecución de IoT Edge, tempSensor y el sensor personalizado que ha creado en este tutorial. Los módulos pueden tardar unos minutos en comenzar; si no los ve inicialmente, vuelva a ejecutar el comando. 

Los mensajes que generan los módulos se ven con el comando `iotedge logs <module name>`. 

Puede ver los mensajes conforme llegan a IoT Hub mediante Visual Studio Code. 

1. Para supervisar los datos que llegan al centro de IoT, haga clic en **...** y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
2. Para supervisar el mensaje de D2C de un determinado dispositivo, haga clic con el botón derecho en un dispositivo de la lista y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
3. Para detener la supervisión de datos, ejecute el comando **Azure IoT Hub: Stop Monitoring D2C Message** (Detener supervisión de mensaje de D2C) en la paleta de comandos. 
4. Para ver o actualizar el módulo gemelo, haga clic con el botón derecho en el módulo de la lista y seleccione **Edit module twin** (Editar módulo gemelo). Para actualizar el módulo gemelo, guarde el archivo JSON gemelo y haga clic con el botón derecho en el área del editor y seleccione **Update Module Twin** (Actualizar módulo gemelo).
5. Para ver los registros de Docker, instale [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para VS Code y busque en el explorador de Docker los módulos que se ejecutan localmente. En el menú contextual, haga clic en **Show Logs** (Mostrar registros) en la vista del terminal integrado. 

## <a name="clean-up-resources"></a>Limpieza de recursos 

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba. 

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo IoT Edge que contiene código para filtrar los datos sin procesar generados por el dispositivo IoT Edge. Puede continuar con cualquiera de los siguientes tutoriales para conocer otras formas en las que Azure IoT Edge puede ayudarle a transformar los datos perimetrales en conocimientos empresariales.

> [!div class="nextstepaction"]
> [Implementación de Azure Function como módulo](tutorial-deploy-function.md)
> [Implementación de Azure Stream Analytics como módulo](tutorial-deploy-stream-analytics.md)

