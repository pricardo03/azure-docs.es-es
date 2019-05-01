---
title: Introducción a los dispositivos gemelos de Azure IoT Hub (.NET/.NET) | Microsoft Docs
description: Describe cómo usar dispositivos gemelos de IoT Hub de Azure para agregar etiquetas y, luego, usar una consulta de IoT Hub. Usará el SDK de dispositivo Azure IoT para .NET con el fin de implementar una aplicación para dispositivo simulado, además del SDK de servicios Azure IoT para .NET con el objetivo de implementar una aplicación de servicio que agrega las etiquetas y ejecuta la consulta de IoT Hub.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: robinsh
ms.openlocfilehash: eec63cbdbdb852aff2cf9c6fa768bdc2580d4665
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60780532"
---
# <a name="get-started-with-device-twins-netnet"></a>Introducción a los dispositivos gemelos (.NET/.NET)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Al final de este tutorial, tendrá dos aplicaciones de consola de .NET:

* **CreateDeviceIdentity**, una aplicación .NET que crea una identidad de dispositivo y una clave de seguridad asociada para conectar la aplicación para dispositivo simulado.

* **AddTagsAndQuery**, una aplicación .NET de back-end diseñada para ejecutarse desde el back-end, que agrega etiquetas y consulta dispositivos gemelos.

* **ReportConnectivity**, una aplicación .NET para dispositivo que simula un dispositivo que se conecta a su centro de IoT con la identidad del dispositivo creada anteriormente e informa de su estado de conectividad.

> [!NOTE]
> En el artículo [SDK de IoT de Azure](iot-hub-devguide-sdks.md) se proporciona información sobre los SDK de IoT de Azure que se pueden usar para crear aplicaciones de dispositivo y de back-end.
> 

Para completar este tutorial, necesitará lo siguiente:

* Visual Studio 2017.
* Una cuenta de Azure activa. (En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) en tan solo unos minutos).

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Recuperación de la cadena de conexión del centro de IoT

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registro de un nuevo dispositivo en el centro de IoT

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-the-service-app"></a>Creación de la aplicación de servicio

En esta sección, creará una aplicación de consola de .NET (que usa C#) que agrega metadatos de ubicación al dispositivo gemelo asociado con **myDeviceId**. A continuación, consulta los dispositivos gemelos almacenados en IoT Hub seleccionando los dispositivos ubicados en Estados Unidos y, después, los que informan de una conexión de red de telefonía móvil.

1. En Visual Studio, agregue un proyecto de escritorio clásico de Windows de Visual C# a la solución actual mediante la plantilla de proyecto **Aplicación de consola** . Asigne el nombre **AddTagsAndQuery** al proyecto.
   
    ![Nuevo proyecto de escritorio clásico de Windows de Visual C#](./media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png)

2. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **AddTagsAndQuery** y luego seleccione **Administrar paquetes NuGet...**.

3. En la ventana **Administrador de paquetes NuGet**, seleccione **Examinar** y busque **Microsoft.Azure.Devices**. Seleccione **Instalar** para instalar el paquete **Microsoft.Azure.Devices** y acepte las condiciones de uso. Este procedimiento permite descargar, instalar y agregar una referencia al paquete NuGet del [SDK de servicios IoT de Azure](https://www.nuget.org/packages/Microsoft.Azure.Devices/) y sus dependencias.
   
    ![Ventana del Administrador de paquetes NuGet](./media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png)

4. Agregue las siguientes instrucciones `using` al principio del archivo **Program.cs** :

    ```csharp  
    using Microsoft.Azure.Devices;
    ```

5. Agregue los campos siguientes a la clase **Program** . Sustituya el valor de marcador de posición por la cadena de conexión de IoT Hub para el centro que creó en la sección anterior.

    ```csharp  
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

6. Agregue el método siguiente a la clase **Program** :

    ```csharp  
    public static async Task AddTagsAndQuery()
    {
        var twin = await registryManager.GetTwinAsync("myDeviceId");
        var patch =
            @"{
                tags: {
                    location: {
                        region: 'US',
                        plant: 'Redmond43'
                    }
                }
            }";
        await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
        var query = registryManager.CreateQuery(
          "SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
        var twinsInRedmond43 = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43: {0}", 
          string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
        query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
        var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43 using cellular network: {0}", 
          string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
    }
    ```
   
    La clase **RegistryManager** expone todos los métodos necesarios para interactuar con dispositivos gemelos del servicio. El código anterior inicializa primero el objeto **registryManager**, luego recupera el dispositivo gemelo de **myDeviceId** y, por último, actualiza sus etiquetas con la información de la ubicación deseada.
   
    Después de la actualización, ejecuta dos consultas: la primera selecciona solo los dispositivos gemelos que se encuentran en la planta **Redmond43** y la segunda mejora la consulta para seleccionar solo los dispositivos que están también conectados a través de la red de telefonía móvil.
   
    Tenga en cuenta que el código anterior, cuando crea el objeto de **consulta**, especifica un número máximo de documentos devueltos. El objeto **consulta** contiene una propiedad booleana **HasMoreResults** que puede utilizar para invocar a los métodos **GetNextAsTwinAsync** varias veces para recuperar todos los resultados. Un método llamado **GetNextAsJson** está disponible para los resultados que no son dispositivos gemelos, por ejemplo, los resultados de consultas de agregación.

7. Por último, agregue las líneas siguientes al método **Main** :

    ```csharp  
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddTagsAndQuery().Wait();
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

8. En el Explorador de soluciones, abra **Establecer proyectos de inicio...** y asegúrese de que la **acción** de **AddTagsAndQuery** sea **Iniciar**. Compile la solución.

9. Ejecute esta aplicación haciendo clic con el botón derecho en el proyecto **AddTagsAndQuery** y seleccionando **Depurar**, seguido de **Iniciar nueva instancia**. Debería ver un dispositivo en los resultados de la consulta que pregunta por todos los dispositivos que se encuentran en **Redmond43** y ninguno para la consulta que restringe los resultados a los dispositivos que utilizan una red de telefonía móvil.
   
    ![Resultados de consulta en ventana](./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png)

En la siguiente sección, creará una aplicación de dispositivo que notifica la información de conectividad y cambia el resultado de la consulta en la sección anterior.

## <a name="create-the-device-app"></a>Creación de la aplicación del dispositivo

En esta sección, creará una aplicación de consola de .NET que se conecta al centro como **myDeviceId** y, luego, actualiza las propiedades notificadas para contener la información que indica que está conectado mediante una red de telefonía móvil.

1. En Visual Studio, agregue un proyecto de escritorio clásico de Windows de Visual C# a la solución actual mediante la plantilla de proyecto **Aplicación de consola** . Denomine el proyecto **ReportConnectivity**.
   
    ![Nueva aplicación para dispositivo de Windows clásico de Visual C#](./media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png)
    
2. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ReportConnectivity** y, luego, seleccione **Administrar paquetes NuGet**.

3. En la ventana **Administrador de paquetes NuGet**, seleccione **Examinar** y busque **Microsoft.Azure.Devices.Client**. Seleccione **Instalar** para instalar el paquete **Microsoft.Azure.Devices.Client** y acepte las condiciones de uso. Este procedimiento descarga, instala y agrega una referencia al paquete NuGet del [SDK de dispositivo IoT de Azure](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) y sus dependencias.
   
    ![Ventana del Administrador de paquetes NuGet: aplicación cliente](./media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png)

4. Agregue las siguientes instrucciones `using` al principio del archivo **Program.cs** :

    ```csharp  
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. Agregue los campos siguientes a la clase **Program** . Sustituya el valor de marcador de posición por la cadena de conexión del dispositivo del anotó en la sección anterior.

    ```csharp  
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;
      DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Agregue el método siguiente a la clase **Program** :

    ```csharp
    public static async void InitClient()
    {
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
              TransportType.Mqtt);
            Console.WriteLine("Retrieving twin");
            await Client.GetTwinAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

    El objeto **Cliente** expone todos los métodos necesarios para interactuar con dispositivos gemelos del dispositivo. El código mostrado anteriormente, inicializa el objeto **Client** y, luego, recupera el dispositivo gemelo para **myDeviceId**.

7. Agregue el método siguiente a la clase **Program** :

    ```csharp  
    public static async void ReportConnectivity()
    {
        try
        {
            Console.WriteLine("Sending connectivity data as reported property");
            
            TwinCollection reportedProperties, connectivity;
            reportedProperties = new TwinCollection();
            connectivity = new TwinCollection();
            connectivity["type"] = "cellular";
            reportedProperties["connectivity"] = connectivity;
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

   El código anterior actualiza la propiedad notificada de **myDeviceId** con la información de conectividad.

8. Por último, agregue las líneas siguientes al método **Main** :

    ```csharp
    try
    {
        InitClient();
        ReportConnectivity();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

9. En el Explorador de soluciones, abra **Establecer proyectos de inicio** y asegúrese de que la **acción** de **ReportConnectivity** sea **Iniciar**. Compile la solución.

10. Ejecute esta aplicación haciendo clic con el botón derecho en el proyecto **ReportConnectivity** y seleccionando **Depurar**, seguido de **Iniciar nueva instancia**. Debería ver cómo obtiene la información del gemelo y, luego, envía la conectividad como una *propiedad notificada*.
   
    ![Ejecución la aplicación para dispositivo para notificar sobre la conectividad](./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png)
       
11. Ahora que el dispositivo ha informado sobre su información de conectividad, debe aparecer en ambas consultas. Ejecute la aplicación **AddTagsAndQuery** .NET para volver a ejecutar las consultas. Esta vez **myDeviceId** debe aparecer en los resultados de ambas consulta.
   
    ![Conectividad de dispositivo notificada correctamente](./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, configuró un centro de IoT nuevo en Azure Portal y, después, creó una identidad de dispositivo en el registro de identidades del centro de IoT. Ha agregado metadatos de dispositivo como etiquetas desde una aplicación back-end y ha escrito una aplicación de dispositivo simulado para notificar la información de conectividad de dispositivos en el dispositivo gemelo. También ha aprendido cómo consultar esta información mediante el lenguaje de consulta de IoT Hub de tipo SQL.

Use los siguientes recursos para obtener información sobre cómo:

* Enviar telemetría desde dispositivos con el tutorial [Envío de telemetría desde un dispositivo a IoT Hub (iOS)](quickstart-send-telemetry-dotnet.md).

* Configurar dispositivos mediante las propiedades deseadas del dispositivo gemelo con el tutorial [Uso de las propiedades deseadas para configurar dispositivos](tutorial-device-twins.md).

* Controlar los dispositivos de forma interactiva (por ejemplo, encender un ventilador desde una aplicación controlada por el usuario), con el tutorial [Uso de métodos directos](quickstart-control-device-dotnet.md).
