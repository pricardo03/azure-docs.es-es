---
title: Introducción a la identidad de módulo y a los módulos gemelos de Azure IoT Hub (.NET) | Microsoft Docs
description: Aprenda a crear la identidad de módulo y a actualizar módulos gemelos mediante los SDK de IoT para .NET.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: 3b10ae0998acbb010e61ebb41c602c7d42bc14fa
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59263169"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-net-back-end-and-net-device"></a>Introducción a la identidad de módulo y los dispositivos gemelos de módulo de IoT Hub con un dispositivo .NET y un servidor back-end .NET

> [!NOTE]
> [Las identidades de módulo y los módulos gemelos](iot-hub-devguide-module-twins.md) son similares a la identidad de dispositivo y el dispositivo gemelo de Azure IoT Hub, pero ofrecen granularidad más fina. Aunque la identidad de módulo y los dispositivos gemelos de Azure IoT Hub permiten que la aplicación back-end configure un dispositivo y proporcionan visibilidad para las condiciones del dispositivo, la identidad de módulo y los módulos gemelos proporcionan estas funcionalidades para los componentes individuales del dispositivo. En los dispositivos compatibles con varios componentes, como dispositivos con sistema operativo o firmware, permiten la configuración individual y condiciones específicas por componente.

Al final de este tutorial tendrá dos aplicaciones de consola de .NET:

* **CreateIdentities**, que crea una identidad de dispositivo, una de módulo y la clave de seguridad asociada para conectar el dispositivo y los clientes del módulo.

* **UpdateModuleTwinReportedProperties**, que envía las propiedades notificadas por el módulo gemelo actualizadas a IoT Hub.

> [!NOTE]
> Para más información acerca de los diversos SDK que puede usar para crear ambas aplicaciones para que se ejecuten en dispositivos y en el back-end de la solución, consulte el artículo de los [SDK de Azure IoT Hub](iot-hub-devguide-sdks.md).

Para completar este tutorial, necesitará lo siguiente:

* Visual Studio 2017.

* Una cuenta de Azure activa. (En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) en tan solo unos minutos).

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Recuperación de la cadena de conexión del centro de IoT

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

Ya se creó IoT Hub y ya tiene el nombre de host y la cadena de conexión de IoT Hub que necesita para completar el resto del tutorial.

[!INCLUDE [iot-hub-get-started-create-module-identity-csharp](../../includes/iot-hub-get-started-create-module-identity-csharp.md)]


## <a name="update-the-module-twin-using-net-device-sdk"></a>Actualización del módulo gemelo con el SDK de dispositivo .NET

En esta sección se crea una aplicación de consola .NET en el dispositivo simulado que actualiza las propiedades notificadas del módulo gemelo.

1. **Creación de un proyecto de Visual Studio:** En Visual Studio, agregue un proyecto de escritorio clásico de Windows de Visual C# a la solución existente mediante la plantilla de proyecto **Aplicación de consola (.NET Framework)**. Asegúrese de que la versión de .NET Framework sea 4.6.1 o una posterior. Denomine el proyecto **UpdateModuleTwinReportedProperties**.

    ![Creación de un proyecto de Visual Studio](./media/iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.png)

2. **Instalación del SDK de dispositivo más reciente de .NET de Azure IoT Hub:** La identidad de módulo y el módulo gemelo se encuentran en versión preliminar pública. Solo está disponible en los SDK de dispositivo de la versión preliminar de IoT Hub. En Visual Studio, abra Herramientas > Administrador de paquetes NuGet > Administrar paquetes NuGet para la solución. Busque Microsoft.Azure.Devices.Client. Asegúrese de que ha marcado la casilla de inclusión de la versión preliminar. Seleccione la versión más reciente e instale. Ahora tendrá acceso a todas las características del módulo. 

    ![Instalación del SDK de servicio .NET de Azure IoT Hub V1.16.0-preview-005](./media/iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png)

3. **Obtenga la cadena de conexión del módulo**: para ello, inicie sesión en [Azure Portal](https://portal.azure.com/). Vaya a la instancia de IoT Hub y haga clic en Dispositivos IoT. Busque myFirstDevice, ábralo y verá que myFirstModule se creó correctamente. Copie la cadena de conexión del módulo, la necesitará en el paso siguiente.

    ![Detalles del módulo en Azure Portal](./media/iot-hub-csharp-csharp-module-twin-getstarted/module-detail.png)

4. **Cree la aplicación de consola UpdateModuleTwinReportedProperties**

    Agregue las siguientes instrucciones `using` al principio del archivo **Program.cs** :

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    ```

    Agregue los campos siguientes a la clase **Program** . Reemplace el valor del marcador de posición por la cadena de conexión del módulo.

    ```csharp
    private const string ModuleConnectionString = 
      "<Your module connection string>";
    private static ModuleClient Client = null;
    static void ConnectionStatusChangeHandler(ConnectionStatus status, 
      ConnectionStatusChangeReason reason)
    {
        Console.WriteLine("Connection Status Changed to {0}; the reason is {1}", 
          status, reason);
    }
    ```

    Agregue el método **OnDesiredPropertyChanged** siguiente a la clase **Program**:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
        {
            Console.WriteLine("desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
            Console.WriteLine("Sending current time as reported property");
            TwinCollection reportedProperties = new TwinCollection
            {
                ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
            };

            await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
        }
    ```

    Por último, agregue las líneas siguientes al método **Main** :

    ```csharp
    static void Main(string[] args)
    {
        Microsoft.Azure.Devices.Client.TransportType transport = 
          Microsoft.Azure.Devices.Client.TransportType.Amqp;

        try
        {
            Client = 
              ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
            Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

            Console.WriteLine("Retrieving twin");
            var twinTask = Client.GetTwinAsync();
            twinTask.Wait();
            var twin = twinTask.Result;
            Console.WriteLine(JsonConvert.SerializeObject(twin.Properties)); 

            Console.WriteLine("Sending app start time as reported property");
            TwinCollection reportedProperties = new TwinCollection();
            reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;

            Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (AggregateException ex)
        {
            Console.WriteLine("Error in sample: {0}", ex);
        }

        Console.WriteLine("Waiting for Events.  Press enter to exit...");
        Console.ReadLine();
        Client.CloseAsync().Wait();
    }
    ```

    Este ejemplo de código muestra cómo recuperar el módulo gemelo y actualizar las propiedades notificadas con el protocolo AMQP. En la versión preliminar pública solo se admite AMQP para las operaciones de módulos gemelos.

5. Además del método **Main** anterior, puede agregar el bloque de código siguiente para enviar eventos a IoT Hub desde el módulo:

    ```csharp
    Byte[] bytes = new Byte[2];
    bytes[0] = 0;
    bytes[1] = 1;
    var sendEventsTask = Client.SendEventAsync(new Message(bytes));
    sendEventsTask.Wait();
    Console.WriteLine("Event sent to IoT Hub.");
    ```

## <a name="run-the-apps"></a>Ejecución de las aplicaciones

Ya está preparado para ejecutar las aplicaciones. En el Explorador de soluciones de Visual Studio, haga clic con el botón derecho en la solución y seleccione **Establecer proyectos de inicio**. Seleccione **Proyectos de inicio múltiples** e **Iniciar** como acción para la aplicación de consola. Después, presione F5 para iniciar la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Para continuar la introducción a IoT Hub y explorar otros escenarios de IoT, consulte:

* [Introducción a administración de dispositivos](iot-hub-node-node-device-management-get-started.md)
* [Introducción a IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)