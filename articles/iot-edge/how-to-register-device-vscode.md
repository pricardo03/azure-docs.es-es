---
title: Registrar un nuevo dispositivo de Azure IoT Edge (VS Code) | Microsoft Docs
description: Use Visual Studio Code para crear un nuevo dispositivo de IoT Edge en Azure IoT Hub
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7902461f58df1b4fe0c3ed3b577f668fe8be4cc2
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035435"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Registrar un nuevo dispositivo de Azure IoT Edge desde Visual Studio Code

Para poder usar los dispositivos IoT con Azure IoT Edge, debe registrarlos con IoT Hub. Una vez que registre un dispositivo, recibirá una cadena de conexión que se puede usar para configurar el dispositivo para cargas de trabajo de Edge. 

En este artículo se muestra cómo registrar un nuevo dispositivo IoT Edge mediante Visual Studio Code (VS Code). Hay varias maneras de realizar la mayoría de operaciones en VS Code. En este artículo usa Explorer, pero también puede usar la paleta de comandos para ejecutar la mayoría de los pasos. 

## <a name="prerequisites"></a>requisitos previos

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure.
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code.

## <a name="sign-in-to-access-your-iot-hub"></a>Iniciar sesión para acceder a IoT Hub

Puede usar las extensiones de Azure IoT para Visual Studio Code para realizar operaciones con IoT Hub. Para que estas operaciones funcionen, debe iniciar sesión en su cuenta de Azure y seleccionar la instancia de IoT Hub en la que está trabajando.

1. En Visual Studio Code, abra la vista **Explorer** (Explorador).

2. En la parte inferior de esta vista, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

   ![Expandir los dispositivos de Azure IoT Hub](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

3. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Si no ve el botón de los puntos suspensivos, mantenga el mouse sobre el encabezado. 

4. Elija **Select IoT Hub** (Seleccionar IoT Hub).

5. Si no ha iniciado sesión en su cuenta de Azure, siga las indicaciones para hacerlo. 

6. Seleccione su suscripción a Azure. 

7. Seleccione IoT Hub. 

## <a name="create-a-device"></a>Crear un dispositivo

1. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

2. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Si no ve el botón de los puntos suspensivos, mantenga el mouse sobre el encabezado. 

3. Seleccione **Create IoT Edge Device** (Crear un dispositivo de IoT Edge). 

4. En el cuadro de texto que aparece, otorgue a su dispositivo un identificador. 

En la pantalla de salida, consulte el resultado del comando. Se imprime la información del dispositivo, que incluye el valor de **deviceId** que proporcionó, y el valor de **connectionString** que puede usar para conectar su dispositivo físico a su instancia de IoT Hub. 

## <a name="view-all-devices"></a>Ver todos los dispositivos

Todos los dispositivos que se conectan a IoT Hub se enumeran en la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) del explorador de Visual Studio Code. Los dispositivos de IoT Edge se distinguen de los dispositivos que no son Edge con un icono diferente; además, se pueden expandir para mostrar los módulos implementados en cada dispositivo. 

   ![Ver dispositivos en VS Code](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Recuperación de la cadena de conexión

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT.

1. Haga clic con el botón derecho del mouse en el id. del dispositivo en la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 
2. Seleccione **Copy Device Connection String** (Copiar la cadena de conexión del dispositivo).

   La cadena de conexión se copiará al portapapeles. 

También puede seleccionar **Get Device Info** (Obtener información del dispositivo), si hace clic con el botón derecho del mouse en el menú para ver toda la información del dispositivo, incluyendo la cadena de conexión, en la ventana de salida. 


## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar módulos en un dispositivo con Visual Studio Code](how-to-deploy-modules-vscode.md).
