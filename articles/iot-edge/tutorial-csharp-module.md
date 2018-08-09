---
title: Tutorial de C# de Azure IoT Edge | Microsoft Docs
description: En este tutorial se muestra la creación de un módulo IoT Edge con código C# y su implementación en un dispositivo perimetral.
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 991113b4e3e501d6d058a83baa795a5d7cbaa585
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439686"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Tutorial: Desarrollo de un módulo IoT Edge con C# en el dispositivo simulado

Los módulos Azure IoT Edge se pueden usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos IoT Edge. En este tutorial, se detallan los pasos para crear e implementar un módulo de IoT Edge que filtra los datos de sensor. Para realizar el tutorial, utilizará el dispositivo IoT Edge que creó en las guías de inicio rápido para implementar Deploy Azure IoT Edge en un dispositivo simulado con [Windows][lnk-tutorial1-win] o [Linux][lnk-tutorial1-lin]. En este tutorial, aprenderá a:    

> [!div class="checklist"]
> * Usar Visual Studio Code para crear un módulo IoT Edge basado en el SDK de .NET Core 2.0.
> * Utilizar Visual Studio Code y Docker para crear una imagen de Docker y publicarla en el Registro.
> * Implementar el módulo en el dispositivo IoT Edge.
> * Ver datos generados.


El módulo IoT Edge que creó en este tutorial filtra lo datos sobre la temperatura generados por el dispositivo. Solo envía mensajes a los niveles superiores si la temperatura sobrepasa el umbral especificado. Este tipo de análisis perimetral resulta útil para reducir la cantidad de datos que se comunican a la nube y se almacenan en ella. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para dispositivos de [Linux](quickstart-linux.md) o de [Windows](quickstart.md).

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar en Azure. 

Recursos de desarrollo:

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de C# para Visual Studio Code (con tecnología de OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/)


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
En los siguientes pasos creará un proyecto de módulo IoT Edge basado en el SDK de .NET Core 2.0 mediante Visual Studio Code y la extensión de Azure IoT Edge.

1. En Visual Studio Code, seleccione **Ver** > **Paleta de comandos** para abrir la paleta de comandos de VS Code. 
2. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión) y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.
3. En la paleta de comandos, escriba el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge) y ejecútelo. En la paleta de comandos, proporcione la siguiente información para crear la solución: 

   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado **EdgeSolution**.
   3. Elija el **módulo de C#** como la plantilla del módulo. 
   4. Reemplace el nombre de módulo predeterminado por **CSharpModule**. 
   5. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión que copió. La cadena final se parece a \<nombre del registro\>.azurecr.io/csharpmodule.

4.  La ventana de VS Code carga el área de trabajo de la solución de IoT Edge: la carpeta modules, una carpeta \.vscode, un archivo de plantilla de manifiesto de implementación y un archivo \.env. En el explorador de VS Code, abra **modules** > **CSharpModule** > **Program.cs**.

5. En la parte superior del espacio de nombres **CSharpModule**, agregue tres instrucciones **using** para los tipos que se usarán más adelante en:

    ```csharp
    using System.Collections.Generic;     // For KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // For TwinCollection
    using Newtonsoft.Json;                // For JsonConvert
    ```

6. Agregue la variable **temperatureThreshold** a la clase **Program**. Esta variable establece el valor que debe superar la temperatura medida para que los datos se envíen al centro de IoT. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

7. Agregue las clases **MessageBody**, **Machine** y **Ambient** a la clase **Program**. Estas clases definen el esquema esperado para el cuerpo de los mensajes entrantes.

    ```csharp
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
    ```

8. En el método **Init**, el código crea y configura un objeto **ModuleClient**. Este objeto permite al módulo conectarse al entorno de ejecución de Azure IoT Edge local para enviar y recibir mensajes. La cadena de conexión que se usa en el método **Init** la suministra el entorno de ejecución de IoT Edge al módulo. Después de crear **ModuleClient**, el código lee el valor **temperatureThreshold** en las propiedades deseadas del módulo gemelo. El código registra una devolución de llamada para recibir mensajes desde el centro de IoT Edge a través del punto de conexión **input1**. Reemplace el método **SetInputMessageHandlerAsync** por uno nuevo y agregue un método **SetDesiredPropertyUpdateCallbackAsync** para las actualizaciones a las propiedades deseadas. Para realizar este cambio, reemplace la última línea del método **Init** con el siguiente código:

    ```csharp
    // Register a callback for messages that are received by the module.
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read the TemperatureThreshold value from the module twin's desired properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine($"Property TemperatureThreshold not exist: {e.Message}"); 
    }

    // Attach a callback for updates to the module twin's desired properties.
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertiesUpdate, null);

    // Register a callback for messages that are received by the module.
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Agregue el método **onDesiredPropertiesUpdate** a la clase **Program**. Este método recibe las actualizaciones sobre las propiedades que se quieren del módulo gemelo y actualiza la variable **temperatureThreshold** para que coincida. Todos los módulos tienen su propio módulo gemelo, que le permite configurar el código que se ejecuta dentro de un módulo directamente desde la nube.

    ```csharp
    static Task OnDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

10. Reemplace el método **PipeMessage** por el método **FilterMessages**. Se llama a este método cada vez que el módulo recibe un mensaje del centro de IoT Edge. Filtra los mensajes que informan de temperaturas por debajo del umbral de temperatura que se establecen mediante el módulo gemelo. También agrega la propiedad **MessageType** al mensaje con el valor establecido en **Alerta**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);
        try
        {
            ModuleClient moduleClient = (ModuleClient)userContext;
            var messageBytes = message.GetBytes();
            var messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get the message body.
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await moduleClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed.
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed.
            var moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed.
            ModuleClient moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Guarde este archivo.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior se ha creado una solución de IoT Edge y se ha agregado código a **CSharpModule** para filtrar los mensajes en los que la temperatura registrada de la máquina está por debajo del umbral aceptable. Ahora, tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedor. 

1. Escriba el comando siguiente en el terminal integrado de Visual Studio Code para iniciar sesión en Docker. A continuación, puede insertar la imagen del módulo en Azure Container Registry.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilice el nombre de usuario, la contraseña y el servidor de inicio de sesión que copió de Azure Container Registry en la primera sección. También puede recuperar estos valores en la sección **Claves de acceso** del registro en Azure Portal.

2. En el explorador de VS Code, abra el archivo deployment.template.json en el área de trabajo de la solución de IoT Edge. Este archivo indica a **$edgeAgent** que implemente los dos módulos: **tempSensor** y **CSharpModule**. El valor **CSharpModule.image** se establece en una versión para Linux amd64 de la imagen. 

   Compruebe que la plantilla tiene el nombre de módulo correcto, no el nombre **SampleModule** predeterminado que cambió cuando creó la solución de IoT Edge.

   Para más información sobre los manifiestos de implementación, consulte esta [descripción acerca de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

3. En el archivo deployment.template.json, la sección **registryCredentials** almacena las credenciales de Registro de Docker. Los pares reales de nombre de usuario y contraseña se almacenan en el archivo .env, que Git pasa por alto.  

4. Agregue el módulo gemelo **CSharpModule** al manifiesto de implementación. Inserte el siguiente contenido JSON en la parte inferior de la sección **moduleContent**, después del módulo gemelo **$edgeHub**: 
    ```json
        "CSharpModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Guarde este archivo.

5. En el explorador de VS Code, haga clic con el botón derecho en el archivo deployment.template.json y seleccione **Build IoT Edge solution** (Compilar solución de IoT Edge). 

Cuando le indica a Visual Studio Code que compile la solución, esta herramienta primero toma la información de la plantilla de implementación y genera un archivo deployment.json en una nueva carpeta denominada **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos compilan el código, empaquetan CSharpModule.dll en contenedores e insertan el código en el registro de contenedor que especificó cuando inicializó la solución. 

Puede ver la dirección de la imagen de contenedor completa con la etiqueta en el terminal integrado de VS Code. La dirección de la imagen se crea a partir de la información del archivo module.json, con el formato \<repositorio\>:\<versión\>-\<plataforma\>. Para este tutorial, debería ser similar a registryname.azurecr.io/csharpmodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

1. Configure la extensión de Azure IoT Toolkit con la cadena de conexión del centro de IoT: 

    1. Abra el explorador de VS Code seleccionando **Ver** > **Explorador**.

    1. En el explorador, seleccione **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub), seleccione los puntos suspensivos (**...**) y elija **Select IoT Hub** (Seleccionar IoT Hub). Siga las instrucciones para iniciar sesión en la cuenta de Azure y elija su centro de IoT. 

       > [!Note]
       > También puede completar la configuración seleccionando **Set IoT Hub Connection String** (Establecer cadena de conexión de IoT Hub). Escriba la cadena de conexión del centro de IoT al que se conecta el dispositivo IoT Edge en la ventana emergente.

2. En el explorador de dispositivos de Azure IoT Hub, haga clic con el botón derecho en un identificador de dispositivo IoT Edge y, a continuación, seleccione **Create deployment for IoT Edge device** (Crear implementación para el dispositivo IoT Edge). Seleccione el archivo deployment.json de la carpeta config y, a continuación, elija **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge).

3. Actualice la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Debería ver el nuevo **CSharpModule** en ejecución junto con el módulo **TempSensor** y el **$edgeAgent** y **$edgeHub**. 

## <a name="view-generated-data"></a>Visualización de datos generados

1. Para supervisar los datos que llegan al centro de IoT, seleccione los puntos suspensivos (**...**) y, luego, **Start Monitoring D2C Messages** (Inicio de la supervisión de mensajes de D2C).
2. Para supervisar el mensaje de D2C de un determinado dispositivo, haga clic con el botón derecho en un dispositivo de la lista y seleccione **Start Monitoring D2C Messages** (Iniciar supervisión de mensajes de D2C).
3. Para dejar de supervisar los datos, ejecute el comando **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: detener la supervisión del mensaje de D2C) en la paleta de comandos. 
4. Para ver o actualizar el módulo gemelo, haga clic con el botón derecho en el módulo de la lista y seleccione **Edit module twin** (Editar módulo gemelo). Para actualizar el módulo gemelo, guarde el archivo JSON gemelo, haga clic con el botón derecho en el área del editor y seleccione **Update Module Twin** (Actualizar módulo gemelo).
5. Para ver los registros de Docker, instale [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para VS Code. Puede encontrar los módulos que se están ejecutando localmente en el explorador de Docker. En el menú contextual, seleccione **Show Logs** (Mostrar registros) en la vista del terminal integrado.
 
## <a name="clean-up-resources"></a>Limpieza de recursos 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar.

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

> [!IMPORTANT]
> La eliminación de los recursos de Azure y de los grupos de recursos es un proceso irreversible. Cuando se eliminan estos elementos, el grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. Si ha creado el centro de IoT en un grupo de recursos ya existente que tiene recursos que desea conservar, elimine solo el recurso del centro de IoT en sí en lugar de eliminar todo el grupo de recursos.
>

Para eliminar solo el centro de IoT, ejecute el siguiente comando usando el nombre del centro y el nombre del grupo de recursos:

```azurecli-interactive
az iot hub delete --name {hub_name} --resource-group IoTEdgeResources
```


Para eliminar un grupo de recursos entero por el nombre:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después seleccione **Grupos de recursos**.

2. En el cuadro de texto **Filtrar por nombre**, escriba el nombre del grupo de recursos que contiene el centro de IoT. 

3. A la derecha del grupo de recursos, en la lista de resultados, seleccione los puntos suspensivos (**...**) y, después, **Eliminar grupo de recursos**.

4. Se le pedirá que confirme la eliminación del grupo de recursos. Escriba de nuevo el nombre del grupo de recursos para confirmar y seleccione **Eliminar**. Transcurridos unos instantes, el grupo de recursos y todos los recursos que contiene se eliminan.



## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo IoT Edge que contiene código para filtrar datos sin procesar generados por el dispositivo IoT Edge. Cuando esté listo para compilar sus propios módulos, consulte [Develop a C# module with Azure IoT Edge for Visual Studio Code](how-to-develop-csharp-module.md) (Desarrollo de un módulo de C# con Azure IoT Edge para Visual Studio Code). Puede continuar con los siguientes tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Almacenamiento de datos en el perímetro con bases de datos de SQL Server](tutorial-store-data-sql-server.md)

<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
