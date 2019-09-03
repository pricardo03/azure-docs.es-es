---
title: Creación de un dispositivo IoT Plug and Play (versión preliminar) | Microsoft Docs
description: Uso de un modelo de funcionalidad de dispositivo para generar código de dispositivo. Luego, ejecute el código de dispositivo y verá que el dispositivo se conecta a su centro de IoT.
author: miagdp
ms.author: miag
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 386c2fa23e8d01f696ef3cf6078bac5fcec58f05
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050140"
---
# <a name="quickstart-use-a-device-capability-model-to-create-an-iot-plug-and-play-device"></a>Inicio rápido: Uso de un modelo de funcionalidad de dispositivo para un dispositivo IoT Plug and Play

Un _modelo de funcionalidad de dispositivo_ (DCM) describe las funcionalidades de un dispositivo IoT Plug and Play. Un DCM suele estar asociado a una SKU del producto. Las funcionalidades definidas en el DCM se organizan en interfaces reutilizables. Desde un DCM se puede generar el código de un dispositivo básico. En este inicio rápido se muestra cómo usar VS Code para crear un dispositivo IoT Plug and Play mediante un DCM.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido, es preciso instalar el siguiente software en el equipo local:

* [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/): asegúrese de incluir el componente **Administrador de paquetes NuGet** y la carga de trabajo **Desarrollo para el escritorio con C++** al instalar Visual Studio.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).
* [Visual Studio Code](https://code.visualstudio.com/)

### <a name="install-azure-iot-device-workbench"></a>Instalación de Azure IoT Device Workbench

Siga estos pasos para instalar la extensión Azure IoT Device Workbench en VS Code:

1. En VS Code, seleccione la pestaña **Extensions** (Extensiones).
1. Busque **Azure IoT Device Workbench**.
1. Seleccione **Instalar**.

### <a name="install-the-azure-iot-explorer"></a>Instalación de Azure IoT Explorer

Descargue e instale la herramienta Azure IoT Explorer desde la página de la[versión más reciente](https://github.com/Azure/azure-iot-explorer/releases).

### <a name="get-the-connection-string-for-your-company-model-repository"></a>Obtención de la cadena de conexión para el repositorio de modelos de su empresa

Puede encontrar la _cadena de conexión para el repositorio de modelos de su empresa_ en el [ portal de Azure Certified for IoT ](https://preview.catalog.azureiotsolutions.com) cuando inicia sesión con una cuenta profesional o educativa de Microsoft o con su identificador de asociado de Microsoft, en caso de que tenga. Después de iniciar sesión, seleccione **Company repository** (Repositorio de la empresa) y, después, **Connection strings** (Cadenas de conexión).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-an-iot-hub"></a>Preparación de un centro de IoT

También necesitará un centro de IoT de Azure en la suscripción de Azure para completar este inicio rápido. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

Agregue la extensión Microsoft Azure IoT para la CLI de Azure:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Ejecute el siguiente comando la nueva identidad del dispositivo en su centro de IoT. Reemplace los marcadores de posición **YourIoTHubName** y **YourDevice** por los nombres reales. Si aún no tiene un centro de IoT, siga [estas instrucciones para crearlo](../iot-hub/iot-hub-create-using-cli.md):

```azurecli-interactive
az iot hub device-identity create --hub-name [YourIoTHubName] --device-id [YourDevice]
```

Ejecute los siguientes comandos para obtener la _cadena de conexión del dispositivo_ que acaba de registrar:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name [YourIoTHubName] --device-id [YourDevice] --output table
```

Ejecute los comandos siguientes para obtener la _cadena de conexión de su centro de IoT_:

```azurecli-interactive
az iot hub show-connection-string --hub-name [YourIoTHubName] --output table
```

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

### <a name="get-azure-iot-device-sdk-for-c"></a>Obtención del SDK de dispositivo IoT de Azure para C

En este inicio rápido se prepara un entorno de desarrollo que se puede usar para clonar y compilar el SDK de dispositivo IoT de Azure para C.

1. Abra el símbolo del sistema. Ejecute el siguiente comando para clonar el repositorio de GitHub del [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c):

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Esta operación puede tardar varios minutos en completarse.

1. Cree un `pnp_app` subdirectorio en la raíz del clon local del repositorio. Use esta carpeta para los archivos del modelo de dispositivo y el código auxiliar del código de dispositivo.

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir pnp_app
    ```

## <a name="author-your-model"></a>Creación de un modelo

En este inicio rápido, se usa un modelo de funcionalidad del dispositivo de ejemplo existente y las interfaces asociadas.

1. Descargue el [modelo de funcionalidad del dispositivo](https://github.com/Azure/IoTPlugandPlay/blob/master/samples/SampleDevice.capabilitymodel.json) y un [ejemplo de interfaz](https://github.com/Azure/IoTPlugandPlay/blob/master/samples/EnvironmentalSensor.interface.json), y guarde los archivos en la carpeta `pnp_app`.

    > [!TIP]
    > Para descargar un archivo de GitHub, vaya hasta el archivo, haga clic con el botón derecho en **Sin procesar** y seleccione **Guardar vínculo como**.

1. Abra la carpeta `pnp_app` con VS Code. Los archivos se pueden ver con Intellisense:

    ![Modelo de funcionalidad del dispositivo](media/quickstart-create-pnp-device/dcm.png)

1. En los archivos descargados, reemplace `<YOUR_COMPANY_NAME_HERE>` en los campos `@id` y `schema` por un valor único. Use solo los caracteres a-z, A-Z, 0-9 y el carácter de subrayado. Para más información, consulte [Formato de identificador de gemelo digital](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format).

## <a name="generate-the-c-code-stub"></a>Generación de código auxiliar de C

Ahora que ya tiene un DCM y sus interfaces asociadas, puede generar el código del dispositivo que implementa el modelo. Para generar el código auxiliar de C en VS Code:

1. Con la carpeta con los archivos de DCM abierta, use **Ctrl+Mayús+P** para abrir la paleta de comandos, escriba **, IoT Plug and Play** y seleccione **Generate Device Code Stub** (Generar código auxiliar de dispositivo).

    > [!NOTE]
    > La primera vez que use la utilidad de generación de código de IoT Plug and Play, tardará unos segundos en descargarse.

1. Elija el archivo de DCM que quiere usar para generar el código auxiliar del dispositivo.

1. Escriba el nombre del proyecto, **sample_device**. Será el nombre de la aplicación del dispositivo.

1. Elija **ANSI C** como lenguaje.

1. Elija **CMake Project** (Proyecto de CMake) como tipo de proyecto.

1. Elija **Via IoT Hub device connection string** (Mediante la cadena de conexión del dispositivo de IoT Hub) como método de conexión.

1. VS Code abre una nueva ventana con los archivos del código auxiliar del dispositivo generados.
    ![Código del dispositivo](media/quickstart-create-pnp-device/device-code.png)

## <a name="build-the-code"></a>Compilación del código

El SDK del dispositivo se usa para compilar el código auxiliar del dispositivo generado. La aplicación que se compila simula un dispositivo que se conecta a un centro de IoT. La aplicación envía datos de telemetría y propiedades, y recibe comandos.

1. En VS Code, Abra `CMakeLists.txt` en la carpeta raíz del SDK del dispositivo.

1. Agregue la línea siguiente en la parte inferior del archivo `CMakeLists.txt` para incluir la carpeta de código auxiliar del dispositivo al compilar:

    ```txt
    add_subdirectory(pnp_app/sample_device)
    ```

1. Cree un subdirectorio cmake en la carpeta raíz del SDK del dispositivo y vaya a esa carpeta:

    ```cmd\sh
    mkdir cmake
    cd cmake
    ```

1. Ejecute los siguientes comandos para compilar el SDK del dispositivo y el código auxiliar generado:

    ```cmd\sh
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > Si cmake no encuentra el compilador de C++, aparecerán errores de compilación al ejecutar el comando anterior. En ese caso, pruebe a ejecutar el comando en el [símbolo del sistema de Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs).

1. Una vez que la compilación se complete correctamente, ejecute la aplicación y use la cadena de conexión del dispositivo al centro de IoT como parámetro.

    ```cmd\sh
    cd azure-iot-sdk-c\cmake\pnp_app\sample_device\Release\
    sample_device.exe "[IoT Hub device connection string]"
    ```

1. La aplicación del dispositivo comienza a enviar datos a IoT Hub.

    ![Aplicación del dispositivo en ejecución](media/quickstart-create-pnp-device/device-app-running.png)

## <a name="validate-the-code"></a>Validación del código

### <a name="publish-device-model-files-to-model-repository"></a>Publicación de los archivos del modelo del dispositivo en el repositorio de modelos

Para validar el código del con dispositivo con **Azure IoT Explorer**, es preciso publicar los archivos en el repositorio de modelos.

1. Con la carpeta con los archivos de DCM abierta, use **Ctrl+Mayús+P** para abrir la paleta de comandos, escriba y seleccione  **IoT Plug and Play: Submit files to Model Repository** (IoT Plug and Play: Enviar archivos al repositorio de modelos).

1. Seleccione los archivos `SampleDevice.capabilitymodel.json` y `EnvironmentalSensor.interface.json`.

1. Escriba la cadena de conexión del repositorio de modelos de su empresa.

    > [!NOTE]
    > La cadena de conexión solo es necesaria la primera vez que se conecta al repositorio.

1. Tanto en la ventana de salida de VS Code como en la notificación, puede comprobar que los archivos se han publicado correctamente.

    > [!NOTE]
    > Si aparecen errores al publicar los archivos del modelo de dispositivo, puede probar a usar el comando **IoT Plug and Play: Sign out Model Repository** (IoT Plug and Play: Cerrar sesión del repositorio de modelos) para cerrar la sesión y repasar los pasos realizados.

### <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Uso de Azure IoT Explorer para validar el código

1. Abra Azure IoT Explorer y verá la página **App configurations** (Configuraciones de aplicaciones).

1. Escriba la cadena de conexión de IoT Hub y haga clic en **Conectar**.

1. Después de conectarse, verá la página de información general del dispositivo.

1. Para agregar el repositorio de la empresa, seleccione **Settings** (Configuración), después, **+ New** (+ Nuevo) y, después, **Company repository** (Repositorio de la empresa).

1. Agregue la cadena de conexión del repositorio de modelos de su empresa. Seleccione **Conectar**.

1. En la página de información general del dispositivo, busque la identidad del dispositivo que creó anteriormente y selecciónela para ver más detalles.

1. Expanda la interfaz con el identificador **urn:azureiot:EnvironmentalSensor:1** para ver los primitivos de IoT Plug and Play (propiedades, comandos y telemetría).

1. Seleccione la página **Telemetry** (Telemetría) para ver los datos de telemetría que envía el dispositivo.

1. Seleccione la página **Properties(non-writable)** [Propiedades (no se permite la escritura)] para ver las propiedades que no permiten la escritura notificadas por el dispositivo.

1. Seleccione la página **Properties(writable)** [Propiedades (se permite la escritura)] para ver las propiedades que permiten la escritura que se pueden actualizar.

1. Expanda el **nombre** de la propiedad, actualícelo con un nuevo nombre y seleccione **update writable property** (actualizar propiedad editable). 
2. Para ver si el nuevo nombre se muestra en la columna **Reported Property** (Propiedad notificada), haga clic en el botón **Refresh** (Actualizar) de la parte superior de la página.

1. Seleccione la página **Command** (Comando) para ver todos los comandos que admite el dispositivo.

1. Expanda el comando **blink** y establezca un nuevo intervalo de tiempo de intermitencia. Seleccione **Send Command** (Enviar comando) para llamar al comando en el dispositivo.

1. Vaya al dispositivo simulado para comprobar que el comando se ejecutó como se esperaba.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a crear un dispositivo IoT Plug and Play mediante un DCM.

Para más información acerca de IoT Plug and Play, continúe con el tutorial:

> [!div class="nextstepaction"]
> [Creación y prueba de un modelo de funcionalidad del dispositivo mediante Visual Studio Code](tutorial-pnp-visual-studio-code.md)
