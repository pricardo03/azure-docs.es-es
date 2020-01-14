---
title: Conexión de un dispositivo Raspberry Pi a la aplicación de Azure IoT Central (C#) | Microsoft Docs
description: Como desarrollador de dispositivos, aprenda a conectar un dispositivo Raspberry Pi a su aplicación de Azure IoT Central mediante C#.
author: viv-liu
ms.author: viviali
ms.date: 09/13/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: d27f792b39a1809cde0f27186f343af7d7aef60a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75454147"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-c"></a>Conexión de un dispositivo Raspberry Pi a su aplicación de Azure IoT Central (C#)

[!INCLUDE [howto-raspberrypi-selector](../../../includes/iot-central-howto-raspberrypi-selector.md)]

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

En este artículo se describe cómo conectar, en tanto que desarrollador de dispositivos, un dispositivo Raspberry Pi a una aplicación de Microsoft Azure IoT Central con el lenguaje de programación C#.

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos descritos en este artículo, necesita los siguientes componentes:

* Una aplicación de Azure IoT Central creada a partir de la plantilla de la **Aplicación heredada**. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
* Un dispositivo Raspberry Pi que ejecuta el sistema operativo Raspbian. Raspberry Pi debe poder conectarse a Internet. Para obtener más información, consulte [Configuración del dispositivo Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3).

## <a name="add-a-device-template"></a>Incorporación de una plantilla de dispositivo

En la aplicación de Azure IoT Central, agregue una nueva plantilla de dispositivo **Raspberry Pi** con las siguientes características:

* Datos de telemetría, lo que incluye las siguientes medidas que recopilará el dispositivo:
  * Humedad
  * Temperatura
  * Presión
  * Magnetómetro (X, Y, Z)
  * Acelerómetro (X, Y, Z)
  * Giroscopio (X, Y, Z)
* Configuración
  * Voltage (Voltaje)
  * Current
  * Fan Speed
  * Conmutador de infrarrojos.
* Propiedades
  * Propiedad Die number (Número de chip) del dispositivo
  * Propiedad Location cloud (ubicación en la nube)

1. Seleccione **+Nuevo** desde ![Plantilla de dispositivo](media/howto-connect-raspberry-pi-csharp/adddevicetemplate.png) en las plantillas de dispositivo
   

2. Seleccione **Raspberry Pi** y cree la plantilla de dispositivo Raspberry Pi ![Agregar plantilla de dispositivo](media/howto-connect-raspberry-pi-csharp/newdevicetemplate.png)

Para obtener detalles completos sobre la configuración de la plantilla de dispositivo, consulte los [detalles de la plantilla de dispositivo Raspberry Pi](#raspberry-pi-device-template-details).

## <a name="add-a-real-device"></a>Adición de un dispositivo real

En su aplicación de Azure IoT Central, agregue un dispositivo real desde la plantilla de dispositivo **Raspberry Pi**. Anote los detalles de conexión del dispositivo (**Id. de ámbito**, **Id. de dispositivo** y **Clave principal**). Para más información, consulte [Add a real device to your Azure IoT Central application](tutorial-add-device.md) (Adición de un dispositivo real a la aplicación de Azure IoT Central).

### <a name="create-your-net-application"></a>Creación de su aplicación .NET

La aplicación de dispositivo se crea y se prueba en el equipo de escritorio.

Para completar los pasos siguientes, puede usar Visual Studio Code. Para obtener más información, vea [Working with C#](https://code.visualstudio.com/docs/languages/csharp) (Trabajo con C#).

> [!NOTE]
> Si lo prefiere, puede completar los pasos siguientes utilizando un editor de código diferente.

1. Para inicializar el proyecto de .NET y agregar los paquetes de NuGet necesarios, ejecute los siguientes comandos:

   ```cmd/sh
   mkdir pisample
   cd pisample
   dotnet new console
   dotnet add package Microsoft.Azure.Devices.Client
   dotnet add package Microsoft.Azure.Devices.Provisioning.Client
   dotnet add package Microsoft.Azure.Devices.Provisioning.Transport.Mqtt
   dotnet restore
   ```

1. Abra la carpeta `pisample` en Visual Studio Code. A continuación, abra el archivo de proyecto **pisample.csproj**. Agregue la etiqueta `<RuntimeIdentifiers>` que se muestra en el fragmento de código siguiente:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RootNamespace>pisample</RootNamespace>
        <RuntimeIdentifiers>win-arm;linux-arm</RuntimeIdentifiers>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Devices.Client" Version="1.21.0" />
        <PackageReference Include="Microsoft.Azure.Devices.Provisioning.Client" Version="1.4.0" />
        <PackageReference Include="Microsoft.Azure.Devices.Provisioning.Transport.Mqtt" Version="1.1.8" />
      </ItemGroup>
    </Project>
    ```

    > [!NOTE]
    > Los números de versión del paquete pueden ser mayores que los que se muestran.

1. Guarde **pisample.csproj**. Si Visual Studio Code le pide que ejecute el comando de restauración, elija **Restaurar**.

1. Abra el archivo **Program.cs** y sustituya el contenido por el código siguiente: Actualice `{your Scope ID}`, `{your Device ID}` y `{your Device Primary Key}` con los valores que anotó anteriormente:

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Microsoft.Azure.Devices.Provisioning.Client;
    using Microsoft.Azure.Devices.Provisioning.Client.Transport;

    using Newtonsoft.Json;

    namespace pisample
    {
      class Program
      {
        static string ScopeID = "{your Scope ID}";
        static string DeviceID = "{your Device ID}";
        static string PrimaryKey = "{your Device Primary Key}";
        static string GlobalDeviceEndpoint = "global.azure-devices-provisioning.net";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();
        static CancellationTokenSource cts;
        static double baseTemperature = 60;
        static double basePressure = 500;
        static double baseHumidity = 50;

        static async Task Main(string[] args)
        {
          Console.WriteLine("== Raspberry Pi Azure IoT Central example ==");

          try
          {

            using (var security = new SecurityProviderSymmetricKey(DeviceID, PrimaryKey, null))
            {
              DeviceRegistrationResult result = await RegisterDeviceAsync(security);
              if (result.Status != ProvisioningRegistrationStatusType.Assigned) {
                Console.WriteLine("Failed to register device");
                return;
              }
              IAuthenticationMethod auth = new DeviceAuthenticationWithRegistrySymmetricKey(result.DeviceId, (security as SecurityProviderSymmetricKey).GetPrimaryKey());
              Client = DeviceClient.Create(result.AssignedHub, auth, TransportType.Mqtt);
            }

            await SendDevicePropertiesAsync();

            Console.Write("Register settings changed handler...");
            await Client.SetDesiredPropertyUpdateCallbackAsync(HandleSettingChanged, null);
            Console.WriteLine("Done");

            cts = new CancellationTokenSource();
            Task task = SendTelemetryAsync(cts.Token);

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
            cts.Cancel();
            await task;
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine(ex.Message);
          }
        }

        public static async Task<DeviceRegistrationResult> RegisterDeviceAsync(SecurityProviderSymmetricKey security)
        {
            Console.WriteLine("Register device...");

            using (var transport = new ProvisioningTransportHandlerMqtt(TransportFallbackType.TcpOnly))
            {
              ProvisioningDeviceClient provClient =
                        ProvisioningDeviceClient.Create(GlobalDeviceEndpoint, ScopeID, security, transport);

              Console.WriteLine($"RegistrationID = {security.GetRegistrationID()}");


              Console.Write("ProvisioningClient RegisterAsync...");
              DeviceRegistrationResult result = await provClient.RegisterAsync();

              Console.WriteLine($"{result.Status}");
              Console.WriteLine($"ProvisioningClient AssignedHub: {result.AssignedHub}; DeviceID: {result.DeviceId}");

              return result;
            }
        }

        public static async Task SendDevicePropertiesAsync()
        {
            Console.WriteLine("Send device properties...");
            Random random = new Random();
            TwinCollection telemetryConfig = new TwinCollection();
            reportedProperties["dieNumber"] = random.Next(1, 6);
            Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

            await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }

        private static async Task SendTelemetryAsync(CancellationToken token)
        {
          Random rand = new Random();

          while (true)
          {
            double currentTemperature = baseTemperature + rand.NextDouble() * 20;
            double currentPressure = basePressure + rand.NextDouble() * 100;
            double currentHumidity = baseHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
              humidity = currentHumidity,
              pressure = currentPressure,
              temp = currentTemperature
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));

            token.ThrowIfCancellationRequested();
            await Client.SendEventAsync(message);

            Console.WriteLine("{0} > Sending telemetry: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
          }
        }


        private static async Task HandleSettingChanged(TwinCollection desiredProperties, object userContext)
        {
          Console.WriteLine("Received settings change...");
          Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

          string setting = "fanSpeed";
          if (desiredProperties.Contains(setting))
          {
            // Act on setting change, then
            BuildAcknowledgement(desiredProperties, setting);
          }
          setting = "setVoltage";
          if (desiredProperties.Contains(setting))
          {
            // Act on setting change, then
            BuildAcknowledgement(desiredProperties, setting);
          }
          setting = "setCurrent";
          if (desiredProperties.Contains(setting))
          {
            // Act on setting change, then
            BuildAcknowledgement(desiredProperties, setting);
          }
          setting = "activateIR";
          if (desiredProperties.Contains(setting))
          {
            // Act on setting change, then
            BuildAcknowledgement(desiredProperties, setting);
          }
          Console.WriteLine("Send settings changed acknowledgement...");
          await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }

        private static void BuildAcknowledgement(TwinCollection desiredProperties, string setting)
        {
          reportedProperties[setting] = new
          {
            value = desiredProperties[setting]["value"],
            status = "completed",
            desiredVersion = desiredProperties["$version"],
            message = "Processed"
          };
        }
      }
    }
    ```

## <a name="run-your-net-application"></a>Ejecución de su aplicación .NET

Para compilar y ejecutar la aplicación de ejemplo, siga estos pasos:

1. En el entorno de línea de comandos, ejecute el comando siguiente:

   ```cmd/sh
   dotnet restore
   dotnet publish -r linux-arm
   ```

1. Copie la carpeta `pisample\bin\Debug\netcoreapp2.1\linux-arm\publish` al dispositivo Raspberry Pi. Puede usar el comando **scp** para copiar los archivos, por ejemplo:

    ```cmd/sh
    scp -r publish pi@192.168.0.40:publish
    ```

    Para obtener más información, consulte [Raspberry Pi remote access](https://www.raspberrypi.org/documentation/remote-access/) (Acceso remoto de Raspberry Pi).

1. Inicie sesión en el dispositivo Raspberry Pi y ejecute los siguientes comandos en un shell:

    ```cmd/sh
    sudo apt-get update
    sudo apt-get install libc6 libcurl3 libgcc1 libgssapi-krb5-2 liblttng-ust0 libstdc++6 libunwind8 libuuid1 zlib1g
    ```

1. En su Raspberry Pi, ejecute los comandos siguientes:

    ```cmd/sh
    cd publish
    chmod 777 pisample
    ./pisample
    ```

    ![El programa se inicia](./media/howto-connect-raspberry-pi-csharp/device_begin.png)

1. En la aplicación de Azure IoT Central, puede ver cómo el código que se ejecuta en Raspberry Pi interactúa con la aplicación:

   * En la página **Measurements** (Medidas) para el dispositivo real, puede ver la telemetría.
   * En la página **Properties** (Propiedades), puede ver el valor de la propiedad **Die Number** (Número de chip) notificado.
   * En la página **Settings** (Configuración), puede cambiar distintos parámetros en el instalador de Raspberry Pi, como el voltaje y la velocidad del ventilador.

     En la captura de pantalla siguiente se muestra Raspberry Pi recibiendo el cambio de configuración:

     ![Raspberry Pi recibe el cambio de configuración](./media/howto-connect-raspberry-pi-csharp/device_switch.png)

## <a name="raspberry-pi-device-template-details"></a>Detalles de la plantilla de dispositivo Raspberry PI

Una aplicación creada a partir de la plantilla de aplicación **Ejemplo Devkits** incluye una plantilla de dispositivo **Raspberry Pi** con las siguientes características:

### <a name="telemetry-measurements"></a>Medidas de telemetría

| Nombre del campo     | Unidades  | Mínima | Máxima | Posiciones decimales |
| -------------- | ------ | ------- | ------- | -------------- |
| humedad       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| presión       | hPa    | 260     | 1260    | 0              |
| magnetometerX (magnetómetro X)  | mgauss | -1000   | 1000    | 0              |
| magnetometerY (magnetómetro Y)  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ (magnetómetro Z)  | mgauss | -1000   | 1000    | 0              |
| accelerometerX (acelerómetro X) | mg     | -2000   | 2000    | 0              |
| accelerometerY (acelerómetro Y) | mg     | -2000   | 2000    | 0              |
| accelerometerZ (acelerómetro Z) | mg     | -2000   | 2000    | 0              |
| gyroscopeX (giróscopo X)     | mdps   | -2000   | 2000    | 0              |
| gyroscopeY (giróscopo Y)     | mdps   | -2000   | 2000    | 0              |
| gyroscopeZ (giróscopo Z)     | mdps   | -2000   | 2000    | 0              |

### <a name="settings"></a>Configuración

Valores numéricos

| Nombre para mostrar | Nombre del campo | Unidades | Posiciones decimales | Mínima | Máxima | Initial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltage (Voltaje)      | setVoltage | Voltios | 0              | 0       | 240     | 0       |
| Current      | setCurrent | Amp  | 0              | 0       | 100     | 0       |
| Fan Speed    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Cambiar configuración

| Nombre para mostrar | Nombre del campo | Texto activado | Texto desactivado | Initial |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | ACTIVAR      | Apagado      | Off     |

### <a name="properties"></a>Propiedades

| Tipo            | Nombre para mostrar | Nombre del campo | Tipo de datos                              |
| --------------- | ------------ | ---------- | -------------------------------------- |
| Propiedad de dispositivo | Die number   | dieNumber  | number                                 |
| Location        | Location     | ubicación   | {lat: float, long: float, alt?: float} |

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar un dispositivo Raspberry Pi a la aplicación Azure IoT Central, le sugerimos como siguiente paso aprender a [configurar una plantilla de dispositivo personalizada](howto-set-up-template.md) para su dispositivo IoT.
