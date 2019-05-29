---
title: Aprovisionamiento de un dispositivo de TPM simulado para Azure IoT Hub mediante C | Microsoft Docs
description: En esta guía de inicio rápido se utilizan inscripciones individuales. En esta guía de inicio rápido creará y aprovisionará un dispositivo de TPM simulado mediante el SDK de dispositivos para C para Azure IoT Hub Device Provisioning Service.
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: ca6914967d855123c70bf746a9d68d2e045e76d9
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65908679"
---
# <a name="quickstart-provision-a-simulated-tpm-device-using-the-azure-iot-c-sdk"></a>Guía de inicio rápido: Aprovisionamiento de un dispositivo de TPM simulado mediante el SDK para C de Azure IoT

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

En este inicio rápido, aprenderá a crear y ejecutar un simulador de dispositivos de Módulo de plataforma segura (TPM) en una máquina de desarrollo Windows. Este dispositivo simulado lo conectará a un centro de IoT mediante una instancia del servicio Device Provisioning. Se usará código de ejemplo del [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c) para ayudar a inscribir el dispositivo en una instancia del servicio Device Provisioning y simular una secuencia de arranque para el dispositivo.

Si no está familiarizado con el proceso de aprovisionamiento automático, revise los [Conceptos sobre aprovisionamiento automático](concepts-auto-provisioning.md). Además, asegúrese de completar los pasos descritos en [Configuración del servicio Azure IoT Hub Device Provisioning con Azure Portal](./quick-setup-auto-provision.md) antes de continuar con este inicio rápido. 

Azure IoT Hub Device Provisioning Service admite dos tipos de inscripciones:
- [Grupos de inscripción](concepts-service.md#enrollment-group): usados para inscribir varios dispositivos relacionados.
- [Inscripciones individuales](concepts-service.md#individual-enrollment): usadas para inscribir un solo dispositivo.

En este artículo se mostrarán las inscripciones individuales.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 o posterior con la carga de trabajo ["Desarrollo para el escritorio con C++"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) habilitada.
* Tener instalada la versión más reciente de [Git](https://git-scm.com/download/).


<a id="setupdevbox"></a>

## <a name="prepare-a-development-environment-for-the-azure-iot-c-sdk"></a>Preparación de un entorno de desarrollo para el SDK para C de Azure IoT

En esta sección, preparará un entorno de desarrollo para compilar el [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c) y el simulador de dispositivos de [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview) de ejemplo.

1. Descargue el [sistema de compilación CMake](https://cmake.org/download/).

    **Antes** de comenzar la instalación de `CMake`, es importante que los requisitos previos de Visual Studio (Visual Studio y la carga de trabajo de desarrollo de escritorio con C++) estén instalados en la máquina. Una vez que los requisitos previos están en su lugar, y se ha comprobado la descarga, instale el sistema de compilación de CMake.

2. Abra un símbolo del sistema o el shell de Bash de Git. Ejecute el siguiente comando para clonar el repositorio de GitHub del [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c):
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Esta operación puede tardar varios minutos en completarse.


3. Cree un subdirectorio `cmake` en el directorio raíz del repositorio de Git y vaya a esa carpeta. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

## <a name="build-the-sdk-and-run-the-tpm-device-simulator"></a>Compilación del SDK y ejecución del simulador de dispositivos de TPM

En esta sección, compilará el SDK para C de Azure IoT, que incluye el código de ejemplo del simulador de dispositivos de TPM. Este ejemplo proporciona un [mecanismo de atestación](concepts-security.md#attestation-mechanism) de TPM mediante la autenticación del token de firma de acceso compartido (SAS).

1. Desde el subdirectorio `cmake` que creó en el repositorio de Git azure-iot-sdk-c, ejecute el siguiente comando para compilar el ejemplo. Mediante este comando de compilación también se genera una solución de Visual Studio para el dispositivo simulado.

    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
    ```

    Si `cmake` no encuentra el compilador de C++, es posible que obtenga errores de compilación durante la ejecución del comando anterior. Si sucede, intente ejecutar este comando en el [símbolo del sistema de Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Una vez realizada la compilación, las últimas líneas de salida tendrán un aspecto similar al siguiente:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

2. Vaya a la carpeta raíz del repositorio de Git que ha clonado y ejecute el simulador de [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview) mediante la ruta de acceso que se muestra a continuación. Este simulador escucha a través de un socket en los puertos 2321 y 2322. No cierre esta ventana de comandos; debe mantener este simulador ejecutándose hasta el final de este inicio rápido. 

   Si se encuentra en la carpeta *cmake*, ejecute los siguientes comandos:

    ```cmd/sh
    cd ..
    .\provisioning_client\deps\utpm\tools\tpm_simulator\Simulator.exe
    ```

    No verá ninguna salida del simulador. Déjelo que siga ejecutando la simulación de un dispositivo de TPM.

<a id="simulatetpm"></a>

## <a name="read-cryptographic-keys-from-the-tpm-device"></a>Lectura de las claves criptográficas desde el dispositivo de TPM

En esta sección, compilará y ejecutará un ejemplo que leerá la clave de aprobación y el identificador de registro del simulador de TPM que dejó ejecutándose y escuchando a través de los puertos 2321 y 2322. Estos valores se usarán para la inscripción de dispositivos en la instancia del servicio Device Provisioning.

1. Inicie Visual Studio y abra el nuevo archivo de solución denominado `azure_iot_sdks.sln`. Este archivo de solución se encuentra en la carpeta `cmake` que creó anteriormente en la raíz del repositorio de Git azure-iot-sdk-c.

2. En el menú de Visual Studio, seleccione **Compilar** > **Compilar solución** para compilar todos los proyectos de la solución.

3. En el *Explorador de soluciones* de Visual Studio, vaya a la carpeta **Aprovisionar\_Herramientas**. Haga clic con el botón derecho en el proyecto **tpm_device_provision** y seleccione **Establecer como proyecto de inicio**. 

4. En el menú de Visual Studio, seleccione **Depurar** > **Iniciar sin depurar** para ejecutar la solución. La aplicación lee y muestra un  **_id. de registro_** y una **_clave de aprobación_** . Copie estos valores. Se usarán en la sección siguiente para la inscripción del dispositivo. 


<a id="portalenrollment"></a>

## <a name="create-a-device-enrollment-entry-in-the-portal"></a>Creación de una entrada de inscripción de dispositivo en el portal

1. Inicie sesión en Azure Portal, haga clic en el botón **Todos los recursos** situado en el menú izquierdo y abra el servicio Device Provisioning.

2. Seleccione la pestaña **Administrar inscripciones** y luego haga clic en el botón **Add individual enrollment** (Agregar inscripción individual) de la parte superior. 

3. En **Agregar inscripción**, escriba la información siguiente y haga clic en el botón **Guardar**.

    - **Mecanismo:** Seleccione **TPM** como *Mecanismo* de atestación de identidad.
    - **Clave de aprobación:** escriba la *clave de aprobación* que generó para el dispositivo de TPM mediante la ejecución del proyecto *tpm_device_provision*.
    - **Id. de registro:** escriba el *id. de registro* que generó para el dispositivo de TPM mediante la ejecución del proyecto *tpm_device_provision*.
    - **Dispositivo de IoT Edge:** Seleccione **Deshabilitar**.
    - **Id. de dispositivo IoT Hub:** escriba **test-docs-device** para proporcionar al dispositivo un identificador.

      ![Especificación de la información de inscripción del dispositivo en el portal](./media/quick-create-simulated-device/enter-device-enrollment.png)  

      Al inscribir el dispositivo correctamente, el *id. del registro* del dispositivo aparecerá en la lista de la pestaña *Individual Enrollments* (Inscripciones individuales). 


<a id="firstbootsequence"></a>

## <a name="simulate-first-boot-sequence-for-the-device"></a>Simulación de la primera secuencia de arranque para el dispositivo

En esta sección, configurará código de ejemplo para usar [Advanced Message Queuing Protocol (AMQP)](https://wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) para enviar la secuencia de arranque del dispositivo a la instancia del servicio Device Provisioning. Esta secuencia de arranque hará que el dispositivo se reconozca y se asigne a un centro de IoT vinculado a la instancia del servicio Device Provisioning.

1. En Azure Portal, seleccione la pestaña **Información general** del servicio Device Provisioning y copie el valor de **_Ámbito de id_** .

    ![Extracción de información del punto de conexión del servicio Device Provisioning desde el portal](./media/quick-create-simulated-device/extract-dps-endpoints.png) 

2. En la ventana del *Explorador de soluciones* de Visual Studio, desplácese hasta la carpeta **Aprovisionar\_Ejemplos**. Expanda el proyecto de ejemplo denominado **prov\_dev\_client\_sample**. Expanda **Archivos de código fuente** y abra **prov\_dev\_cliente\_sample.c**.

3. Cerca de la parte superior del archivo, busque las instrucciones `#define` para cada protocolo de dispositivo, tal como se muestra a continuación. Asegúrese de que solo `SAMPLE_AMQP` tenga quitada la marca de comentario.

    Actualmente, el protocolo [MQTT no se admite para la inscripción individual de TPM](https://github.com/Azure/azure-iot-sdk-c#provisioning-client-sdk).

    ```c
    //
    // The protocol you wish to use should be uncommented
    //
    //#define SAMPLE_MQTT
    //#define SAMPLE_MQTT_OVER_WEBSOCKETS
    #define SAMPLE_AMQP
    //#define SAMPLE_AMQP_OVER_WEBSOCKETS
    //#define SAMPLE_HTTP
    ```

4. Busque la constante `id_scope` y reemplace el valor por el valor de **Ámbito de id.** que copió anteriormente. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Busque la definición de la función `main()` en el mismo archivo. Asegúrese de que la variable `hsm_type` está establecida en `SECURE_DEVICE_TYPE_TPM` y no en `SECURE_DEVICE_TYPE_X509`, tal como se muestra a continuación.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

6. Haga clic con el botón derecho en el proyecto **prov\_dev\_client\_sample** y seleccione **Set as Startup Project** (Establecer como proyecto de inicio). 

7. En el menú de Visual Studio, seleccione **Depurar** > **Iniciar sin depurar** para ejecutar la solución. En la solicitud para volver a compilar el proyecto, haga clic en **Sí** para recompilar el proyecto antes de ejecutarlo.

    La salida siguiente es un ejemplo del cliente de dispositivo de aprovisionamiento de ejemplo que ha arrancado correctamente y se conecta a la instancia del servicio Device Provisioning para obtener información de IoT Hub y registrarse:

    ```cmd
    Provisioning API Version: 1.2.7
    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED

    Registering... Press enter key to interrupt.

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service:
    test-docs-hub.azure-devices.net, deviceId: test-docs-device
    ```

8. Una vez que el dispositivo simulado se aprovisiona para el centro de IoT mediante el servicio de aprovisionamiento, el identificador del dispositivo aparece con los **dispositivos IoT** del centro. 

    ![El dispositivo se registra con el centro de IoT](./media/quick-create-simulated-device/hub-registration.png) 


## <a name="clean-up-resources"></a>Limpieza de recursos

Si piensa seguir trabajando con el ejemplo de cliente de dispositivo y explorándolo, no limpie los recursos que se crean en esta guía de inicio rápido. Si no va a continuar, use el siguiente comando para eliminar todos los recursos creados.

1. Cierre la ventana de salida de ejemplo del cliente del dispositivo en su máquina.
2. Cierre la ventana del simulador de TPM en su máquina.
3. Desde el menú de la izquierda en Azure Portal, haga clic en **Todos los recursos** y seleccione el servicio Device Provisioning. Abra la hoja **Administrar inscripciones** de su servicio y, a continuación, haga clic en la pestaña **Inscripciones individuales**. Seleccione el *ID. DE REGISTRO* del dispositivo que inscribió en esta guía de inicio rápido y haga clic en el botón **Eliminar** situado en la parte superior. 
4. Desde el menú de la izquierda en Azure Portal, haga clic en **Todos los recursos** y seleccione su centro de IoT. Abra la hoja **Dispositivos IoT** de su centro, seleccione el *ID. DE DISPOSITIVO* del dispositivo que registró en este inicio rápido y, luego, haga clic en el botón **Eliminar** situado en la parte superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un dispositivo de TPM simulado en su máquina y lo ha aprovisionado con IoT Hub mediante el servicio Azure IoT Hub Device Provisioning. Para obtener información sobre cómo inscribir el dispositivo de TPM mediante programación, siga la guía de inicio rápido para la inscripción de un dispositivo de TPM mediante programación. 

> [!div class="nextstepaction"]
> [Guía de inicio rápido de Azure: Inscripción de dispositivos de TPM al Servicio Azure IoT Hub Device Provisioning](quick-enroll-device-tpm-java.md)

