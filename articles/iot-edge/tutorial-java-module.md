---
title: Tutorial de Java de Azure IoT Edge | Microsoft Docs
description: En este tutorial se muestra la creación de un módulo IoT Edge con código Java y su implementación en un dispositivo perimetral.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: d5201f3b2a0a3548b1f9eaf4a3f6c6b0fe160d18
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567713"
---
# <a name="tutorial-develop-a-java-iot-edge-module-and-deploy-to-your-simulated-device"></a>Tutorial: Desarrollo de un módulo IoT Edge con Java en el dispositivo simulado

Los módulos Azure IoT Edge se pueden usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos IoT Edge. En este tutorial, se detallan los pasos para crear e implementar un módulo de IoT Edge que filtra los datos de sensor. Utilizará el dispositivo de IoT Edge simulado que creó en las guías de inicio rápido para implementar Azure IoT Edge en un dispositivo simulado con [Windows](quickstart.md) o [Linux](quickstart-linux.md). En este tutorial, aprenderá a:    

> [!div class="checklist"]
> * Use Visual Studio Code para crear un módulo de Java de IoT Edge basado en el paquete de plantillas de Maven de Azure IoT Edge y el SDK de dispositivo de Java de Azure IoT.
> * Utilizar Visual Studio Code y Docker para crear una imagen de Docker y publicarla en el Registro.
> * Implementar el módulo en el dispositivo IoT Edge.
> * Ver datos generados.


El módulo IoT Edge que creó en este tutorial filtra lo datos sobre la temperatura generados por el dispositivo. Solo envía mensajes a los niveles superiores si la temperatura sobrepasa el umbral especificado. Este tipo de análisis perimetral resulta útil para reducir la cantidad de datos que se comunican a la nube y se almacenan en ella. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para [Linux](quickstart-linux.md).
* Los módulos de Java para IoT Edge no son compatibles con los dispositivos Windows.

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure. 

Recursos de desarrollo:

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Paquete de extensiones de Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) para Visual Studio Code.
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 
* [Java SE Development Kit 10](https://aka.ms/azure-jdks) y [establecer la `JAVA_HOME`variable de entorno](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) para que apunte a la instalación de JDK.
* [Maven](https://maven.apache.org/)
* [Docker CE](https://docs.docker.com/install/)
   * Si está desarrollando en un dispositivo Windows, asegúrese de que Docker esté [configurado para usar contenedores de Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). 


## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor
En este tutorial, puede usar la extensión de Azure IoT Edge para VS Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.  

En este tutorial, puede usar cualquier Registro compatible con Docker. Dos de los servicios de Docker Registry más populares que están disponibles en la nube son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Azure Container Registry**.
2. Asigne un nombre al Registro, seleccione una suscripción, elija un grupo de recursos y establezca la SKU en el nivel **Básico**. 
3. Seleccione **Crear**.
4. Una vez que se haya creado el Registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**. 
5. Establezca **Usuario administrador** en **Habilitar**.
6. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. Usará estos valores más adelante en este tutorial al publicar la imagen de Docker en el Registro y al agregar las credenciales del Registro al entorno de ejecución de Azure IoT Edge. 

## <a name="create-an-iot-edge-module-project"></a>Creación de un proyecto de módulo IoT Edge
En los siguientes pasos creará un proyecto de módulo IoT Edge basado en el paquete de plantillas de Maven de Azure IoT Edge y el SDK de dispositivo de Java de Azure IoT mediante Visual Studio Code y la extensión de Azure IoT Edge.

### <a name="create-a-new-solution"></a>Creación de una nueva solución

Cree una plantilla de solución de Java que pueda personalizar con su propio código. 

1. En Visual Studio Code, seleccione **Ver** > **Paleta de comandos** para abrir la paleta de comandos de VS Code. 

2. En la paleta de comandos, escriba el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge) y ejecútelo. En la paleta de comandos, proporcione la siguiente información para crear la solución: 

   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado: **EdgeSolution**.
   3. Elija el **módulo de Java** como la plantilla del módulo. 
   4. Proporcione un valor para groupId o acepte el valor predeterminado **com.edgemodule**.
   5. Reemplace el nombre de módulo predeterminado por **JavaModule**. 
   6. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión que copió. La cadena final se parece a \<nombre del registro\>.azurecr.io/javamodule.


Si es la primera vez que crea un módulo de Java, es posible que se tarden varios minutos en descargar los paquetes de Maven. El área de trabajo de la solución de IoT Edge se carga en la ventana de VS Code. El área de trabajo de la solución contiene cinco componentes de nivel superior. En este tutorial no se editarán la carpeta **\.vscode** ni el archivo **\.gitignore**. La carpeta **modules** contiene el código Java del módulo, así como archivos de Docker para compilar el módulo como una imagen de contenedor. El archivo **\.env** almacena las credenciales del registro de contenedor. El archivo **deployment.template.json** contiene la información que usa el entorno de ejecución de IoT Edge para implementar módulos en un dispositivo. 

Si no especificó un registro de contenedor al crear la solución y aceptó el valor predeterminado localhost:5000, no tendrá un archivo \.env. 

### <a name="add-your-registry-credentials"></a>Adición de las credenciales del Registro

El archivo del entorno almacena las credenciales del registro de contenedor y las comparte con el runtime de IoT Edge. El entorno de ejecución necesita estas credenciales para extraer las imágenes privadas e insertarlas en el dispositivo IoT Edge. 

1. En el explorador de VS Code, abra el archivo .env. 
2. Actualice los campos con los valores de **nombre de usuario** y **contraseña** que ha copiado del Registro de contenedor de Azure. 
3. Guarde este archivo. 

### <a name="update-the-module-with-custom-code"></a>Actualización del módulo con código personalizado

1. En el Explorador de VS Code, abra **modules** > **JavaModule** > **src** > **main** > **java** > **com** > **edgemodule** > **App.java**.

5. Agregue el código siguiente en la parte superior del archivo para importar nuevas clases a las que se hace referencia.

    ```java
    import java.io.StringReader;
    import java.util.concurrent.atomic.AtomicLong;
    import java.util.HashMap;
    import java.util.Map;
    
    import javax.json.Json;
    import javax.json.JsonObject;
    import javax.json.JsonReader;
    
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Pair;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Property;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.TwinPropertyCallBack;
    ```

5. Agregue la siguiente definición a la clase **App**. Esta variable establece el valor que debe superar la temperatura medida para que los datos se envíen al centro de IoT. 

    ```java
    private static final String TEMP_THRESHOLD = "TemperatureThreshold";
    private static AtomicLong tempThreshold = new AtomicLong(25);
    ```

7. Reemplace el método execute de **MessageCallbackMqtt** por el código siguiente. Se llama a este método cada vez que el módulo recibe un mensaje MQTT del centro de IoT Edge. Filtra los mensajes que informan de temperaturas por debajo del umbral de temperatura que se establecen mediante el módulo gemelo.

    ```java
        private int counter = 0;
       @Override
        public IotHubMessageResult execute(Message msg, Object context) {
            this.counter += 1;
 
            String msgString = new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET);
            System.out.println(
                   String.format("Received message %d: %s",
                            this.counter, msgString));
            if (context instanceof ModuleClient) {
                try (JsonReader jsonReader = Json.createReader(new StringReader(msgString))) {
                    final JsonObject msgObject = jsonReader.readObject();
                    double temperature = msgObject.getJsonObject("machine").getJsonNumber("temperature").doubleValue();
                    long threshold = App.tempThreshold.get();
                    if (temperature >= threshold) {
                        ModuleClient client = (ModuleClient) context;
                        System.out.println(
                            String.format("Temperature above threshold %d. Sending message: %s",
                            threshold, msgString));
                        client.sendEventAsync(msg, eventCallback, msg, App.OUTPUT_NAME);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            return IotHubMessageResult.COMPLETE;
        }
    ```

8. Agregue las dos clases internas estáticas siguientes a la clase **App**. Estas clases reciben actualizaciones sobre las propiedades que se quieren del módulo gemelo y actualiza la variable **tempThreshold** para que coincida. Todos los módulos tienen su propio módulo gemelo, que le permite configurar el código que se ejecuta dentro de un módulo directamente desde la nube.

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
 
    protected static class OnProperty implements TwinPropertyCallBack {
        @Override
        public void TwinPropertyCallBack(Property property, Object context) {
            if (!property.getIsReported()) {
                if (property.getKey().equals(App.TEMP_THRESHOLD)) {
                    try {
                        long threshold = Math.round((double) property.getValue());
                        App.tempThreshold.set(threshold);
                    } catch (Exception e) {
                        System.out.println("Faile to set TemperatureThread with exception");
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```

9. Agregue las líneas siguientes al método **main** después de **client.open()** para suscribirse a las actualizaciones del módulo gemelo.

    ```java
    client.startTwin(new DeviceTwinStatusCallBack(), null, new OnProperty(), null);
    Map<Property, Pair<TwinPropertyCallBack, Object>> onDesiredPropertyChange = new HashMap<Property, Pair<TwinPropertyCallBack, Object>>() {
        {
            put(new Property(App.TEMP_THRESHOLD, null), new Pair<TwinPropertyCallBack, Object>(new OnProperty(), null));
        }
    };
    client.subscribeToTwinDesiredProperties(onDesiredPropertyChange);
    client.getTwin();
    ```

11. Guarde este archivo.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior, creó una solución de IoT Edge y agregó código a **JavaModule** para filtrar los mensajes en los que la temperatura registrada por la máquina está por debajo del umbral aceptable. Ahora, tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedor. 

1. Escriba el comando siguiente en el terminal integrado de Visual Studio Code para iniciar sesión en Docker. A continuación, puede insertar la imagen del módulo en Azure Container Registry.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilice el nombre de usuario, la contraseña y el servidor de inicio de sesión que copió de Azure Container Registry en la primera sección. También puede recuperar estos valores en la sección **Claves de acceso** del registro en Azure Portal.

2. En el explorador de VS Code, abra el archivo deployment.template.json en el área de trabajo de la solución de IoT Edge. Este archivo indica a **$edgeAgent** que implemente los dos módulos: **tempSensor** y **JavaModule**. El valor **JavaModule.image** se establece en una versión para Linux amd64 de la imagen. Cambie la versión de imagen a **arm32v7** si esa es la arquitectura de su dispositivo de IoT Edge. 

   Compruebe que la plantilla tiene el nombre de módulo correcto, no el nombre **SampleModule** predeterminado que cambió cuando creó la solución de IoT Edge.

   Para más información sobre los manifiestos de implementación, consulte esta [descripción acerca de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

3. En el archivo deployment.template.json, la sección **registryCredentials** almacena las credenciales de Registro de Docker. Los pares reales de nombre de usuario y contraseña se almacenan en el archivo .env, que Git pasa por alto.  

4. Agregue el módulo gemelo **JavaModule** al manifiesto de implementación. Inserte el siguiente contenido JSON en la parte inferior de la sección **moduleContent**, después del módulo gemelo **$edgeHub**: 
    ```json
        "JavaModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Guarde este archivo.

5. En el explorador de VS Code, haga clic con el botón derecho en el archivo deployment.template.json y seleccione **Build and Push IoT Edge solution** (Compilar e insertar solución de IoT Edge). 

Cuando le indica a Visual Studio Code que compile la solución, esta herramienta primero toma la información de la plantilla de implementación y genera un archivo deployment.json en una nueva carpeta denominada **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos compilan el código, empaquetan en contenedores la aplicación Java e insertan el código en el registro de contenedor que especificó cuando inicializó la solución. 

Puede ver la dirección de la imagen de contenedor completa con la etiqueta en el terminal integrado de VS Code. La dirección de la imagen se crea a partir de la información del archivo module.json, con el formato \<repositorio\>:\<versión\>-\<plataforma\>. Para este tutorial, se debería parecer a registryname.azurecr.io/javamodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

En el artículo de la guía de inicio rápido que siguió para configurar el dispositivo de IoT Edge, implementó un módulo mediante Azure Portal. También puede implementar módulos con la extensión Azure IoT Toolkit para Visual Studio Code. Ya tiene un manifiesto de implementación preparado para su escenario, el archivo **deployment.json**. Ahora todo lo que necesita hacer es seleccionar un dispositivo que reciba la implementación.

1. En la paleta de comandos de VS Code, ejecute el comando **Azure: Sign in** y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.

2. En la paleta de comandos de VS Code, ejecute **Azure IoT Hub: Select IoT Hub**. 

3. Elija la suscripción y la instancia de IoT Hub que contienen el dispositivo IoT Edge que desea configurar. 

4. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

5. Haga clic con el botón derecho en el nombre del dispositivo IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear una implementación para un dispositivo individual). 

   ![Crear una implementación para un dispositivo individual](./media/tutorial-java-module/create-deployment.png)

6. Seleccione el archivo **deployment.json** de la carpeta **config** y, a continuación, haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge). No utilice el archivo deployment.template.json. 

7. Haga clic en el botón Actualizar. Debería ver el nuevo **JavaModule** que se ejecuta junto con el módulo **TempSensor**, **$edgeAgent** y **$edgeHub**.  

## <a name="view-generated-data"></a>Visualización de datos generados

Una vez aplicado el manifiesto de implementación al dispositivo de IoT Edge, el entorno de ejecución de IoT Edge del dispositivo recopila la información de implementación nueva y comienza a ejecutarse con ella. Los módulos que se ejecuten en el dispositivo y que no están incluidos en el manifiesto de implementación se detienen. Los módulos que falten en el dispositivo se inician. 

Puede ver el estado del dispositivo de IoT Edge con la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) del explorador de Visual Studio Code. Expanda los detalles del dispositivo para ver una lista de los módulos implementados y en ejecución. 

En el propio dispositivo de IoT Edge puede ver el estado de los módulos de implementación mediante el comando `iotedge list`. Debería ver cuatro módulos: los dos módulos personalizados del entorno de ejecución de IoT Edge, tempSensor y el sensor personalizado que ha creado en este tutorial. Los módulos pueden tardar unos minutos en comenzar; si no los ve inicialmente, vuelva a ejecutar el comando. 

Los mensajes que generan los módulos se ven con el comando `iotedge logs <module name>`. 

Puede ver los mensajes conforme llegan a IoT Hub mediante Visual Studio Code. 

1. Para supervisar los datos que llegan al centro de IoT, seleccione los puntos suspensivos (**...**) y, luego, **Start Monitoring D2C Messages** (Inicio de la supervisión de mensajes de D2C).
2. Para supervisar el mensaje de D2C de un determinado dispositivo, haga clic con el botón derecho en un dispositivo de la lista y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
3. Para dejar de supervisar los datos, ejecute el comando **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: detener la supervisión del mensaje de D2C) en la paleta de comandos. 
4. Para ver o actualizar el módulo gemelo, haga clic con el botón derecho en el módulo de la lista y seleccione **Edit module twin** (Editar módulo gemelo). Para actualizar el módulo gemelo, guarde el archivo JSON gemelo, haga clic con el botón derecho en el área del editor y seleccione **Update Module Twin** (Actualizar módulo gemelo).
5. Para ver los registros de Docker, instale [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para VS Code. Puede encontrar los módulos que se están ejecutando localmente en el explorador de Docker. En el menú contextual, seleccione **Show Logs** (Mostrar registros) en la vista del terminal integrado.
 
## <a name="clean-up-resources"></a>Limpieza de recursos 

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba. 

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo IoT Edge que contiene código para filtrar datos sin procesar generados por el dispositivo IoT Edge. Puede continuar con los siguientes tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Almacenamiento de datos en el perímetro con bases de datos de SQL Server](tutorial-store-data-sql-server.md)

