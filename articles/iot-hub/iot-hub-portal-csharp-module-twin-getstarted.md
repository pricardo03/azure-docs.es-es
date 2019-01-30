---
title: Introducción a la identidad de módulo y a los módulos gemelos de Azure IoT Hub (portal y .NET) | Microsoft Docs
description: Aprenda a crear la identidad de módulo y a actualizar módulos gemelos mediante el portal y .NET.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: dobett
ms.openlocfilehash: 6048ddbcfed4ab929d521693175c456ad91f22e2
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447108"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>Introducción a la identidad de módulo y los módulos gemelos de IoT Hub con el portal y un dispositivo .NET

> [!NOTE]
> [Las identidades de módulo y los módulos gemelos](iot-hub-devguide-module-twins.md) son similares a la identidad de dispositivo y el dispositivo gemelo de Azure IoT Hub, pero ofrecen granularidad más fina. Aunque la identidad de módulo y los dispositivos gemelos de Azure IoT Hub permiten que la aplicación back-end configure un dispositivo y proporcionan visibilidad para las condiciones del dispositivo, la identidad de módulo y los módulos gemelos proporcionan estas funcionalidades para los componentes individuales del dispositivo. En los dispositivos compatibles con varios componentes, como dispositivos con sistema operativo o firmware, permiten la configuración individual y condiciones específicas por componente.
>

En este tutorial, aprenderá a:

1. Crear una identidad de módulo en el portal. 

2. Usar el SDK de dispositivo para .NET para actualizar el módulo gemelo desde el dispositivo.

> [!NOTE]
> Para más información acerca de los diversos SDK que puede usar para crear ambas aplicaciones para que se ejecuten en dispositivos y en el back-end de la solución, consulte [Azure IoT SDKs][lnk-hub-sdks] (SDK de IoT de Azure).
>

Para completar este tutorial, necesitará lo siguiente:

* Visual Studio 2015 o Visual Studio 2017.
* Una cuenta de Azure activa. (En caso de no tenerla, puede crear una [cuenta gratuita][lnk-free-trial] en solo unos minutos).

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Recuperación de la cadena de conexión del centro de IoT

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registro de un nuevo dispositivo en el centro de IoT

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-module-identity-in-the-portal"></a>Creación de una identidad de módulo en el portal

En una identidad del dispositivo se pueden crear hasta 20 identidades de módulo. Haga clic en el botón **Add Module Identity** (Agregar identidad de módulo) de la parte superior para crear la primera identidad de módulo, denominada **myFirstModule**. 

  ![Detalles del dispositivo][9]

Guárdela y haga clic en la identidad de módulo que acaba de crear. Verá los detalles de la identidad de módulo. Copie la cadena de conexión: clave principal. Se utilizará en la siguiente sección para configurar el módulo en el dispositivo.

  ![Detalles del dispositivo][12]

## <a name="update-the-module-twin-using-net-device-sdk"></a>Actualización del módulo gemelo con el SDK de dispositivo .NET

Ha creado correctamente la identidad del módulo en la instancia de IoT Hub. Probemos la comunicación con la nube desde el dispositivo simulado. Al crearse una identidad de módulo, se crea también un módulo gemelo en IoT Hub de manera implícita. En esta sección creará una aplicación de consola .NET en el dispositivo simulado que actualiza las propiedades notificadas del módulo gemelo.

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

En Visual Studio, agregue un proyecto de escritorio clásico de Windows de Visual C# a la solución existente mediante la plantilla de proyecto **Aplicación de consola (.NET Framework)**. Asegúrese de que la versión de .NET Framework sea 4.6.1 o una posterior. Denomine el proyecto **UpdateModuleTwinReportedProperties**.

  ![Creación de un proyecto de Visual Studio][13]

## <a name="install-the-latest-azure-iot-hub-net-device-sdk"></a>Instalación del SDK de dispositivo más reciente de .NET de Azure IoT Hub

La identidad de módulo y el módulo gemelo se encuentran en versión preliminar pública. Solo está disponible en los SDK de dispositivo de la versión preliminar de IoT Hub. En Visual Studio, abra Herramientas > Administrador de paquetes NuGet > Administrar paquetes NuGet para la solución. Busque Microsoft.Azure.Devices.Client. Asegúrese de que ha marcado la casilla de inclusión de la versión preliminar. Seleccione la versión más reciente e instale. Ahora tendrá acceso a todas las características del módulo. 

  ![Instalación del SDK de servicio .NET de Azure IoT Hub V1.16.0-preview-005][14]

## <a name="get-your-module-connection-string"></a>Obtención de la cadena de conexión del módulo

Inicie sesión en [Azure Portal][lnk-portal]. Vaya a la instancia de IoT Hub y haga clic en Dispositivos IoT. Busque myFirstDevice, ábralo y verá que myFirstModule se creó correctamente. Copie la cadena de conexión del módulo, la necesitará en el paso siguiente.

  ![Detalles del módulo en Azure Portal][15]

## <a name="create-updatemoduletwinreportedproperties-console-app"></a>Cree la aplicación de consola UpdateModuleTwinReportedProperties

Agregue las siguientes instrucciones `using` al principio del archivo **Program.cs** :

```csharp
using Microsoft.Azure.Devices.Client;
using Microsoft.Azure.Devices.Shared;
```

Agregue los campos siguientes a la clase **Program** . Reemplace el valor del marcador de posición por la cadena de conexión del módulo.

```csharp
private const string ModuleConnectionString = "<Your module connection string>";
private static ModuleClient Client = null;
```

Agregue el método **OnDesiredPropertyChanged** siguiente a la clase **Program**:

```csharp
private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
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
    Microsoft.Azure.Devices.Client.TransportType transport = Microsoft.Azure.Devices.Client.TransportType.Amqp;

    try
    {
        Client = ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
        Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
        Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

        Console.WriteLine("Retrieving twin");
        var twinTask = Client.GetTwinAsync();
        twinTask.Wait();
        var twin = twinTask.Result;
        Console.WriteLine(JsonConvert.SerializeObject(twin));

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
    Console.ReadKey();
    Client.CloseAsync().Wait();
}

private static void ConnectionStatusChangeHandler(ConnectionStatus status, ConnectionStatusChangeReason reason)
{
    Console.WriteLine($"Status {status} changed: {reason}");
}
```

Este ejemplo de código muestra cómo recuperar el módulo gemelo y actualizar las propiedades notificadas con el protocolo AMQP. En la versión preliminar pública solo se admite AMQP para las operaciones de módulos gemelos.

## <a name="run-the-apps"></a>Ejecución de las aplicaciones

Ya está preparado para ejecutar las aplicaciones. En el Explorador de soluciones de Visual Studio, haga clic con el botón derecho en la solución y seleccione **Establecer proyectos de inicio**. Seleccione **Proyectos de inicio múltiples** e **Iniciar** como acción para la aplicación de consola. Y después presione F5 para iniciar la ejecución de ambas aplicaciones. 

## <a name="next-steps"></a>Pasos siguientes

Para continuar la introducción a IoT Hub y explorar otros escenarios de IoT, consulte:

* [Introducción a la identidad de módulo y los módulos gemelos de IoT Hub con un dispositivo .NET y la copia de seguridad de .NET][lnk-csharp-csharp-getstarted]
* [Introducción a IoT Edge][lnk-iot-edge]


<!-- Images. -->
[9]:./media/iot-hub-portal-csharp-module-twin-getstarted/create-module-id.JPG
[12]:./media/iot-hub-portal-csharp-module-twin-getstarted/module-details.JPG
[13]: ./media/iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.JPG
[14]: ./media/iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png
[15]: ./media/iot-hub-csharp-csharp-module-twin-getstarted/module-detail.JPG
<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-csharp-csharp-getstarted]: iot-hub-csharp-csharp-module-twin-getstarted.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
