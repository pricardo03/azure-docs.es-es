---
title: Almacenamiento de datos con el módulo SQL de Azure IoT Edge | Microsoft Docs
description: Aprenda a almacenar datos localmente en el dispositivo IoT Edge con un módulo de SQL Server
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/30/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 2b393a5b60ba534fba8115ab3ef0f35a26ad3ed4
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43300360"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Tutorial: Almacenamiento de datos en el perímetro con bases de datos de SQL Server

Use Azure IoT Edge y SQL Server para almacenar y consultar datos en el perímetro. Azure IoT Edge incluye características de almacenamiento básicas integradas que almacenan mensajes en la memoria caché cuando un dispositivo se queda sin conexión y que, posteriormente, los reenvían cuando se restablece esta. No obstante, puede que también quiera funcionalidades de almacenamiento más avanzadas como, por ejemplo, poder consultar datos de forma local. Mediante la incorporación de bases de datos locales, los dispositivos de IoT Edge pueden realizar procesos más complejos sin tener que mantener una conexión con IoT Hub. Por ejemplo, un técnico de campo puede visualizar los datos de los últimos días de un sensor en una máquina de forma local, incluso aunque esos datos se carguen solo una vez al mes en la nube para ayudar a mejorar un modelo de aprendizaje automático.

Este artículo proporciona instrucciones para implementar una base de datos de SQL Server en un dispositivo IoT Edge. Azure Functions, ejecutándose en el dispositivo IoT Edge, estructura los datos de entrada y a continuación los envía a la base de datos. Los pasos descritos en este artículo también pueden aplicarse a otras bases de datos que funcionan en contenedores, como MySQL o PostgreSQL.

En este tutorial, aprenderá a: 

> [!div class="checklist"]
> * Usar Visual Studio Code para crear una instancia de Azure Functions
> * Implementar una base de datos SQL en el dispositivo IoT Edge
> * Usar Visual Studio Code para compilar módulos e implementarlos en el dispositivo IoT Edge
> * Visualización de datos generados

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para dispositivos de [Linux](quickstart-linux.md) o de [Windows](quickstart.md).

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar en Azure. 

Recursos de desarrollo:

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de C# para Visual Studio Code (con tecnología de OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) para Visual Studio Code. 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor
En este tutorial, puede usar la extensión de Azure IoT Edge para VS Code a fin de generar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.  

En este tutorial, puede usar cualquier Registro compatible con Docker. Dos de los servicios de Registro de Docker más populares que están disponibles en la nube son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Container Registry**.

    ![creación de un registro de contenedor](./media/tutorial-deploy-function/create-container-registry.png)

2. Asigne un nombre a su registro y elija una suscripción.
3. Para el grupo de recursos, se recomienda que utilice el mismo nombre de grupo de recursos que contiene la instancia de IoT Hub. Al mantener todos los recursos en el mismo grupo, puede administrarlos juntos. Por ejemplo, si elimina el grupo de recursos que se usó con fines de prueba también se eliminarán todos los recursos que contiene. 
4. Establezca la SKU en **Básico** y cambie **Usuario administrador** a **Habilitar**. 
5. Haga clic en **Create**(Crear).
6. Una vez que se haya creado el Registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**. 
7. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. Los usará más adelante en el tutorial. 

## <a name="create-a-function-project"></a>Creación de un proyecto de aplicación de una función

Para enviar datos a una base de datos, necesita un módulo que pueda estructurar los datos correctamente y que, después, los almacene en una tabla. 

En los siguientes pasos puede ver cómo crear una función de IoT Edge mediante Visual Studio Code y la extensión de Azure IoT Edge.

1. Abra Visual Studio Code.
2. Abra el terminal integrado de VS Code, para lo que debe seleccionar **View** > **Terminal** (Ver > Terminal).
3. Para abrir la paleta de comandos de VS Code, seleccione **View** > **Command palette** (Ver > Paleta de comandos).
4. En la paleta de comandos, escriba y ejecute el comando **Azure: Sign in** (Azure: iniciar sesión) y siga las instrucciones para iniciar sesión en la cuenta de Azure. Si ya ha iniciado sesión, puede omitir este paso.
3. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución IoT Edge). En la paleta de comandos, proporcione la siguiente información para crear la solución: 
   1. Seleccione la carpeta en la que desea crear la solución. 
   2. Proporcione un nombre para la solución o acepte el valor predeterminado **EdgeSolution**.
   3. Elija **Azure Functions - C#** como la plantilla del módulo. 
   4. Asigne el nombre **sqlFunction** al módulo. 
   5. Especifique la instancia de Azure Container Registry que creó en la sección anterior como el repositorio de imágenes del primer módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión que copió. La cadena final se parece a **\<nombre del registro\>.azurecr.io/sqlFunction**.

4. El área de trabajo de la solución de IoT Edge se carga en la ventana de Visual Studio Code. Hay una carpeta de **módulos**, una carpeta **.vscode** y un archivo de plantilla de manifiesto de implementación. Abra **modules** > **sqlFunction** > **EdgeHubTrigger-Csharp** > **run.csx**.

5. Reemplace el contenido del archivo por el código siguiente:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"
   #r "System.Data.SqlClient"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;
   using System.Threading.Tasks;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
       const int temperatureThreshold = 25;
       byte[] messageBytes = messageReceived.GetBytes();
       var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

       if (!string.IsNullOrEmpty(messageString))
       {
           // Get the body of the message and deserialize it
           var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

           //Store the data in SQL db
           const string str = "<sql connection string>";
           using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
           {
           conn.Open();
           var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
           var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");"; 
               using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
               {
               //Execute the command and log the # rows affected.
               var rows = await cmd.ExecuteNonQueryAsync();
               log.Info($"{rows} rows were updated");
               }
           }

           if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
           {
               // Send the message to the output as the temperature value is greater than the threshold
               var filteredMessage = new Message(messageBytes);
               // Copy the properties of the original message into the new Message object
               foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
               {
                   filteredMessage.Properties.Add(prop.Key, prop.Value);
               }
               // Add a new property to the message to indicate it is an alert
               filteredMessage.Properties.Add("MessageType", "Alert");
               // Send the message        
               await output.AddAsync(filteredMessage);
               log.Info("Received and transferred a message with temperature above the threshold");
           }
       }
   }

   //Define the expected schema for the body of incoming messages
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

6. En la línea 24, reemplace la cadena **\<sql connection string\>** por la cadena siguiente. La propiedad **Data Source** hace referencia al nombre del contenedor de SQL Server que creará con el nombre **SQL** en la siguiente sección. 

   ```C#
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. Guarde el archivo **run.csx**. 

## <a name="add-a-sql-server-container"></a>Incorporación de un contenedor de SQL Server

Un [manifiesto de implementación](module-composition.md) declara qué módulos del entorno de ejecución de IoT Edge se instalarán en el dispositivo IoT Edge. Agregó el código para hacer un módulo de función personalizado en la sección anterior, pero el módulo de SQL Server ya está integrado. Solo debe indicar el entorno de ejecución de IoT Edge para incluirlo y configurarlo en el dispositivo. 

1. En el explorador de Visual Studio Code, abra el archivo **deployment.template.json**. 
2. Busque la sección **moduleContent.$edgeAgent.properties.desired.modules**. Debería haber dos módulos en la lista: **tempSensor**, que genera datos simulados, y su módulo **sqlFunction**.
3. Si usa contenedores de Windows, modifique la sección **sqlFunction.settings.image**.
    ```json
    "image": "${MODULES.sqlFunction.windows-amd64}"
    ```
4. Agregue el código siguiente para declarar un tercer módulo. Agregue una coma después de la sección sqlFunction e inserte:

   ```json
   "sql": {
       "version": "1.0",
       "type": "docker",
       "status": "running",
       "restartPolicy": "always",
       "settings": {
           "image": "",
           "createOptions": ""
       }
   }
   ```

   Este es un ejemplo, por si hubiera confusiones con la adición de un elemento JSON. ![Incorporación de un contenedor de SQL Server](./media/tutorial-store-data-sql-server/view_json_sql.png)

5. Según el tipo de contenedores de Docker del dispositivo IoT Edge, actualice los parámetros de **sql.settings** con el código siguiente:

   * Contenedores de Windows:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   * Contenedores de Linux:

      ```json
      "image": "microsoft/mssql-server-linux:2017-latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   >[!Tip]
   >Cada vez que cree un contenedor de SQL Server en un entorno de producción, debería [cambiar la contraseña de administrador del sistema predeterminada](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

6. Guarde el archivo **deployment.template.json**.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En las secciones anteriores, ha creado una solución con un módulo y, después, agregó otro a la plantilla del manifiesto de implementación. Ahora, debe compilar la solución, crear imágenes de contenedor para los módulos e insertar las imágenes en el registro de contenedor. 

1. En el archivo .env, indique las credenciales del registro al entorno de ejecución de IoT Edge para que pueda acceder a las imágenes del módulo. Busque las secciones **CONTAINER_REGISTRY_USERNAME** y **CONTAINER_REGISTRY_PASSWORD** e inserte las credenciales después del símbolo igual: 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourContainerReg=<username>
   CONTAINER_REGISTRY_PASSWORD_yourContainerReg=<password>
   ```
2. Guarde el archivo .env.
3. Inicie sesión en el registro de contenedor en Visual Studio Code para que pueda insertar las imágenes en el registro. Use las mismas credenciales que ha agregado al archivo .env. En el terminal integrado, escriba el siguiente comando:

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Se le pedirá la contraseña. Pegue la contraseña en el símbolo del sistema y presione **ENTRAR**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

4. En el explorador de VS Code, haga clic con el botón derecho en el archivo **deployment.template.json** y seleccione **Build and Push IoT Edge solution** (Compilar e insertar solución de IoT Edge). 

## <a name="deploy-the-solution-to-a-device"></a>Implementación de la solución en un dispositivo

Puede establecer módulos en un dispositivo con IoT Hub, pero también puede acceder a este y a los dispositivos mediante Visual Studio Code. En esta sección puede configurar el acceso a IoT Hub y, posteriormente, usar VS Code para implementar la solución en el dispositivo IoT Edge. 

1. En la paleta de comandos de VS Code, seleccione **Azure IoT Hub: Select IoT Hub**.
2. Siga las indicaciones para iniciar sesión en su cuenta de Azure. 
3. En la paleta de comandos, seleccione la suscripción de Azure y, a continuación, seleccione IoT Hub. 
4. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 
5. Haga clic con el botón derecho en el dispositivo que quiere como destino para la implementación y seleccione **Create deployment for single device** (Crear implementación para dispositivo individual). 
6. En el explorador de archivos, vaya a la carpeta **config** dentro de la solución y elija **deployment.json**. Haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge). 

Si la implementación es correcta, se imprimirá el mensaje de confirmación en la salida de VS Code. También puede comprobar que todos los módulos están activados y ejecutándose en el dispositivo. 

En el dispositivo IoT Edge, ejecute el siguiente comando para ver el estado de los módulos. Esto puede tardar unos minutos.

   ```PowerShell
   iotedge list
   ```

## <a name="create-the-sql-database"></a>Creación de la base de datos SQL

Cuando aplica el manifiesto de implementación al dispositivo, consigue la ejecución de tres módulos. El módulo tempSensor genera datos en un entorno simulado. El módulo sqlFunction toma los datos y les da un formato para una base de datos. 

Esta sección le guiará en el proceso de configuración de la base de datos SQL para almacenar los datos de temperatura. 

1. En una herramienta de línea de comandos, conéctese a la base de datos. 
   * Contenedor Windows:
   
      ```cmd
      docker exec -it sql cmd
      ```
    
   * Contenedor Linux: 

      ```bash
      docker exec -it sql bash
      ```

2. Abra la herramienta de comando SQL.
   * Contenedor Windows:

      ```cmd
      sqlcmd -S localhost -U SA -P "Strong!Passw0rd"
      ```

   * Contenedor Linux: 

      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. Cree la base de datos: 

   * Contenedor Windows
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = 'C:\mssql\measurementsdb.mdf')
      GO
      ```

   * Contenedor Linux
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
      GO
      ```

4. Defina la tabla.

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

Puede personalizar el archivo de Docker de SQL Server para configurar automáticamente SQL Server para que se implemente en varios dispositivos IoT Edge. Para más información, consulte el [proyecto de demostración de contenedor de Microsoft SQL Server](https://github.com/twright-msft/mssql-node-docker-demo-app). 

## <a name="view-the-local-data"></a>Visualización de los datos locales

Después de que la tabla se crea, el módulo sqlFunction empieza a almacenar datos en una base de datos local de SQL Server 2017 del dispositivo IoT Edge. 

Desde dentro de la herramienta de comando SQL, ejecute el siguiente comando para ver los datos de tabla con formato: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

   ![Visualización de datos locales](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Limpieza de recursos

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba. 

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo de Azure Functions que contiene código para filtrar datos sin procesar generados por el dispositivo IoT Edge. Cuando esté listo para compilar sus propios módulos, consulte [Develop Azure Functions with Azure IoT Edge for Visual Studio Code](how-to-develop-csharp-function.md) (Desarrollo de Azure Functions con Azure IoT Edge para Visual Studio Code). 

Puede continuar con los siguientes tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Búsqueda de promedios mediante una ventana flotante en Azure Stream Analytics](tutorial-deploy-stream-analytics.md)
