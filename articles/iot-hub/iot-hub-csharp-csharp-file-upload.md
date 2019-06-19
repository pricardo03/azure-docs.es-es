---
title: Carga de archivos desde dispositivos a Azure IoT Hub con .NET | Microsoft Docs
description: Cómo cargar archivos de un dispositivo a la nube mediante el SDK de dispositivo IoT de Azure para. NET. Los archivos cargados se almacenan en un contenedor de blobs de Azure Storage.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: robinsh
ms.openlocfilehash: cdc0f189daebcda592338b463954efab4fc2db96
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65864429"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a>Carga de archivos de un dispositivo a la nube con IoT Hub mediante .NET

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Este tutorial se basa en el código de [Tutorial: cómo envío mensajes de nube a dispositivo con Azure IoT Hub](iot-hub-csharp-csharp-c2d.md) para mostrarle cómo usar las funcionalidades de carga de archivos de IoT Hub. En él se muestra cómo realizar las siguientes acciones:

- Proporcionar un dispositivo de forma segura con un identificador URI de blob de Azure para cargar un archivo.

- Usar las notificaciones de carga de archivo de IoT Hub para desencadenar el procesamiento del archivo en el back-end de aplicación.

En los artículos de [Envío de telemetría de un dispositivo a IoT Hub](quickstart-send-telemetry-dotnet.md) y [Envío de mensajes de la nube al dispositivo con IoT Hub](iot-hub-csharp-csharp-c2d.md) se muestra cómo usar la funcionalidad básica de mensajería del dispositivo a la nube y de la nube al dispositivo de IoT Hub. En el tutorial [Configuración del enrutamiento de mensajes con IoT Hub](tutorial-routing.md) se describe una forma de almacenar de manera confiable los mensajes enviados del dispositivo a la nube en Azure Blob Storage. Sin embargo, en algunos casos no se pueden asignar fácilmente los datos de que los dispositivos envían en los mensajes de dispositivo a nube con un tamaño relativamente reducido que acepta Azure IoT Hub. Por ejemplo:

* Archivos grandes con imágenes
* Vídeos
* Datos de vibración muestreados con alta frecuencia
* Cierto tipo de datos procesados previamente

Dichos archivos se suelen procesar por lotes en la nube mediante herramientas como [Azure Data Factory](../data-factory/introduction.md) o la pila [Hadoop](../hdinsight/index.yml). Cuando necesite cargar archivos desde un dispositivo, todavía puede usar la seguridad y confiabilidad de IoT Hub.

Al final de este tutorial, ejecutará dos aplicaciones de consola de .NET:

* **SimulatedDevice**, una versión modificada de la aplicación que creó en el tutorial [Envío de mensajes de nube a dispositivo con IoT Hub](iot-hub-csharp-csharp-c2d.md). Esta aplicación carga un archivo en el almacenamiento mediante un identificador URI de SAS proporcionado por el centro de IoT.

* **ReadFileUploadNotification**, que recibe notificaciones de carga de archivos del Centro de IoT.

> [!NOTE]
> IoT Hub admite muchas plataformas de dispositivos y lenguajes (incluido C, Java y JavaScript), mediante los SDK de dispositivo IoT de Azure. Consulte el [Centro para desarrolladores de IoT de Azure](https://azure.microsoft.com/develop/iot) para obtener instrucciones paso a paso sobre cómo conectar el dispositivo a Azure IoT Hub.

Para completar este tutorial, necesitará lo siguiente:

* Visual Studio
* Una cuenta de Azure activa. (En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) en tan solo unos minutos).

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Carga de un archivo desde una aplicación de dispositivo

En esta sección, modificará la aplicación de dispositivo que creó en [Envío de mensajes de nube a dispositivo con IoT Hub](iot-hub-csharp-csharp-c2d.md) para recibir mensajes de nube a dispositivo de IoT Hub.

1. En Visual Studio, haga clic con el botón derecho en el proyecto **SimulatedDevice**, haga clic en **Agregar** y luego haga clic en **Elemento existente**. Vaya a un archivo de imagen e inclúyalo en su proyecto. En este tutorial se supone que la imagen se llama `image.jpg`.

1. Haga doble clic en ella y luego haga clic en **Propiedades**. Asegúrese de que **Copiar en el directorio de salida** está establecido en **Copiar siempre**.

    ![Muestra dónde se actualiza la propiedad de imagen para Copiar en el directorio de salida](./media/iot-hub-csharp-csharp-file-upload/image-properties.png)

1. En el archivo **Program.cs** , agregue las siguientes instrucciones al principio del archivo:

    ```csharp
    using System.IO;
    ```

1. Agregue el método siguiente a la clase **Program** :

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    El método `UploadToBlobAsync` toma el nombre de archivo y el origen de streaming del archivo que se va a cargar y administra la carga en el almacenamiento. La aplicación de consola muestra el tiempo que se tarda en cargar el archivo.

1. Agregue el siguiente método en el método **Main**, inmediatamente delante de la línea `Console.ReadLine()`:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Por simplificar, este tutorial no implementa ninguna directiva de reintentos. En el código de producción, deberá implementar directivas de reintentos (por ejemplo, retroceso exponencial), tal y como se sugiere en el artículo [Control de errores transitorios](/azure/architecture/best-practices/transient-faults).

## <a name="receive-a-file-upload-notification"></a>Recepción de una notificación de carga de archivos

En esta sección, se escribe una aplicación de consola de .NET que recibe mensajes de notificación de carga de archivos de IoT Hub.

1. En la solución actual de Visual Studio, cree un proyecto de aplicación de Visual C# para Windows con la plantilla de proyecto de **Aplicación de consola** . Denomine al proyecto **ReadFileUploadNotification**.

    ![Nuevo proyecto en Visual Studio](./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png)

2. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ReadFileUploadNotification** y luego haga clic en **Administrar paquetes NuGet...** .

3. En la ventana **Administrador de paquetes NuGet**, busque **Microsoft.Azure.Devices**, haga clic en **Instalar** y acepte las condiciones de uso.

    Esta acción descarga, instala y agrega una referencia al [paquete NuGet del SDK de servicios IoT de Azure ](https://www.nuget.org/packages/Microsoft.Azure.Devices/) en el proyecto **ReadFileUploadNotification**.

4. En el archivo **Program.cs** , agregue las siguientes instrucciones al principio del archivo:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

5. Agregue los campos siguientes a la clase **Program** . Sustituya el valor del marcador de posición por la cadena de conexión de IoT Hub de [Envío de telemetría de un dispositivo a IoT Hub](quickstart-send-telemetry-dotnet.md):

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

6. Agregue el método siguiente a la clase **Program** :

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload notification: {0}", 
              string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }   
    ```

    Tenga en cuenta que este patrón de recepción es el mismo que se usa para recibir mensajes de nube a dispositivo desde la aplicación de dispositivo.

7. Por último, agregue las líneas siguientes al método **Main** :

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Ejecución de las aplicaciones

Ahora está preparado para ejecutar las aplicaciones.

1. Desde Visual Studio, haga clic con el botón derecho en la solución y seleccione **Establecer proyectos de inicio**. Seleccione **Proyectos de inicio múltiples**, después, elija la acción **Iniciar** para **ReadFileUploadNotification** y **SimulatedDevice**.

2. Presione **F5**. Deberían iniciarse las dos aplicaciones. Debería ver que se ha completado la carga en una aplicación de consola y que la otra aplicación de consola ha recibido el mensaje de notificación de carga. Puede usar [Azure Portal](https://portal.azure.com/) o el Explorador de servidores de Visual Studio para comprobar que el archivo cargado está en la cuenta de Azure Storage.

    ![Captura de pantalla que muestra la pantalla de salida](./media/iot-hub-csharp-csharp-file-upload/run-apps1.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a usar la funcionalidad de carga de archivos de IoT Hub para simplificar la carga de archivos desde los dispositivos. Puede continuar explorando las características y los escenarios del centro de IoT con los siguientes artículos:

* [Creación de un centro de IoT mediante programación](iot-hub-rm-template-powershell.md)
* [Introducción a SDK para C](iot-hub-device-sdk-c-intro.md)
* [SDK de IoT de Azure](iot-hub-devguide-sdks.md)

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Implementación de IA en dispositivos perimetrales con Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)

