---
title: Conexión de un dispositivo de IoT Plug and Play (versión preliminar) a Azure IoT Central | Microsoft Docs
description: Uso de un modelo de funcionalidad de dispositivo para generar código de dispositivo. Después, ejecute el código del dispositivo, vea si el dispositivo se conecta a la aplicación IoT Central y use las vistas generadas automáticamente.
author: dominicbetts
ms.author: dobett
ms.date: 08/08/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 5d5815467444afeb5f08380eea6868336044d237
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73892331"
---
# <a name="tutorial-use-a-device-capability-model-to-create-an-iot-plug-and-play-device-and-connect-it-to-your-iot-central-application"></a>Tutorial: Uso de un modelo de funcionalidad de dispositivo para crear un dispositivo IoT Plug and Play y conectarlo a la aplicación IoT Central

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Un _modelo de funcionalidad de dispositivo_ (DCM) describe las funcionalidades de un dispositivo [IoT Plug and Play](../../iot-pnp/overview-iot-plug-and-play.md). IoT Central puede usar un DCM para crear una plantilla de dispositivo y visualizaciones para un dispositivo la primera vez que este se conecta.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Usar Visual Studio Code para crear un dispositivo IoT Plug and Play mediante un DCM.
> * Ejecutar el código del dispositivo en Windows y ver si se conecta a la aplicación IoT Central.
> * Especificar los datos de telemetría simulados que envía el dispositivo.

## <a name="prerequisites"></a>Requisitos previos

Complete el inicio rápido [Creación de una aplicación de Azure IoT Central (características en versión preliminar)](./quick-deploy-iot-central.md) para crear una aplicación de IoT Central mediante la plantilla **Custom app > Preview application**.

Para completar este tutorial, es preciso instalar el siguiente software en la máquina local:

* [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/): asegúrese de incluir el componente **Administrador de paquetes NuGet** y la carga de trabajo **Desarrollo para el escritorio con C++** al instalar Visual Studio.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/): al instalar **CMake**, seleccione la opción **Add CMake to the system PATH** (Agregar CMake a la ruta de acceso del sistema).
* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* La utilidad `dps-keygen`:

    ```cmd/sh
    npm i -g dps-keygen
    ```

### <a name="install-azure-iot-tools"></a>Instalación de Azure IoT Tools

Siga estos pasos para instalar el paquete de extensiones Azure IoT Tools en VS Code:

1. En VS Code, seleccione la pestaña **Extensions** (Extensiones).
1. Busque **Azure IoT Tools**.
1. Seleccione **Instalar**.

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

### <a name="get-azure-iot-device-sdk-for-c"></a>Obtención del SDK de dispositivo IoT de Azure para C

Prepare un entorno de desarrollo que se puede usar para compilar el SDK de dispositivo IoT de Azure para C.

1. Abra el símbolo del sistema. Ejecute el siguiente comando para clonar el repositorio de GitHub del [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c):

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Esta operación puede tardar varios minutos en completarse.

1. Cree una carpeta `central_app` en la raíz del clon local del repositorio. Use esta carpeta para los archivos del modelo de dispositivo y el código auxiliar del código de dispositivo.

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir central_app
    ```

## <a name="generate-device-key"></a>Generación de la clave del dispositivo

Para conectar un dispositivo a una aplicación IoT Central, se necesita una clave de dispositivo. Para generar una clave de dispositivo:

1. Inicie sesión en la aplicación de IoT Central que creó mediante la plantilla **Custom app > Preview application** en el inicio rápido [Creación de una aplicación de Azure IoT Central (características en versión preliminar)](./quick-deploy-iot-central.md).

1. Vaya a la página **Administration** (Administración) y seleccione **Device Connection** (Conexión de dispositivo).

1. Anote los valores de **ID Scope** (Id. de ámbito) y **Primary Key** (Clave principal) que ve al seleccionar **View Keys** (Ver claves). Estos valores se usarán más adelante en el tutorial.

    ![Conexión del dispositivo](./media/tutorial-connect-pnp-device/device-connection.png)

1. Para generar una clave de dispositivo, abra un símbolo del sistema y ejecute el siguiente comando:

    ```cmd/sh
    dps-keygen  -di:mxchip-01 -mk:{Primary Key from previous step}
    ```

    Anote la _clave de dispositivo_ generada, ya que usará ese valor en un paso posterior de este tutorial.

## <a name="download-your-model"></a>Descarga del modelo

En este tutorial, usará el DCM público para un dispositivo MxChip IoT DevKit. Para ejecutar el código no se necesita un dispositivo DevKit real, ya que en este tutorial se compila el código para ejecutarlo en Windows.

1. Abra la carpeta `azure-iot-sdk-c\central_app` con VS Code.

1. Use **Ctrl+Mayús+P** para abrir la paleta de comandos, escriba **IoT Plug and Play** y seleccione **Open Model Repository** (Abrir repositorio de modelos). Seleccione **Public repository** (Repositorio público). VS Code muestra una lista de los DCM en el repositorio de modelos público.

1. Seleccione el DCM **MXChip IoT DevKit** con el identificador `urn:mxchip:mxchip_iot_devkit:1`. Después, seleccione **Download** (Descargar ). Ya tiene una copia del DCM en la carpeta `central_app`.

![Repositorio de modelos y DCM](./media/tutorial-connect-pnp-device/public-repository.png)

> [!NOTE]
> Para trabajar con IoT Central, el modelo de funcionalidad del dispositivo debe tener todas las interfaces definidas insertadas en el mismo archivo.

## <a name="generate-the-c-code-stub"></a>Generación de código auxiliar de C

Ahora que ya tiene el DCM **MXChip IoT DevKit** y sus interfaces asociadas, puede generar el código del dispositivo que implementa el modelo. Para generar el código auxiliar de C en VS Code:

1. Con la carpeta con los archivos de DCM abierta, use **Ctrl+Mayús+P** para abrir la paleta de comandos, escriba **, IoT Plug and Play** y seleccione **Generate Device Code Stub** (Generar código auxiliar de dispositivo).

    > [!NOTE]
    > La primera vez que use la utilidad de generación de código de IoT Plug and Play, tardará unos segundos en descargarse.

1. Seleccione el archivo del DCM **MXChip IoT DevKit** que acaba de descargar.

1. Escriba el nombre del proyecto **devkit_device**.

1. Elija **ANSI C** como lenguaje.

1. Elija **CMake Project** (Proyecto de CMake) como tipo de proyecto. No elija **MXChip IoT DevKit Project** (Proyecto de MXChip IoT DevKit), ya que esta opción es para cuando hay un dispositivo DevKit real.

1. Elija **Via DPS (Device Provisioning Service) symmetric key** (Clave simétrica a través de DPS [Device Provisioning Service]) como método de conexión.

1. VS Code abre una nueva ventana con los archivos del código auxiliar del dispositivo generados en la carpeta `devkit_device`.

![Código de dispositivo generado](./media/tutorial-connect-pnp-device/generated-code.png)

## <a name="build-the-code"></a>Compilación del código

El SDK del dispositivo se usa para compilar el código auxiliar del dispositivo generado. La aplicación que se compila simula un dispositivo **MXChip IoT DevKit** y se conecta a la aplicación IoT Central. La aplicación envía datos de telemetría y propiedades, y recibe comandos.

1. En VS Code, abra el archivo `CMakeLists.txt` de la carpeta `azure-iot-sdk-c`. Asegúrese de abrir el `CMakeLists.txt` archivo de la carpeta `azure-iot-sdk-c`, no el de la carpeta `devkit_device`.

1. Agregue la línea siguiente en la parte inferior del archivo `CMakeLists.txt` para incluir la carpeta de código auxiliar del dispositivo al compilar:

    ```txt
    add_subdirectory(central_app/devkit_device)
    ```

1. Cree una carpeta `cmake` en la carpeta `azure-iot-sdk-c` y vaya a esa carpeta en un símbolo del sistema:

    ```cmd\sh
    mkdir cmake
    cd cmake
    ```

1. Ejecute los siguientes comandos para compilar el SDK del dispositivo y el código auxiliar generado:

    ```cmd\sh
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    cmake --build . -- /m /p:Configuration=Release
    ```

1. Después de que la compilación finalice correctamente, ejecute la aplicación en el mismo símbolo del sistema. Reemplace `scopeid` y `primarykey` por los valores que anotó anteriormente:

    ```cmd\sh
    .\central_app\devkit_device\Release\devkit_device.exe scopeid primarykey mxchip-001
    ```

1. La aplicación del dispositivo comienza a enviar datos a la aplicación IoT Central.

## <a name="view-the-device"></a>Visualización del dispositivo

Una vez que el código del dispositivo se conecte a IoT Central, se pueden ver tanto las propiedades como los datos de telemetría que envía:

1. En la aplicación IoT Central, vaya a la página **Devices** (Dispositivos) y seleccione el dispositivo **mxchip-01**. Este dispositivo se agregó automáticamente cuando se conectó el código del dispositivo:

    ![Página de información general](./media/tutorial-connect-pnp-device/overview-page.png)

    Después de un par de minutos, esta página muestra gráficos con los datos de telemetría que el dispositivo envía.

1. Seleccione la página **About** (Acerca de) para ver los valores de las propiedades que ha enviado el dispositivo.

1. Seleccione la página**Commands** (Comandos) para llamar a los comandos del dispositivo. Puede ver que el dispositivo responde en el símbolo del sistema que ejecuta el código del dispositivo.

1. Vaya a la página **Device templates** (Plantillas de dispositivos) para ver la plantilla que IoT Central creó a partir del DCM en el repositorio público:

    ![Página Device templates (Plantillas de dispositivos)](./media/tutorial-connect-pnp-device/device-template.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a conectar un dispositivo IoT Plug and Play que se generó a partir de un DCM en el repositorio de modelos público.

Para más información sobre los DCM y cómo crear sus propios modelos, continúe con la guía de procedimientos:

> [!div class="nextstepaction"]
> [Creación de un grupo de dispositivos](./tutorial-use-device-groups.md)
