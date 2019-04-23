---
title: Cómo programar trabajos con IoT Hub de Azure (.NET/.NET) | Microsoft Docs
description: Cómo programar un trabajo de IoT Hub de Azure para invocar un método directo en varios dispositivos. El SDK de dispositivo IoT de Azure para .NET se usa para implementar las aplicaciones de dispositivo simulado y una aplicación de servicio para ejecutar el trabajo.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/06/2018
ms.author: robinsh
ms.openlocfilehash: db34734e9fbb8635f1a62def8a877d83d02e2206
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59788095"
---
# <a name="schedule-and-broadcast-jobs-netnet"></a>Programación y difusión de trabajos (.NET/.NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Use Azure IoT Hub para programar y realizar el seguimiento de los trabajos que actualizan millones de dispositivos. Use los trabajos para:

* Actualizar las propiedades deseadas
* Actualizar etiquetas
* Invocar métodos directos

Los trabajos contienen una de estas acciones y realizan el seguimiento de la ejecución en un conjunto de dispositivos, que define una consulta de dispositivo gemelo. Por ejemplo, una aplicación de back-end puede utilizar un trabajo para invocar un método directo en 10 000 dispositivos que reinicie los dispositivos. Especifique el conjunto de dispositivos con una consulta de dispositivo gemelo y programe el trabajo para que se ejecute en otro momento. Este trabajo realiza el seguimiento del progreso mientras los dispositivos reciben y ejecutan el método directo de reinicio.

Para más información sobre estas funcionalidades, vea:

* Dispositivo gemelo y propiedades: [Introducción a los dispositivos gemelos](iot-hub-csharp-csharp-twin-getstarted.md) y [Tutorial: Cómo usar las propiedades del dispositivo gemelo](tutorial-device-twins.md)

* Métodos directos: [Guía del desarrollador de IoT Hub: métodos directos](iot-hub-devguide-direct-methods.md) y [Tutorial: Uso de métodos directos](quickstart-control-device-dotnet.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

En este tutorial se muestra cómo realizar las siguientes acciones:

* Creación de una aplicación de dispositivo que implemente un método directo denominado **LockDoor** que la aplicación de back-end puede llamar.

* Creación de una aplicación de back-end que cree un trabajo para llamar al método directo **LockDoor** en varios dispositivos. Otro trabajo envía las actualizaciones en la propiedad deseada a varios dispositivos.

Al final de este tutorial tendrá dos aplicaciones de consola de .NET (C#):

**SimulateDeviceMethods** que se conecta a IoT Hub e implementa el método directo **LockDoor**.

**ScheduleJob**, que usa trabajos para llamar al método directo **LockDoor** y actualizar las propiedades del dispositivo gemelo deseadas en varios dispositivos.

Para completar este tutorial, necesitará lo siguiente:

* Visual Studio 2017.
* Una cuenta de Azure activa. En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) en tan solo unos minutos.

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Recuperación de la cadena de conexión del centro de IoT

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registro de un nuevo dispositivo en el centro de IoT

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Creación de una aplicación de dispositivo simulado

En esta sección, creará una aplicación de consola de .NET que responderá a un método directo llamado por el back-end de solución.

1. En Visual Studio, agregue un proyecto de escritorio clásico de Windows de Visual C# a la solución actual mediante la plantilla de proyecto **Aplicación de consola** . Asigne al proyecto el nombre **SimulateDeviceMethods**.
   
    ![Nueva aplicación para dispositivo de Windows clásico de Visual C#](./media/iot-hub-csharp-csharp-schedule-jobs/create-device-app.png)
    
2. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **SimulateDeviceMethods** y luego haga clic en **Administrar paquetes NuGet...**.

3. En la ventana **Administrador de paquetes NuGet**, seleccione **Examinar** y busque **Microsoft.Azure.Devices.Client**. Seleccione **Instalar** para instalar el paquete **Microsoft.Azure.Devices.Client** y acepte las condiciones de uso. Este procedimiento descarga, instala y agrega una referencia al paquete NuGet del [SDK de dispositivo IoT de Azure](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) y sus dependencias.
   
    ![Ventana del Administrador de paquetes NuGet: aplicación cliente](./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png)

4. Agregue las siguientes instrucciones `using` al principio del archivo **Program.cs** :
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. Agregue los campos siguientes a la clase **Program** . Sustituya el valor de marcador de posición por la cadena de conexión del dispositivo que anotó en la sección anterior:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;
    ```

6. Agregue lo siguiente para implementar el método directo en el dispositivo:

    ```csharp
    static Task<MethodResponse> LockDoor(MethodRequest methodRequest, object userContext)
    {
        Console.WriteLine();
        Console.WriteLine("Locking Door!");
        Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);
            
        string result = "'Door was locked.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```

7. Agregue lo siguiente para implementar el agente de escucha de dispositivos gemelos en el dispositivo:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

8. Finalmente, agregue el código siguiente al método **Main** para abrir la conexión a su IoT Hub e inicializar la escucha del método directo:
   
    ```csharp
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
          TransportType.Mqtt);

        Client.SetMethodHandlerAsync("LockDoor", LockDoor, null);
        Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null);

        Console.WriteLine("Waiting for direct method call and device twin update\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        Client.SetMethodHandlerAsync("LockDoor", null, null);
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
9. Guarde el trabajo y compile la solución.         

> [!NOTE]
> Por simplificar, este tutorial no implementa ninguna directiva de reintentos. En el código de producción, debe implementar directivas de reintento (por ejemplo, reintento de conexión), tal y como se sugiere en el artículo [Control de errores transitorios](/azure/architecture/best-practices/transient-faults).
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Programación de trabajos para llamar a un método directo y envío de actualizaciones de dispositivo gemelo

En esta sección, creará una aplicación de consola de .NET (mediante C#) que usa trabajos para llamar al método directo **LockDoor** y enviar las actualizaciones en la propiedad deseada a varios dispositivos.

1. En Visual Studio, agregue un proyecto de escritorio clásico de Windows de Visual C# a la solución actual mediante la plantilla de proyecto **Aplicación de consola** . Asigne al proyecto el nombre **ScheduleJob**.

    ![Nuevo proyecto de escritorio clásico de Windows de Visual C#](./media/iot-hub-csharp-csharp-schedule-jobs/createnetapp.png)

2. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ScheduleJob** y luego haga clic en **Administrar paquetes NuGet...**.

3. En la ventana **Administrador de paquetes NuGet**, seleccione **Examinar**, busque **Microsoft.Azure.Devices**, seleccione **Instalar** para instalar el paquete **Microsoft.Azure.Devices** y acepte los términos de uso. Este paso permite descargar, instalar y agregar una referencia al paquete NuGet del [SDK de servicio IoT de Azure](https://www.nuget.org/packages/Microsoft.Azure.Devices/) y sus dependencias.

    ![Ventana del Administrador de paquetes NuGet](./media/iot-hub-csharp-csharp-schedule-jobs/servicesdknuget.png)

4. Agregue las siguientes instrucciones `using` al principio del archivo **Program.cs** :
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Agregue la siguiente instrucción `using` si no está ya presente en las instrucciones predeterminadas.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

6. Agregue los campos siguientes a la clase **Program** . Sustituya los marcadores de posición por la cadena de conexión de IoT Hub que creó en la sección anterior y el nombre del dispositivo.

    ```csharp
    static JobClient jobClient;
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
    ```

7. Agregue el método siguiente a la clase **Program** :

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && 
          (result.Status != JobStatus.Failed));
    }
    ```

8. Agregue el método siguiente a la clase **Program** :

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = 
          new CloudToDeviceMethod("LockDoor", TimeSpan.FromSeconds(5), 
          TimeSpan.FromSeconds(5));
       
        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            $"DeviceId IN ['{deviceId}']",
            directMethod,
            DateTime.UtcNow,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Method Job");
    }
    ```

9. Agregue otro método a la clase **Program**:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        Twin twin = new Twin(deviceId);
        twin.Tags = new TwinCollection();
        twin.Tags["Building"] = "43";
        twin.Tags["Floor"] = "3";
        twin.ETag = "*";

        twin.Properties.Desired["LocationUpdate"] = DateTime.UtcNow;

        JobResponse createJobResponse = jobClient.ScheduleTwinUpdateAsync(
            jobId,
            $"DeviceId IN ['{deviceId}']", 
            twin, 
            DateTime.UtcNow, 
            (long)TimeSpan.FromMinutes(2).TotalSeconds).Result;

        Console.WriteLine("Started Twin Update Job");
    }
    ```

    > [!NOTE]
    > Para más información sobre la sintaxis de consulta, consulte [Lenguaje de consulta de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language).
    > 

10. Por último, agregue las líneas siguientes al método **Main** :

    ```csharp
    Console.WriteLine("Press ENTER to start running jobs.");
    Console.ReadLine();

    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

11. Guarde el trabajo y compile la solución. 

## <a name="run-the-apps"></a>Ejecución de las aplicaciones

Ya está preparado para ejecutar las aplicaciones.

1. En el Explorador de soluciones de Visual Studio, haga clic con el botón derecho en la solución y, después, haga clic en **Compilar**. **Proyectos de inicio múltiples**. Asegúrese de que `SimulateDeviceMethods` está en la parte superior de la lista seguido de `ScheduleJob`. Establezca ambas acciones en **Iniciar** y haga clic en **Aceptar**.

2. Ejecute los proyectos haciendo clic en **Iniciar** o vaya al menú **Depurar** y haga clic en **Iniciar depuración**.

3. Verá la salida de las aplicaciones back-end y de dispositivo.

    ![Ejecutar las aplicaciones para programar trabajos](./media/iot-hub-csharp-csharp-schedule-jobs/schedulejobs.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha utilizado un trabajo para programar un método directo para un dispositivo y la actualización de las propiedades del dispositivo gemelo.

Para continuar la introducción a IoT Hub y los patrones de administración de dispositivos como remoto a través de la actualización de firmware de aire, leer [Tutorial: Cómo realizar una actualización de firmware](tutorial-firmware-update.md).

Para más información sobre cómo implementar AI en dispositivos perimetrales con Azure IoT Edge, consulte [Introducción a IoT Edge](../iot-edge/tutorial-simulate-device-linux.md).