---
title: 'Registre un dispositivo nuevo desde Visual Studio Code: Azure IoT Edge | Microsoft Docs'
description: Uso de Visual Studio Code para crear un nuevo dispositivo IoT Edge en Azure IoT Edge y recuperar la cadena de conexión
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c8fce104d48acc3a562599c65eb15cb0a66657b7
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66495271"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Registrar un nuevo dispositivo de Azure IoT Edge desde Visual Studio Code

Para poder usar los dispositivos IoT con Azure IoT Edge, debe registrarlos con IoT Hub. Después de registrar un dispositivo, recibirá una cadena de conexión que puede usarse para configurar el dispositivo para cargas de trabajo de IoT Edge.

En este artículo se muestra cómo registrar un nuevo dispositivo IoT Edge mediante Visual Studio Code (VS Code). Hay varias maneras de realizar la mayoría de operaciones en VS Code. Este artículo usa el explorador, pero también puede utilizar la paleta de comandos para ejecutar los pasos.

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure.
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) para Visual Studio Code

## <a name="sign-in-to-access-your-iot-hub"></a>Iniciar sesión para acceder a IoT Hub

Puede usar las extensiones de Azure IoT para Visual Studio Code para realizar operaciones con IoT Hub. Para que estas operaciones para que funcione, deberá iniciar sesión en su cuenta de Azure y seleccione IoT hub.

1. En Visual Studio Code, abra la vista **Explorer** (Explorador).

1. En la parte inferior del explorador, expanda el **Azure IoT Hub** sección.

   ![Sección Expansión de los dispositivos de Azure IoT Hub](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

1. Haga clic en el **...**  en el **Azure IoT Hub** encabezado de sección. Si no ve el botón de los puntos suspensivos, mantenga el ratón sobre el encabezado o haga clic en él.

1. Elija **Select IoT Hub** (Seleccionar IoT Hub).

1. Si no has iniciado sesión en su cuenta de Azure, siga las indicaciones para hacerlo.

1. Seleccione su suscripción a Azure.

1. Seleccione IoT Hub.

## <a name="create-a-device"></a>Crear un dispositivo

1. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub).

1. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Si no ve el botón de los puntos suspensivos, mantenga el ratón sobre el encabezado o haga clic en él.

1. Seleccione **Create IoT Edge Device** (Crear un dispositivo de IoT Edge).

1. En el cuadro de texto que aparece, otorgue a su dispositivo un identificador.

En la pantalla de salida, consulte el resultado del comando. Se imprime la información del dispositivo, que incluye el valor de **deviceId** que proporcionó, y el valor de **connectionString** que puede usar para conectar su dispositivo físico a su instancia de IoT Hub.

## <a name="view-all-devices"></a>Ver todos los dispositivos

Se muestran todos los dispositivos que se conectan a IoT hub en el **Azure IoT Hub** sección del explorador de código de Visual Studio. Los dispositivos de IoT Edge son discernible de dispositivos sin bordes con un icono diferente y el hecho de que el **$edgeAgent** y **$edgeHub** módulos se implementan en cada dispositivo IoT Edge.

   ![Visualización de todos los dispositivos IoT Edge en su centro de IoT](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Recuperación de la cadena de conexión

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT.

1. Haga doble clic en el identificador del dispositivo en el **Azure IoT Hub** sección.

1. Seleccione **Copy Device Connection String** (Copiar la cadena de conexión del dispositivo).

   La cadena de conexión se copiará al portapapeles.

También puede seleccionar **Get Device Info** (Obtener información del dispositivo), si hace clic con el botón derecho del mouse en el menú para ver toda la información del dispositivo, incluyendo la cadena de conexión, en la ventana de salida.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar módulos en un dispositivo con Visual Studio Code](how-to-deploy-modules-vscode.md).
