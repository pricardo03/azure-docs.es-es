---
title: Tutorial de C de Azure IoT Edge | Microsoft Docs
description: En este tutorial se muestra la creación de un módulo IoT Edge con código C y su implementación en un dispositivo perimetral
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 07/30/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: c9d1931f1b78bb19f5e321a19baca45265ea7ab4
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413168"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Tutorial: Desarrollo de un módulo IoT Edge con C en el dispositivo simulado

Los módulos de IoT Edge se pueden usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos de IoT Edge. En este tutorial, se detallan los pasos para crear e implementar un módulo de IoT Edge que filtra los datos de sensor. En este tutorial, aprenderá a:    

> [!div class="checklist"]
> * Usar Visual Studio Code para crear un módulo de IoT Edge en C.
> * Utilizar Visual Studio Code y Docker para crear una imagen de Docker y publicarla en un contenedor de registro. 
> * Implementación del módulo en el dispositivo IoT Edge
> * Consulta de los datos generados


El módulo IoT Edge que creó en este tutorial filtra lo datos sobre la temperatura generados por el dispositivo. Solo envía mensajes a los niveles superiores si la temperatura sobrepasa el umbral especificado. Este tipo de análisis perimetral resulta útil para reducir la cantidad de datos que se comunican a la nube y se almacenan en ella. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para dispositivos de [Linux](quickstart-linux.md) o de [Windows](quickstart.md).

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar en Azure. 

Recursos de desarrollo:

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) para Visual Studio Code.
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code.
* [Docker CE](https://docs.docker.com/install/). 


## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor
En este tutorial, puede usar la extensión de Azure IoT Edge para VS Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.  

En este tutorial, puede usar cualquier Registro compatible con Docker. Dos de los servicios de Registro de Docker más populares que están disponibles en la nube son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Azure Container Registry**.
2. Asigne un nombre al Registro, seleccione una suscripción, elija un grupo de recursos y establezca la SKU en el nivel **Básico**. 
3. Seleccione **Crear**.
4. Una vez que se haya creado el Registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**. 
5. Establezca **Usuario administrador** en **Habilitar**.
6. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. Usará estos valores más adelante en este tutorial al publicar la imagen de Docker en el registro y al agregar las credenciales del registro al tiempo de ejecución de Edge. 

## <a name="create-an-iot-edge-module-project"></a>Creación de un proyecto de módulo IoT Edge
En los siguientes pasos puede ver cómo crear un proyecto de módulo IoT Edge basado en .NET Core 2.0 mediante Visual Studio Code y la extensión de Azure IoT Edge.
1. En Visual Studio Code, seleccione **Ver** > **Terminal integrado** para abrir el terminal integrado de VS Code.
2. Seleccione **Ver** > **Paleta de comandos** para abrir la paleta de comandos de VS Code. 
3. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión) y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.
4. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución IoT Edge). En la paleta de comandos, proporcione la siguiente información para crear la solución: 
   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado: **EdgeSolution**.
   3. Elija el **módulo de C** como la plantilla del módulo. 
   4. Llame al módulo **CModule**. 
   5. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión que copió. La cadena final se parece a **\<nombre del registro\>.azurecr.io/cmodule**.
 
4. El área de trabajo de la solución de IoT Edge se carga en la ventana de Visual Studio Code. Hay una carpeta **.vscode**, una carpeta **modules**, un archivo de plantilla de manifiesto de implementación y un archivo **.env**. El código del módulo predeterminado se implementa como un módulo de canalización. 

5. Para filtrar los mensajes en formato JSON, es necesario importar una biblioteca JSON para C. Puede elegir cualquier biblioteca JSON o escribir la suya propia para analizar JSON en el módulo de C. Los pasos siguientes usan [Parson](https://github.com/kgabis/parson) como ejemplo.
   1. Descargue **parson.c** y **parson.h** desde el [repositorio de Parson de GitHub](https://github.com/kgabis/parson). Y copie y pegue estos dos archivos en la carpeta **CModule**.
   2. Abra los **módulos** > **CModule** > **CMakeLists.txt**. Agregue las siguientes líneas para importar la biblioteca de Parson como my_parson.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. En `target_link_libraries`, en **CMakeLists.txt**, agregue `my_parson` en este.
   4. Abra los **módulos** > **CModule** > **main.c**. En la parte inferior de la sección include, agregue el código siguiente para incluir `parson.h` para la compatibilidad con JSON:

      ```c
      #include "parson.h"
      ```

6. Agregue una variable global `temperatureThreshold` después de la sección include. Esta variable establece el valor que debe superar la temperatura medida para que los datos se envíen a IoT Hub. 

    ```c
    static double temperatureThreshold = 25;
    ```

7. Reemplace la función `CreateMessageInstance` por el siguiente código. Esta función asigna un contexto para la devolución de llamada. 

    ```c
    static MESSAGE_INSTANCE* CreateMessageInstance(IOTHUB_MESSAGE_HANDLE message)
    {
        MESSAGE_INSTANCE* messageInstance = (MESSAGE_INSTANCE*)malloc(sizeof(MESSAGE_INSTANCE));
        if (NULL == messageInstance)
        {
            printf("Failed allocating 'MESSAGE_INSTANCE' for pipelined message\r\n");
        }
        else
        {
            memset(messageInstance, 0, sizeof(*messageInstance));

            if ((messageInstance->messageHandle = IoTHubMessage_Clone(message)) == NULL)
            {
                free(messageInstance);
                messageInstance = NULL;
            }
            else
            {
                messageInstance->messageTrackingId = messagesReceivedByInput1Queue;
                MAP_HANDLE propMap = IoTHubMessage_Properties(messageInstance->messageHandle);
                if (Map_AddOrUpdate(propMap, "MessageType", "Alert") != MAP_OK)
                {
                    printf("ERROR: Map_AddOrUpdate Failed!\r\n");
                }
            }
        }

        return messageInstance;
    }
    ```

8. Reemplace la función `InputQueue1Callback` por el siguiente código. Esta función implementa el filtro de mensajería real. 

    ```c
    static IOTHUBMESSAGE_DISPOSITION_RESULT InputQueue1Callback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
        IOTHUBMESSAGE_DISPOSITION_RESULT result;
        IOTHUB_CLIENT_RESULT clientResult;
        IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle = (IOTHUB_MODULE_CLIENT_LL_HANDLE)userContextCallback;

        unsigned const char* messageBody;
        size_t contentSize;

        if (IoTHubMessage_GetByteArray(message, &messageBody, &contentSize) != IOTHUB_MESSAGE_OK)
        {
            messageBody = "<null>";
        }

        printf("Received Message [%zu]\r\n Data: [%s]\r\n", 
                messagesReceivedByInput1Queue, messageBody);

        JSON_Value *root_value = json_parse_string(messageBody);
        JSON_Object *root_object = json_value_get_object(root_value);
        double temperature;
        if (json_object_dotget_value(root_object, "machine.temperature") != NULL && (temperature = json_object_dotget_number(root_object, "machine.temperature")) > temperatureThreshold)
        {
            printf("Machine temperature %f exceeds threshold %f\r\n", temperature, temperatureThreshold);
            // This message should be sent to next stop in the pipeline, namely "output1".  What happens at "outpu1" is determined
            // by the configuration of the Edge routing table setup.
            MESSAGE_INSTANCE *messageInstance = CreateMessageInstance(message);
            if (NULL == messageInstance)
            {
                result = IOTHUBMESSAGE_ABANDONED;
            }
            else
            {
                printf("Sending message (%zu) to the next stage in pipeline\n", messagesReceivedByInput1Queue);

                clientResult = IoTHubModuleClient_LL_SendEventToOutputAsync(iotHubModuleClientHandle, messageInstance->messageHandle, "output1", SendConfirmationCallback, (void *)messageInstance);
                if (clientResult != IOTHUB_CLIENT_OK)
                {
                    IoTHubMessage_Destroy(messageInstance->messageHandle);
                    free(messageInstance);
                    printf("IoTHubModuleClient_LL_SendEventToOutputAsync failed on sending msg#=%zu, err=%d\n", messagesReceivedByInput1Queue, clientResult);
                    result = IOTHUBMESSAGE_ABANDONED;
                }
                else
                {
                    result = IOTHUBMESSAGE_ACCEPTED;
                }
            }
        }
        else
        {
            printf("Not sending message (%zu) to the next stage in pipeline.\r\n", messagesReceivedByInput1Queue);
            result = IOTHUBMESSAGE_ACCEPTED;
        }

        messagesReceivedByInput1Queue++;
        return result;
    }
    ```

9. Agregue una función `moduleTwinCallback`. Este método recibe las actualizaciones sobre las propiedades que se quieren del módulo gemelo y actualiza la variable **temperatureThreshold** para que coincida. Todos los módulos tienen su propio módulo gemelo, que le permite configurar el código que se ejecuta dentro de un módulo directamente desde la nube.

    ```c
    static void moduleTwinCallback(DEVICE_TWIN_UPDATE_STATE update_state, const unsigned char* payLoad, size_t size, void* userContextCallback)
    {
        printf("\r\nTwin callback called with (state=%s, size=%zu):\r\n%s\r\n", 
            ENUM_TO_STRING(DEVICE_TWIN_UPDATE_STATE, update_state), size, payLoad);
        JSON_Value *root_value = json_parse_string(payLoad);
        JSON_Object *root_object = json_value_get_object(root_value);
        if (json_object_dotget_value(root_object, "desired.TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_dotget_number(root_object, "desired.TemperatureThreshold");
        }
        if (json_object_get_value(root_object, "TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_get_number(root_object, "TemperatureThreshold");
        }
    }
    ```

10. Agregue una nueva devolución de llamada para `moduleTwinCallback` en la función `SetupCallbacksForModule`. Reemplace la función `SetupCallbacksForModule` por el siguiente código.

    ```c
    static int SetupCallbacksForModule(IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle)
    {
        int ret;

        if (IoTHubModuleClient_LL_SetInputMessageCallback(iotHubModuleClientHandle, "input1", InputQueue1Callback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
        {
            printf("ERROR: IoTHubModuleClient_LL_SetInputMessageCallback(\"input1\")..........FAILED!\r\n");
            ret = __FAILURE__;
        }
        else if (IoTHubModuleClient_LL_SetModuleTwinCallback(iotHubModuleClientHandle, moduleTwinCallback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
        {
            printf("ERROR: IoTHubModuleClient_LL_SetModuleTwinCallback(default)..........FAILED!\r\n");
            ret = __FAILURE__;
        }
        else
        {
            ret = 0;
        }

        return ret;
    }
    ```

11. Guarde este archivo.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior se ha creado una solución de IoT Edge y se ha agregado código a CModule que va a filtrar los mensajes donde la temperatura de la máquina informada está por debajo del umbral aceptable. Ahora, tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedor. 

1. Inicie sesión en Docker. Para ello, escriba el siguiente comando en el terminal integrado de Visual Studio Code a fin de que pueda insertar la imagen del módulo en el ACR: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilice el nombre de usuario, la contraseña y el servidor de inicio de sesión que copió de Azure Container Registry en la primera sección. También puede recuperarlos de nuevo en la sección **Claves de acceso** del registro mediante Azure Portal.

2. En el explorador de VS Code, abra el archivo **deployment.template.json** en el área de trabajo de la solución de IoT Edge. Este archivo indica a `$edgeAgent` que debe implementar los dos módulos: **tempSensor** y **CModule**. El valor `CModule.image` se establece en una versión para Linux amd64 de la imagen. Para aprender más sobre manifiestos de implementación, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

3. En el archivo **deployment.template.json**, hay una sección **registryCredentials** que almacena las credenciales de registro de Docker. Los pares reales de nombre de usuario y contraseña se almacenan en el archivo .env que Git omite.

4. Agregue el módulo gemelo CModule al manifiesto de implementación. Inserte el siguiente contenido JSON en la parte inferior de la sección `moduleContent`, después del módulo gemelo `$edgeHub`: 
    ```json
        "CModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Guarde este archivo.
5. En el explorador de VS Code, haga clic con el botón derecho en el archivo **deployment.template.json** y seleccione **Build IoT Edge solution** (Compilar solución de IoT Edge). 

Cuando le indica a Visual Studio Code que compile la solución, esta herramienta primero toma la información de la plantilla de implementación y genera un archivo `deployment.json` en una nueva carpeta **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos compilan el código, empaquetan `CModule.dll` en contenedores y lo insertan en el registro de contenedor que especificó cuando inicializó la solución. 

Puede ver la dirección de la imagen de contenedor completa con la etiqueta en el terminal integrado de VS Code. La dirección de la imagen se crea a partir de la información del archivo `module.json`, con el formato **\<repositorio\>:\<versión\>-\<plataforma\>**. Para este tutorial, se debería parecer a **registryname.azurecr.io/cmodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

1. Configure la extensión de Azure IoT Toolkit con la cadena de conexión del centro de IoT: 
    1. Abra el explorador de VS Code seleccionando **Ver** > **Explorador**. 
    2. En el explorador, haga clic en **AZURE IOT HUB DEVICES** y, luego, en **...**. Haga clic en **Select IoT Hub** (Seleccionar IoT Hub). Siga las instrucciones para iniciar sesión en la cuenta de Azure y elija su instancia de IoT Hub. 
       Tenga en cuenta que también puede realizar la configuración haciendo clic en **Set IoT Hub Connection String** (Establecer cadena de conexión de IoT Hub). Escriba la cadena de conexión del centro de IoT al que se conecta el dispositivo IoT Edge en la ventana emergente.

2. En el Explorador de dispositivos de Azure IoT Hub, haga clic con el botón derecho en el dispositivo IoT Edge y, a continuación, haga clic en **Create deployment for Edge device** (Crear implementación para dispositivo Edge). Seleccione el archivo **deployment.json** de la carpeta **config** y, a continuación, haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge).

3. Haga clic en el botón Actualizar. Debería ver el nuevo **CModule** en ejecución junto con el módulo **TempSensor**, **$edgeAgent** y **$edgeHub**. 

## <a name="view-generated-data"></a>Visualización de datos generados

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
> La eliminación de los recursos de Azure y del grupo de recursos es un proceso irreversible. Una vez eliminados, el grupo de recursos y todos los recursos que contiene se suprimirán permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. Si ha creado una instancia de IoT Hub en un grupo de recursos ya existente que contiene recursos que desea conservar, puede eliminar solo esa instancia en lugar de eliminar todo el grupo de recursos.
>

Para eliminar solo la instancia de IoT Hub, ejecute el siguiente comando usando el nombre del centro y el nombre del grupo de recursos:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Para eliminar un grupo de recursos entero por el nombre:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Grupos de recursos**.

2. Escriba el nombre del grupo de recursos que contiene la instancia de IoT Hub en el cuadro de texto **Filtrar por nombre...**. 

3. A la derecha del grupo de recursos de la lista de resultados, haga clic en **...** y, a continuación, en **Eliminar grupo de recursos**.

4. Se le pedirá que confirme la eliminación del grupo de recursos. Escriba otra vez el nombre del grupo de recursos para confirmar y haga clic en **Eliminar**. Transcurridos unos instantes, el grupo de recursos y todos los recursos que contiene se eliminan.



## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo IoT Edge que contiene código para filtrar los datos sin procesar generados por el dispositivo IoT Edge. Cuando esté listo para compilar sus propios módulos, consulte [Develop a C# module with Azure IoT Edge for Visual Studio Code](how-to-develop-c-module.md) (Desarrollo de un módulo de C con Azure IoT Edge para Visual Studio Code). Puede continuar con los siguientes tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Almacenamiento de datos en el perímetro con bases de datos de SQL Server](tutorial-store-data-sql-server.md)

<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
