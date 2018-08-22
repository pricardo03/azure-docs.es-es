---
title: Creación de una instancia de Azure IoT Hub mediante Azure IoT Toolkit para VS Code | Microsoft Docs
description: Cómo usar Azure IoT Toolkit para VS Code para crear una instancia de IoT Hub.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: af31f9375d6a41e13a9122e9730ba9532d3d52c5
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003076"
---
# <a name="create-an-iot-hub-using-the-azure-iot-toolkit-for-visual-studio-code"></a>Creación de una instancia de IoT Hub mediante Azure IoT Toolkit para Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Puede usar [Azure IoT Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para crear instancias de Azure IoT Hub. En este artículo se muestra cómo utilizar Azure IoT Toolkit para crear una instancia de IoT Hub.

Para completar este artículo, necesitará lo siguiente:

* Una cuenta de Azure activa.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

1. En Visual Studio Code, abra la vista **Explorer** (Explorador).

2. En la parte inferior de esta vista, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

   ![Expandir los dispositivos de Azure IoT Hub](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Si no ve el botón de los puntos suspensivos, mantenga el mouse sobre el encabezado. 

4. Seleccione **Crear IoT Hub**.

5. Se mostrará una ventana emergente en la esquina inferior derecha que le permite iniciar sesión en Azure por primera vez.

6. Seleccione una suscripción de Azure. 

7. Seleccione un grupo de recursos.

8. Seleccione una ubicación.

9. Seleccione un plan de tarifa.

10. Escriba un nombre único global para la instancia de IoT Hub.

11. Espere unos minutos hasta que se cree la instancia de IoT Hub.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado una instancia de IoT Hub mediante Azure IoT Toolkit para Visual Studio Code, quizás desee seguir explorando:

* [Utilice la extensión Azure IoT Toolkit para Visual Studio Code a fin de enviar y recibir mensajes entre el dispositivo e IoT Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).
* [Uso de la extensión Azure IoT Toolkit para Visual Studio Code en la administración de dispositivos de Azure IoT Hub](iot-hub-device-management-iot-toolkit.md)
* [Página wiki](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki) para Azure IoT Toolkit.
