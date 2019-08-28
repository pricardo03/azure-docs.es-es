---
title: Conexión de un dispositivo IoT Plug and Play a IoT Central | Microsoft Docs
description: Como desarrollador de dispositivos, obtenga información sobre cómo usar Visual Studio Code para crear y probar un dispositivo IoT Plug and Play que se conecta a IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 630f92bc8cf5fb89626bfe4de4e4a575045c961a
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879356"
---
# <a name="use-visual-studio-code-to-create-an-iot-plug-and-play-device-that-connects-to-iot-central"></a>Uso de Visual Studio Code para crear un dispositivo IoT Plug and Play que se conecte a IoT Central

Esta guía paso a paso le muestra cómo:

* Como operador, aprenderá a agregar y configurar un dispositivo real en la aplicación de Azure IoT Central.

* Como desarrollador de dispositivos, aprenderá a usar Visual Studio Code para crear un [dispositivo IoT Plug and Play](https://aka.ms/iot-pnp-docs) que se conecta a la aplicación de IoT Central.

Use un [modelo de funcionalidad del dispositivo](./concepts-architecture-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) para definir el dispositivo que se conecta a IoT Central. En esta guía, se utiliza un modelo que define un dispositivo de sensor ambiental.

El desarrollador de dispositivos usa el modelo para generar el código de cliente de dispositivo y el generador usa el modelo para [crear una plantilla de dispositivo](./howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) en IoT Central. El modelo actúa como contrato entre el dispositivo y la aplicación de IoT Central.

En la sección de esta guía en la que se describe cómo crear el código generado se supone que usa Windows.

En esta guía, aprenderá a:

* Importar un modelo de funcionalidad del dispositivo en una plantilla de dispositivo en IoT Central.
* Agregar un panel a la plantilla que muestra la telemetría del dispositivo.
* Agregar un dispositivo real desde la plantilla.
* Importar un modelo de funcionalidad del dispositivo a Visual Studio Code.
* Generar una aplicación cliente de dispositivo C a partir del modelo.
* Compilar la aplicación cliente de dispositivo C y conectarla a IoT Central.

## <a name="prerequisites"></a>Requisitos previos

Para probar el código del dispositivo en esta guía, necesita una aplicación IoT Central creada a partir de la plantilla de la aplicación de **versión preliminar**. Si aún no tiene una aplicación IoT Central para usar, complete la guía de inicio rápido [Create an Azure IoT Central application (preview features)](./quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Creación de una aplicación de Azure IoT Central [características de versión preliminar]):

Para trabajar con el modelo de funcionalidad del dispositivo en esta guía, necesita lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/download): Visual Studio Code está disponible para varias plataformas.
* Extensión Azure IoT Device Workbench para Visual Studio Code. Siga estos pasos para instalar la extensión Azure IoT Device Workbench en VS Code:

    1. En VS Code, seleccione la pestaña **Extensiones**.
    1. Busque **Azure IoT Device Workbench**.
    1. Seleccione **Instalar**.

    > [!NOTE]
    > La versión actual del generador de código de la extensión no admite los tipos de esquema **Punto geográfico** y **Vector** ni los tipos semánticos **Aceleración**, **Velocidad** y **Ubicación**. Estos tipos de esquema y semánticos son compatibles con IoT Central.

    > [!NOTE]
    > Para trabajar con IoT Central, el modelo de funcionalidad del dispositivo debe tener todas las interfaces definidas insertadas en el mismo archivo.

Para compilar el código C generado en Windows en esta guía, necesitará lo siguiente:

* [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/): asegúrese de incluir el componente **Administrador de paquetes NuGet** y la carga de trabajo **Desarrollo para el escritorio con C++** al instalar Visual Studio.
* [Git](https://git-scm.com/download)
* [CMake](https://cmake.org/download/): al instalar **CMake**, seleccione la opción **Add CMake to the system PATH** (Agregar CMake a la ruta de acceso del sistema).
* Una copia local del SDK de C de Azure IoT:

    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

Para seguir las instrucciones del dispositivo inicial que aparecen al final de esta guía paso a paso, también debe instalar:

* [Node.js](https://nodejs.org)
* La [herramienta dps-keygen](https://www.npmjs.com/package/dps-keygen):

    ```cmd/sh
    npm i -g dps-keygen
    ```

## <a name="create-device-template"></a>Creación de la plantilla de dispositivo

Cree una carpeta llamada **pnp_app** en la carpeta **azure-iot-sdk-c** que contenga el SDK de C de Azure IOT que ha clonado. Descargue el modelo de funcionalidad del dispositivo [EnvironmentalSensor.capabilitymodel.json](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) de GitHub y guarde el archivo en la carpeta **pnp_app**. Reemplace las dos instancias de `<YOUR_COMPANY_NAME_HERE>` por su nombre. Use solo los caracteres a-z, A-Z, 0-9 y el carácter de subrayado. Anote el valor completo del campo `"@id"` que identifica de forma única el modelo de funcionalidad del dispositivo, ya que lo necesitará más adelante. Este modelo incluye dos interfaces:

* La interfaz común **DeviceManagement**.
* La interfaz personalizada **EnvironmentalSensor**.

Para crear una plantilla de dispositivo para el dispositivo de sensor ambiental en IoT Central:

1. Vaya a la página **Plantillas de dispositivo** y seleccione **+ Nuevo**. Luego elija **Personalizar**.

1. Escriba **Sensor ambiental** como nombre de la plantilla de dispositivo.

1. Elija **Importar el modelo de funcionalidad**. A continuación, busque el archivo **EnvironmentalSensor.capabilitymodel.json** que creó y seleccione **Abrir**. Las dos interfaces, **Información del dispositivo** y **Sensor ambiental**, se muestran en el **modelo de capacidad del sensor ambiental**.

1. Seleccione **Vistas** y, a continuación, **Visualización del dispositivo**.

1. En la lista de campos que puede agregar al panel, seleccione los dos valores de **telemetría**, **Humedad** y **Temperatura**, y seleccione **Combinar**. A continuación, elija **Save** (Guardar).

Ahora tiene una plantilla de dispositivo que usa el modelo de **sensor ambiental** y que tiene un panel sencillo para mostrar la telemetría desde un dispositivo.

## <a name="publish-the-template-and-add-a-real-device"></a>Publicación de la plantilla y adición de un dispositivo real

Para publicar la plantilla y agregar un dispositivo real, vaya a la página **Plantillas de dispositivo** y seleccione la plantilla de dispositivo **Sensor ambiental**. Seleccione **Publicar**, revise la información y seleccione **Publicar**.

Antes de conectar una aplicación cliente, debe agregar un dispositivo en la página **Dispositivos** y obtener la información de conexión:

1. Vaya a la página **Dispositivos** y seleccione **Sensor ambiental**. La página **Dispositivos** permite administrar los dispositivos que pueden conectarse a la aplicación.

1. Para agregar un dispositivo real, seleccione **+ Nuevo**, cambie el identificador de dispositivo a **sensor01** y seleccione **Crear**. Asegúrese de que la opción **Simulado** esté **desactivada**.

1. En la lista de dispositivos, seleccione el dispositivo **sensor ambiental**. A continuación, seleccione **Conectar**.

1. Anote el **Id. de ámbito**, el **Identificador de dispositivo** y la **Clave principal**. A continuación, seleccione **Cerrar**.

## <a name="generate-a-device-client-application"></a>Generación de una aplicación cliente de dispositivo

Use Visual Studio Code para generar una aplicación cliente de dispositivo maestro desde el modelo de funcionalidad del dispositivo:

1. Inicie Visual Studio Code y abra la carpeta **pnp_app**.

1. Use **CTRL+Mayús+P** para abrir la paleta de comandos.

1. Escriba **Plug and Play** y, a continuación, seleccione **Generate Device Code Stub**.

1. Seleccione el archivo del modelo de funcionalidad del dispositivo **EnvironmentalSensor.capabilitymodel.json**.

1. Escriba **sensor_app** como nombre del proyecto.

1. Elija **ANSI C** como idioma.

1. Elija **Proyecto de CMake** como destino.

1. Elija **Via DPS (Device Provisioning Service) symmetric key** (Clave simétrica a través de DPS [Device Provisioning Service]) como método de conexión.

Visual Studio Code abre una nueva ventana con el código C generado en la carpeta **sensor_app**.

## <a name="add-connection-details-to-the-device-client"></a>Adición de detalles de conexión al cliente de dispositivo

Para agregar la información de conexión al cliente de dispositivo, abra **main.c** en la carpeta que contiene el código generado:

1. Reemplace `[DPS Id Scope]` por el valor de **Id. de ámbito** que anotó anteriormente.

1. Reemplace `[DPS symmetric key]` por el valor de **Clave principal** que anotó anteriormente.

1. Reemplace `[device registration Id]` por el valor de **Identificador de dispositivo** que anotó anteriormente.

1. Guarde los cambios.

## <a name="build-and-run-the-client"></a>Compilación y ejecución del cliente

Para compilar y ejecutar el cliente, debe personalizar la compilación para C SDK de Azure IoT:

1. Abra el archivo **CMakeLists.txt** en la raíz de la carpeta **azure-iot-sdk-c**.

1. Agregue la siguiente línea al final de este archivo para incluir la nueva aplicación cliente en la compilación:

    ```txt
    add_subdirectory(pnp_app/sensor_app)
    ```

1. Guarde los cambios.

1. Abra el símbolo del sistema y navegue hasta la carpeta **azure-iot-sdk-c**.

1. Para compilar la aplicación, ejecute los siguientes comandos:

    ```cmd
    mkdir cmake
    cd cmake
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -Dskip_samples:BOOL=ON
    cmake --build . -- /m /p:Configuration=Release
    ```

1. Para ejecutar la aplicación, ejecute el siguiente comando desde el mismo símbolo del sistema:

    ```cmd
    pnp_app\sensor_app\Release\sensor_app.exe
    ```

## <a name="view-the-telemetry"></a>Visualización de la telemetría

Después de unos minutos, puede ver la telemetría del dispositivo en el panel del dispositivo en la aplicación IoT Central.

## <a name="connect-device-first"></a>Conexión del dispositivo inicial

Puede conectar un dispositivo IoT Plug and Play a través de una conexión de dispositivo inicial, como se describe en [Conectividad](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json). El proceso de detección sigue este orden:

1. Se asocia a la plantilla de dispositivo si ya se ha publicado en la aplicación de IoT Central.

1. Captura el modelo de funcionalidad del [repositorio público](https://aka.ms/ACFI) de modelos de funcionalidad publicados y certificados.

Con Visual Studio Code y el comando **Generate Device Code Stub** al que se hace referencia anteriormente, puede seguir estos pasos para conectar el dispositivo inicial e incorporar automáticamente el modelo de funcionalidad del dispositivo publicado desde el repositorio público en IoT Central:

1. Use un modelo de funcionalidad del dispositivo existente que se haya publicado en el repositorio público. Necesita el modelo de funcionalidad del dispositivo completo, así como anotar el URN de este modelo.

1. Siga los pasos anteriores para [generar una aplicación cliente de dispositivo](#generate-a-device-client-application) que use Visual Studio Code y genere el código del dispositivo.

1. Desde la aplicación de IoT Central, vaya a la pestaña **Administración** y seleccione la sección **Conexión de dispositivos**. Anote los valores de **Id. de ámbito** y **Clave principal** de la aplicación.

    > [!NOTE]
    > La **Clave principal** que se muestra en esta página es una firma de acceso compartido de grupo que se puede usar para generar varias claves de dispositivo para la aplicación.

1. Use la herramienta [dps-keygen](https://www.npmjs.com/package/dps-keygen) para generar una clave de dispositivo a partir de la clave principal que anotó anteriormente. Para generar la clave de dispositivo, ejecute el comando siguiente:

    ```cmd/sh
    dps-keygen -mk:<Primary_Key> -di:<device_id>
    ```

1. Siga los pasos de la sección anterior [Adición de detalles de conexión al cliente de dispositivo](#add-connection-details-to-the-device-client) para agregar el **Id. de ámbito**, la **Clave principal** y el **Identificador de dispositivo**.

1. Siga los pasos de la sección anterior para [Compilación y ejecución del cliente](#build-and-run-the-client).

1. Ahora verá que el dispositivo se conecta a la aplicación de IoT Central e incorpora automáticamente el modelo de funcionalidad del dispositivo desde el repositorio público como una plantilla de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar un dispositivo real a IoT Central, se sugiere como paso siguiente obtener más información acerca de las plantillas de dispositivo en [Set up a device template](howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Configuración de una plantilla de dispositivo).
