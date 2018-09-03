---
title: Implementación de funciones de Azure con Azure IoT Edge | Microsoft Docs
description: En este tutorial, va a implementar una función de Azure como módulo en un dispositivo perimetral.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/22/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: cf973899a6f56010c47588bdd506418a3d9a7cd8
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "42744049"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules-preview"></a>Tutorial: Implementación de funciones de Azure como módulos de IoT Edge (versión preliminar)

Azure Functions se puede usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos de Azure IoT Edge. En este tutorial, se detallan los pasos para crear e implementar una función de Azure que filtra los datos de sensor en el dispositivo IoT Edge simulado. Para realizar el tutorial, utilizará el dispositivo IoT Edge que creó en las guías de inicio rápido para implementar Azure IoT Edge en un dispositivo simulado con [Windows][lnk-tutorial1-win] o [Linux][lnk-tutorial1-lin]. En este tutorial, aprenderá a:     

> [!div class="checklist"]
> * Usar Visual Studio Code para crear una función de Azure.
> * Usar VS Code y Docker para crear una imagen de Docker y publicarla en el registro de contenedor.
> * Implementar el módulo desde el registro de contenedor en el dispositivo IoT Edge.
> * Ver los datos filtrados.

<center>
![Diagrama de arquitectura del tutorial](./media/tutorial-deploy-function/FunctionsTutDiagram.png)
</center>

>[!NOTE]
>Los módulos de Azure Functions en Azure IoT Edge se encuentran en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

La función de Azure que creó en este tutorial filtra los datos sobre la temperatura generados por el dispositivo. La función solo envía mensajes a los niveles superiores de Azure IoT Hub si la temperatura sobrepasa el umbral especificado. 

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
* [El SDK de .NET Core 2.1](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor

En este tutorial, puede usar la extensión de Azure IoT Edge para VS Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.  

En este tutorial, puede usar cualquier Registro compatible con Docker. Dos de los servicios de Docker Registry más populares que están disponibles en la nube son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Container Registry**.

    ![Creación de un Registro de contenedor](./media/tutorial-deploy-function/create-container-registry.png)

2. Especifique los siguientes valores para crear un registro de contenedor:

   | Campo | Valor | 
   | ----- | ----- |
   | Nombre de registro | Especifique un nombre único. |
   | Subscription | Seleccione una suscripción en la lista desplegable. |
   | Grupos de recursos | Se recomienda usar el mismo grupo de recursos para todos los recursos de prueba que se crean en las guías de inicio rápido y los tutoriales de IoT Edge. Por ejemplo, **IoTEdgeResources**. |
   | Ubicación | Elija una ubicación cercana a usted. |
   | Usuario administrador | Seleccione **Habilitar**. |
   | SKU | Seleccione **Básica**. | 

5. Seleccione **Crear**.

6. Una vez que se haya creado el Registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**. 

7. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. Usará estos valores más adelante en el tutorial. 

## <a name="create-a-function-project"></a>Creación de un proyecto de aplicación de una función

La extensión de Azure IoT Edge para Visual Studio Code que ha instalado en los requisitos previos proporciona funcionalidades de administración, así como algunas plantillas de código. En esta sección se usa Visual Studio Code para crear una solución IoT Edge que contiene una función de Azure. 

1. Abra Visual Studio Code en el equipo de desarrollo.

2. Para abrir la paleta de comandos de VS Code, seleccione **Ver** > **Paleta de comandos**.

3. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in**. Siga las instrucciones para iniciar sesión en la cuenta de Azure.

4. En la paleta de comandos, escriba el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge) y ejecútelo. Para crear la solución, siga las indicaciones de la paleta de comandos.

   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado **EdgeSolution**.
   3. Elija **Azure Functions - C#** como la plantilla del módulo. 
   4. Asigne al módulo el nombre **CSharpFunction**. 
   5. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión que copió. La cadena final se parece a \<nombre del Registro\>.azurecr.io/csharpfunction.

   ![Especificación del repositorio de imágenes de Docker](./media/tutorial-deploy-function/repository.png)

4. La ventana de VS Code carga el área de trabajo de la solución de IoT Edge: una carpeta \.vscode, una carpeta de módulos, un archivo de plantilla de manifiesto de implementación. y un archivo \.env. En el explorador de VS Code, abra **modules** > **CSharpFunction** > **EdgeHubTrigger-Csharp** > **run.csx**.

5. Reemplace el contenido del archivo **run.csx** por el código siguiente:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it.
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold.
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object.
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);                }
                // Add a new property to the message to indicate it is an alert.
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message.       
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages.
    class MessageBody
    {
        public Machine machine {get; set;}
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

6. Guarde el archivo.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior se ha creado una solución de IoT Edge y se ha agregado código a **CSharpFunction** que filtrará los mensajes donde la temperatura de la máquina notificada está por debajo del umbral aceptable. Ahora, tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedor.

En esta sección, se proporcionan las credenciales para el registro de contenedor dos veces. La primera consiste en iniciar sesión localmente desde el equipo de desarrollo para que Visual Studio Code pueda insertar imágenes en el registro. La segunda se encuentra en el archivo **.env** de la solución IoT Edge, que concede a su dispositivo IoT Edge permisos para extraer imágenes del registro. 

1. Abra el terminal integrado de VS Code, para lo que debe seleccionar **View** > **Terminal** (Ver > Terminal). 

1. Inicie sesión en su registro de contenedor, para lo que debe escribir el comando siguiente en el terminal integrado. A continuación, puede insertar la imagen del módulo en Azure Container Registry: 
     
    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Use el nombre de usuario y el servidor de inicio de sesión que copió anteriormente de Azure Container Registry. Cuando se le pida la contraseña, pegue la contraseña del registro de contenedor y presione **Entrar**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

2. En el explorador de VS Code, abra el archivo **deployment.template.json** en el área de trabajo de la solución de IoT Edge. Este archivo indica al entorno de ejecución de Azure IoT Edge qué módulos implementar en un dispositivo. Tenga en cuenta que en el módulo Function, **CSharpFunction** aparece junto con el módulo **tempSensor** que proporciona datos de prueba. Para más información sobre los manifiestos de implementación, consulte esta [descripción acerca de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

   ![Ver el módulo en el manifiesto de implementación](./media/tutorial-deploy-function/deployment-template.png)

3. Abra el archivo **.env** en el área de trabajo de la solución de IoT Edge. Este archivo con Git ignorado almacena las credenciales del registro de contenedor, con el fin de que no tenga que colocarlas en la plantilla del manifiesto de implementación. Especifique el **nombre de usuario** y la **contraseña** del registro de contenedor. 

5. Guarde este archivo.

6. En el explorador de VS Code, haga clic con el botón derecho en el archivo deployment.template.json y seleccione **Build and Push IoT Edge solution** (Compilar e insertar solución de IoT Edge). 

Cuando le indica a Visual Studio Code que compile la solución, esta herramienta primero toma la información de la plantilla de implementación y genera un archivo deployment.json en una nueva carpeta denominada **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. Estos dos comandos compilan el código, empaquetan las funciones en contenedores e insertan el código en el registro de contenedor que especificó cuando inicializó la solución. 

## <a name="view-your-container-image"></a>Visualización de una imagen de contenedor

Visual Studio Code genera un mensaje de éxito cuando la imagen de contenedor se inserta en el registro de contenedor. Si quiere confirmar por su cuenta que la operación es correcta, puede ver la imagen en el registro. 

1. En Azure Portal, vaya al registro de contenedor de Azure. 
2. Seleccione **Repositorios**.
3. Verá que el repositorio **csharpfunction** se muestra en la lista. Seleccione este repositorio para ver más detalles.
4. En la sección **Etiquetas**, verá la etiqueta **0.0.1-amd64**. Esta etiqueta refleja la versión y la plataforma de la imagen que ha creado. Estos valores se establecen en el archivo module.json, en la carpeta CSharpFunction. 

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

Puede usar Azure Portal para implementar el módulo de función en un dispositivo de IoT Edge, como hizo en las guías de inicio rápido. También puede implementar y supervisar los módulos desde dentro de Visual Studio Code. En las secciones siguientes, se usa la extensión Azure IoT Edge para VS Code que se indicó en los requisitos previos. Si no lo ha hecho aún, instale la extensión ahora. 

1. Para abrir la paleta de comandos de VS Code, seleccione **Ver** > **Paleta de comandos**.

2. Busque y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión). Siga las instrucciones para iniciar sesión en la cuenta de Azure. 

3. En la paleta de comandos, busque y ejecute el comando **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: seleccionar IoT Hub). 

4. Seleccione la suscripción que contiene la instancia de IoT Hub y, después, el centro de IoT al que desea acceder.

5. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

6. Haga clic con el botón derecho en el nombre del dispositivo IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear una implementación para un dispositivo individual). 

7. Vaya a la carpeta de la solución que contiene **CSharpFunction**. Abra la carpeta config, seleccione el archivo deployment.json y, a continuación, haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge).

8. Actualice la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Verá el nuevo módulo **CSharpFunction** en ejecución junto con el módulo **TempSensor** y **$edgeAgent** y **$edgeHub**. 

   ![Visualización de los módulos implementados en VS Code](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-generated-data"></a>Visualización de datos generados

Puede ver todos los mensajes que llegan a su centro de IoT mediante la ejecución de **Azure IoT Hub: Start Monitoring D2C Message** (Azure IoT Hub: iniciar la supervisión de mensajes de D2C) en la paleta de comandos.

También puede filtrar la vista para ver todos los mensajes que llegan al centro de IoT desde un dispositivo específico. Haga clic con el botón derecho en la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) y seleccione **Start Monitoring D2C Messages** (Iniciar la supervisión de mensajes de D2C).

Para dejar de supervisar mensajes, ejecute el comando **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: detener la supervisión de mensaje de D2C) en la paleta de comandos. 


## <a name="clean-up-resources"></a>Limpieza de recursos

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba. 

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo de Azure Functions que contiene código para filtrar datos sin procesar generados por el dispositivo IoT Edge. Cuando esté preparado para crear sus propios módulos, consulte [Develop Azure Functions with Azure IoT Edge for Visual Studio Code](how-to-develop-csharp-function.md) (Desarrollo de Azure Functions con Azure IoT Edge para Visual Studio Code). 

Puede continuar con los siguientes tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Búsqueda de promedios mediante una ventana flotante en Azure Stream Analytics](tutorial-deploy-stream-analytics.md)

<!--Links-->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
