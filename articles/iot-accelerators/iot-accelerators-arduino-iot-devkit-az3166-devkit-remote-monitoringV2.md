---
title: 'IoT DevKit en la nube: conexión de IoT DevKit AZ3166 al acelerador de la solución de IoT Supervisión remota | Microsoft Docs'
description: En este tutorial, aprenderá a enviar el estado de los sensores de IoT DevKit AZ3166 al acelerador de la solución de IoT Supervisión remota para su supervisión y visualización.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: isacabe
ms.openlocfilehash: 32742b2a680370f443051e2d86f90d94e8632850
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720589"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>Conexión de MXChip IoT DevKit AZ3166 al acelerador de la solución Supervisión remota de IoT

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Aprenderá a ejecutar una aplicación de ejemplo en el IoT DevKit para enviar datos de sensor al acelerador de la solución.

[MXChip IoT DevKit](https://aka.ms/iot-devkit) es una placa integral compatible de Arduino con periféricos y sensores varios. Se puede desarrollar para ella con [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) para Visual Studio Code. Incluye un [catálogo de proyectos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) cada vez mayor que sirve de guía para crear prototipos de soluciones de Internet de las cosas (IoT) que aprovechen los servicios de Microsoft Azure.

## <a name="what-you-need"></a>Lo que necesita

Repase la [Guía de introducción](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) y **finalice las siguientes secciones**:

* Preparar el hardware
* Configuración de Wi-Fi
* Empezar a usar el DevKit
* Preparación del entorno de desarrollo

## <a name="open-the-remote-monitoring-sample-in-vs-code"></a>Abra el ejemplo de supervisión remota en VS Code

1. Asegúrese de que IoT DevKit **no está conectado** al equipo. Primero, inicie VS Code y luego conecte DevKit al equipo.

2. Presione `F1` para abrir la paleta de comandos y escriba y seleccione **IoT Workbench: Examples** (IoT Workbench: Ejemplos). A continuación, seleccione **IoT DevKit** como placa.

3. Busque **Supervisión remota** y haga clic en **Open Sample** (Abrir ejemplo). Se abre una nueva ventana de VS Code con la carpeta del proyecto.
  ![IoT Workbench, seleccione el ejemplo de Supervisión remota](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-iot-hub-device-connection-string"></a>Configuración de la cadena de conexión de un dispositivo IoT Hub

1. Cambie el IoT DevKit al **modo de configuración**. Para ello:
   * Mantenga presionado el botón **A**.
   * Presione y suelte el botón de **restablecimiento**.

2. La pantalla muestra el identificador de DevKit y "Configuration" (Configuración).
   
  ![Modo de configuración de IoT DevKit](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

3. Presione `F1` para abrir la paleta de comandos y escriba y seleccione **IoT Workbench: Device > Config Device Settings** (IoT Workbench: Dispositivo > Configurar opciones de dispositivo).

4. Pegue la cadena de conexión que acaba de copiar y haga clic en `Enter` para configurarla.

## <a name="build-and-upload-the-device-code"></a>Creación y carga del código del dispositivo

1. Presione `F1` para abrir la paleta de comandos y escriba y seleccione **IoT Workbench: Device > Device Upload** (IoT Workbench: Dispositivo > Carga de dispositivo).
  ![IoT Workbench: Device - > Upload](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-upload.png)

1. A continuación, VS Code comienza a compilar y cargar el código en DevKit.
  ![IoT Workbench: Device - > Uploaded](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

El DevKit se reinicia y comienza a ejecutar el código.

## <a name="test-the-project"></a>Prueba del proyecto

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>Visualización de los datos de telemetría enviados a la solución de Supervisión remota

Cuando se ejecuta la aplicación de ejemplo, DevKit envía datos de sensor vía Wi-Fi a la solución de Supervisión remota. Para ver el resultado, siga estos pasos:

1. Vaya al panel de la solución y haga clic en **Devices** (Dispositivos).

2. Haga clic en el nombre del dispositivo, en la pestaña de la derecha, puede ver el estado del sensor de DevKit en tiempo real.
  ![Datos del sensor en Azure IoT Suite](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="send-a-c2d-message"></a>Envío de un mensaje C2D

La solución de Supervisión remota permite invocar un método remoto en el dispositivo. El código de ejemplo publica tres métodos que se pueden ver en la sección **Method** (Método) cuando se selecciona el sensor.

![Métodos de IoT DevKit](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

Probemos cambiar el color de uno de los LED de DevKit con el método "LedColor".

1. Seleccione el nombre del dispositivo en la lista de dispositivos y haga clic en **Jobs** (Trabajos).

  ![Creación de un trabajo](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

2. Configure los trabajos como se indica a continuación y haga clic en **Apply** (Aplicar).
  * Select Job (Seleccionar trabajo): **Run method** (Método de ejecución)
  * Method name (Nombre del método): **LedColor**
  * Job Name (Nombre del trabajo): **ChangeLedColor**
  
  ![Configuración del trabajo](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

En unos segundos, el LED RGB de DevKit debería cambiar de color (debajo del botón A).

![LED rojo de IoT DevKit](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si va a pasar a los tutoriales, deje el acelerador de soluciones de supervisión remota implementado.

Si ya no necesita el acelerador de soluciones, elimínelo de la página Soluciones aprovisionadas. Para ello, selecciónelo y, a continuación, haga clic en Eliminar solución:

![Eliminación de la solución](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Problemas y comentarios

Si tiene problemas, consulte las [preguntas frecuentes de IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) o póngase en contacto con nosotros mediante los siguientes canales:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo conectar un dispositivo DevKit al acelerador de la solución de supervisión remota de Azure IoT y visualizar los datos de sensor, estos son los siguientes pasos sugeridos:

* [Introducción a los aceleradores de la solución de Azure IoT](https://docs.microsoft.com/azure/iot-suite/)
* [Personalización de la interfaz de usuario](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md)
* [Conexión de IoT DevKit a una aplicación de Azure IoT Central](../iot-central/howto-connect-devkit.md)