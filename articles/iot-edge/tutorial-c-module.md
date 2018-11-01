---
title: Tutorial de C de Azure IoT Edge | Microsoft Docs
description: En este tutorial se muestra la creación de un módulo IoT Edge con código C y su implementación en un dispositivo perimetral
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: a7151f8b681b0e6129308b194008742986d6a358
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155701"
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
* Los módulos de C para Azure IoT Edge no son compatibles con contenedores de Windows. Si el dispositivo IoT Edge es un equipo Windows, configúrelo para que [use contenedores de Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure. 

Recursos de desarrollo:

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) para Visual Studio Code.
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code.
* [Docker CE](https://docs.docker.com/install/). 

>[!Note]
>Los módulos de C para Azure IoT Edge no son compatibles con contenedores de Windows. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor
En este tutorial, puede usar la extensión de Azure IoT Edge para VS Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.  

En este tutorial, puede usar cualquier Registro compatible con Docker. Dos de los servicios de Registro de Docker más populares que están disponibles en la nube son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry. 

El siguiente comando de la CLI de Azure crea un registro en un grupo de recursos denominado **IoTEdgeResources**. Reemplace **{acr_name}** por un nombre único para el registro. 

   ```azurecli-interactive
   az acr create --resource-group IoTEdgeResources --name {acr_name} --sku Basic --admin-enabled true
   ```

Recupere las credenciales para el registro. 

   ```azurecli-interactive
   az acr credential show --name {acr_name}
   ```

Copie los valores de **Nombre de usuario** y una de las contraseñas. Usará estos valores más adelante en este tutorial al publicar la imagen de Docker en el registro y al agregar las credenciales del registro al tiempo de ejecución de Edge. 

## <a name="create-an-iot-edge-module-project"></a>Creación de un proyecto de módulo IoT Edge
En los siguientes pasos puede ver cómo crear un proyecto de módulo IoT Edge basado en .NET Core 2.0 mediante Visual Studio Code y la extensión de Azure IoT Edge.

### <a name="create-a-new-solution"></a>Creación de una nueva solución

Cree una plantilla de solución de C que pueda personalizar con su propio código. 

1. Seleccione **Ver** > **Paleta de comandos** para abrir la paleta de comandos de VS Code. 

2. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión) y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.

3. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución IoT Edge). En la paleta de comandos, proporcione la siguiente información para crear la solución: 

   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado: **EdgeSolution**.
   3. Elija el **módulo de C** como la plantilla del módulo. 
   4. Llame al módulo **CModule**. 
   5. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost: 5000** por el **\<nombre de registro\>.azurecr.io**. Reemplace solo la parte de localhost de la cadena, no elimine el nombre del módulo. 

   ![Especificación del repositorio de imágenes de Docker](./media/tutorial-c-module/repository.png)

El área de trabajo de la solución de IoT Edge se carga en la ventana de Visual Studio Code. El área de trabajo de la solución contiene cinco componentes de nivel superior. En este tutorial no se editarán la carpeta **\.vscode** ni el archivo **\.gitignore**. La carpeta **modules** contiene el código de C del módulo, así como archivos Dockerfile para compilar el módulo como una imagen de contenedor. El archivo **\.env** almacena las credenciales del registro de contenedor. El archivo **deployment.template.json** contiene la información que usa el entorno de ejecución de IoT Edge para implementar módulos en un dispositivo. 

Si no especificó un registro de contenedor al crear la solución y aceptó el valor predeterminado localhost:5000, no tendrá un archivo \.env. 

   ![Área de trabajo de la solución de C](./media/tutorial-c-module/workspace.png)

### <a name="add-your-registry-credentials"></a>Adición de las credenciales del Registro

El archivo del entorno almacena las credenciales del registro de contenedor y las comparte con el runtime de IoT Edge. El entorno de ejecución necesita estas credenciales para extraer las imágenes privadas e insertarlas en el dispositivo IoT Edge. 

1. En el explorador de VS Code, abra el archivo .env. 
2. Actualice los campos con los valores de **nombre de usuario** y **contraseña** que ha copiado del Registro de contenedor de Azure. 
3. Guarde este archivo. 

### <a name="update-the-module-with-custom-code"></a>Actualización del módulo con código personalizado

Agregue al módulo C código que le permita leer datos del sensor, compruebe si la temperatura que se ha notificado que tiene la máquina ha superado un umbral seguro y pase dicha información a IoT Hub. 

5. En este escenario, los datos del sensor están en formato JSON. Para filtrar los mensajes en formato JSON, importe una biblioteca JSON para C. En este tutorial se usa Parson.

   1. Descargue el [repositorio de Parson Github](https://github.com/kgabis/parson). Copie los archivos **parson.c** y **parson.h** en la carpeta **CModule**.

   2. Abra los **módulos** > **CModule** > **CMakeLists.txt**. En la parte superior del archivo, importe los archivos de Parson como una biblioteca denominada **my_parson**.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. Agregue **my_parson** a la lista de bibliotecas en la función **target_link_libraries** de CMakeLists.txt.

   4. Guarde el archivo **CMakeLists.txt**.

   5. Abra los **módulos** > **CModule** > **main.c**. Al final de la lista de instrucciones include, agregue una nueva para incluir `parson.h`, con el fin de lograr compatibilidad con JSON:

      ```c
      #include "parson.h"
      ```

6. En el archivo **main.c**, agregue una variable global denominada `temperatureThreshold` después de la sección de include. Esta variable establece el valor que debe superar la temperatura medida para que los datos se envíen a IoT Hub. 

    ```c
    static double temperatureThreshold = 25;
    ```

7. Reemplace toda la función `CreateMessageInstance` por el código siguiente. Esta función asigna un contexto para la devolución de llamada. 

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

8. Reemplace toda la función `InputQueue1Callback` por el código siguiente. Esta función implementa el filtro de mensajería real. 

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

10. Reemplace la función `SetupCallbacksForModule` por el código siguiente. 

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

11. Guarde el archivo **main.c**.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior ha creado una solución IoT Edge y ha agregado código a CModule que va a filtrar los mensajes y a eliminar aquellos en los que se notifique que la temperatura del equipo está dentro de los límites aceptables. Ahora, tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedor. 

1. Para abrir el terminal integrado de VSCode, seleccione **Ver** > **Terminal integrado**. 

1. Escriba el comando siguiente en el terminal integrado de Visual Studio Code para iniciar sesión en Docker. Debe iniciar sesión con sus credenciales de Azure Container Registry, con el fin de que pueda insertar la imagen del módulo en el registro. 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilice el nombre de usuario, la contraseña y el servidor de inicio de sesión que copió de Azure Container Registry en la primera sección. También puede recuperarlos de nuevo en la sección **Claves de acceso** del registro mediante Azure Portal.

2. En el explorador de VS Code, abra el archivo **deployment.template.json** en el área de trabajo de la solución de IoT Edge. Este archivo indica a `$edgeAgent` que debe implementar los dos módulos: **tempSensor** y **CModule**. El valor `CModule.image` se establece en una versión para Linux amd64 de la imagen. Para más información sobre los manifiestos de implementación, consulte esta [descripción acerca de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

4. Agregue el módulo gemelo CModule al manifiesto de implementación. Inserte el siguiente contenido JSON en la parte inferior de la sección `moduleContent`, después del módulo gemelo `$edgeHub`: 

    ```json
        "CModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

   ![Adición de un gemelo de CModule gemelo a una plantilla de implementación](./media/tutorial-c-module/module-twin.png)

4. Guarde el archivo **deployment.template.json**.
5. En el explorador de VS Code, haga clic con el botón derecho en el archivo **deployment.template.json** y seleccione **Build and Push IoT Edge solution** (Compilar e insertar solución de IoT Edge). 

Cuando le indica a Visual Studio Code que compile la solución, lo primero que hace esta herramienta es generar un archivo `deployment.json` en una nueva carpeta **config**. La información del archivo deployment.json se recopila del archivo de plantilla que ha actualizado, el archivo .env que ha usado para almacenar las credenciales del registro de contenedor y el archivo module.json de la carpeta CModule. 

Después, Visual Studio Code ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos compilan el código, empaquetan `CModule.dll` en contenedores y lo insertan en el registro de contenedor que especificó cuando inicializó la solución. 

Puede ver la dirección de la imagen de contenedor completa con la etiqueta en el terminal integrado de VS Code. La dirección de la imagen se crea a partir de la información del archivo `module.json`, con el formato **\<repositorio\>:\<versión\>-\<plataforma\>**. Para este tutorial, debería ser similar a la siguiente: **registryname.azurecr.io/cmodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

En el artículo de la guía de inicio rápido que siguió para configurar el dispositivo de IoT Edge, implementó un módulo mediante Azure Portal. También puede implementar módulos con la extensión Azure IoT Toolkit para Visual Studio Code. Ya tiene un manifiesto de implementación preparado para su escenario, el archivo **deployment.json**. Ahora todo lo que necesita hacer es seleccionar un dispositivo que reciba la implementación.

1. En la paleta de comandos de VS Code, ejecute **Azure IoT Hub: Select IoT Hub**. 

2. Elija la suscripción y la instancia de IoT Hub que contienen el dispositivo IoT Edge que desea configurar. 

3. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

4. Haga clic con el botón derecho en el nombre del dispositivo IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear una implementación para un dispositivo individual). 

   ![Crear una implementación para un dispositivo individual](./media/tutorial-c-module/create-deployment.png)

5. Seleccione el archivo **deployment.json** de la carpeta **config** y, a continuación, haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge). No utilice el archivo deployment.template.json. 

6. Haga clic en el botón Actualizar. Debería ver el nuevo **CModule** en ejecución junto con el módulo **TempSensor**, **$edgeAgent** y **$edgeHub**. 

## <a name="view-generated-data"></a>Visualización de datos generados

Una vez aplicado el manifiesto de implementación al dispositivo de IoT Edge, el entorno de ejecución de IoT Edge del dispositivo recopila la información de implementación nueva y comienza a ejecutarse con ella. Los módulos que se ejecuten en el dispositivo y que no están incluidos en el manifiesto de implementación se detienen. Los módulos que falten en el dispositivo se inician. 

Puede ver el estado del dispositivo de IoT Edge con la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) del explorador de Visual Studio Code. Expanda los detalles del dispositivo para ver una lista de los módulos implementados y en ejecución. 

En el propio dispositivo de IoT Edge puede ver el estado de los módulos de implementación mediante el comando `iotedge list`. Debería ver cuatro módulos: los dos módulos personalizados del entorno de ejecución de IoT Edge, tempSensor y el sensor personalizado que ha creado en este tutorial. Los módulos pueden tardar unos minutos en comenzar; si no los ve inicialmente, vuelva a ejecutar el comando. 

Los mensajes que generan los módulos se ven con el comando `iotedge logs <module name>`. 

Puede ver los mensajes conforme llegan a IoT Hub mediante Visual Studio Code. 

1. Para supervisar los datos que llegan al centro de IoT, haga clic en **...** y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
2. Para supervisar el mensaje de D2C de un determinado dispositivo, haga clic con el botón derecho en un dispositivo de la lista y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
3. Para dejar de supervisar los datos, ejecute el comando **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: detener la supervisión del mensaje de D2C) en la paleta de comandos. 
4. Para ver o actualizar el módulo gemelo, haga clic con el botón derecho en el módulo de la lista y seleccione **Edit module twin** (Editar módulo gemelo). Para actualizar el módulo gemelo, guarde el archivo JSON gemelo y haga clic con el botón derecho en el área del editor y seleccione **Update Module Twin** (Actualizar módulo gemelo).
5. Para ver los registros de Docker, instale [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para VS Code y busque en el explorador de Docker los módulos que se ejecutan localmente. En el menú contextual, haga clic en **Show Logs** (Mostrar registros) en la vista del terminal integrado.
 
## <a name="clean-up-resources"></a>Limpieza de recursos 

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba. 

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo IoT Edge que contiene código para filtrar los datos sin procesar generados por el dispositivo IoT Edge. Cuando esté listo para compilar sus propios módulos, consulte [Develop a C# module with Azure IoT Edge for Visual Studio Code](how-to-develop-c-module.md) (Desarrollo de un módulo de C con Azure IoT Edge para Visual Studio Code). Puede continuar con los siguientes tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Almacenamiento de datos en el perímetro con bases de datos de SQL Server](tutorial-store-data-sql-server.md)

