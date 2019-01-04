---
title: Administración de la mensajería de dispositivos en la nube de Azure IoT Hub con la extensión Azure IoT Hub Toolkit para Visual Studio Code | Microsoft Docs
description: Aprenda a usar la extensión Azure IoT Hub Toolkit para Visual Studio Code a fin de supervisar mensajes del dispositivo a la nube y enviar mensajes de la nube al dispositivo en Azure IoT Hub.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 7/20/2018
ms.author: junhan
ms.openlocfilehash: afb0a2ce4dfdd5d3a98a6dad03ee78eeaafbd308
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338386"
---
# <a name="use-azure-iot-hub-toolkit-extension-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Utilice la extensión Azure IoT Hub Toolkit para Visual Studio Code a fin de enviar y recibir mensajes entre el dispositivo e IoT Hub.

![Diagrama integral](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IoT Hub Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (anteriormente Azure IoT Toolkit) es una útil extensión de Visual Studio Code que facilita la administración de IoT Hub. Este artículo trata de cómo utilizar la extensión Azure IoT Hub Toolkit para Visual Studio Code a fin de enviar y recibir mensajes entre el dispositivo e IoT Hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Lo qué aprenderá

Aprenderá a usar la extensión Azure IoT Hub Toolkit para Visual Studio Code a fin de supervisar mensajes del dispositivo a la nube y enviar mensajes de la nube al dispositivo. Los mensajes del dispositivo a la nube podrían ser datos de sensor que el dispositivo recopila y, a continuación, envía al IoT Hub. Los mensajes de la nube al dispositivo podrían ser comandos que el IoT Hub envía al dispositivo para hacer parpadear un LED que está conectado al dispositivo.

## <a name="what-you-will-do"></a>Lo que hará

- Utilizará la extensión Azure IoT Hub Toolkit para Visual Studio Code a fin de supervisar los mensajes del dispositivo a la nube.
- Utilizará la extensión Azure IoT Hub Toolkit para Visual Studio Code a fin de enviar los mensajes de la nube al dispositivo.

## <a name="what-you-need"></a>Lo que necesita

- Una suscripción de Azure activa.
- Un centro de Azure IoT en su suscripción.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT Hub Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="sign-in-to-access-your-iot-hub"></a>Iniciar sesión para acceder a IoT Hub

1. En la vista **Explorador** de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) en la esquina inferior izquierda.
1. Haga clic en **Select IoT Hub** (Seleccionar IoT Hub) en el menú contextual.
1. Se mostrará una ventana emergente en la esquina inferior derecha que le permite iniciar sesión en Azure por primera vez.
1. Después de iniciar sesión, se mostrará la lista de suscripciones de Azure y luego podrá seleccionar la suscripción de Azure e IoT Hub.
1. En unos segundos, aparecerá la lista de dispositivos en la pestaña **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub).

   > [!Note]
   > También puede completar la configuración seleccionando **Set IoT Hub Connection String** (Establecer cadena de conexión de IoT Hub). Escriba la cadena de conexión del centro de IoT al que se conecta el dispositivo IoT en la ventana emergente.
   
## <a name="monitor-device-to-cloud-messages"></a>Supervisión de mensajes de dispositivo a nube

Para supervisar los mensajes que se envían desde el dispositivo al IoT Hub, siga estos pasos:

1. Haga clic con el botón derecho en el dispositivo y seleccione **Start Monitoring D2C Message** (Iniciar la supervisión del mensaje de D2C).
1. Se mostrarán los mensajes controlados en la vista **SALIDA** > **Azure IoT Hub Toolkit**.
1. Para detener la supervisión, haga clic con el botón derecho en la vista **SALIDA** y seleccione **Stop Monitoring D2C Message** (Detener la supervisión de mensaje de D2C).

## <a name="send-cloud-to-device-messages"></a>Envío de mensajes de nube a dispositivo

Para enviar un mensaje desde el IoT Hub al dispositivo, siga estos pasos:

1. Haga clic con el botón derecho en el dispositivo y seleccione **Send C2D Message to Device** (Enviar mensaje de C2D al dispositivo). 
1. Escriba el mensaje en el cuadro de entrada.
1. Se mostrarán los resultados en la vista **SALIDA** > **Azure IoT Hub Toolkit**.

## <a name="next-steps"></a>Pasos siguientes

Ahora sabe cómo supervisar los mensajes del dispositivo a la nube y enviar mensajes de la nube al dispositivo entre el dispositivo de IoT y la instancia de Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
