---
title: Creación de una instancia de Azure IoT Hub mediante Azure IoT Hub Toolkit para VS Code | Microsoft Docs
description: Uso de Azure IoT Hub Toolkit para VS Code para crear una instancia de IoT Hub.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: cee71ddfbf1a20cc7417976d60b04bff6f0deac8
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53339032"
---
# <a name="create-an-iot-hub-using-the-azure-iot-hub-toolkit-for-visual-studio-code"></a>Creación de una instancia de IoT Hub mediante Azure IoT Hub Toolkit para Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Este artículo se muestra cómo usar [Azure IoT Hub Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (antes Azure IoT Toolkit) para crear una instancia de Azure IoT Hub. 

Para completar este artículo, necesitará lo siguiente:

- Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Azure IoT Hub Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

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

Ya ha implementado una instancia de IoT Hub mediante Azure IoT Hub Toolkit para Visual Studio Code. Para más información, consulte los artículos siguientes:

* [Uso de la extensión Azure IoT Hub Toolkit para Visual Studio Code para enviar y recibir mensajes entre un dispositivo e IoT Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md)

* [Uso de la extensión Azure IoT Hub Toolkit para Visual Studio Code en la administración de dispositivos de Azure IoT Hub](iot-hub-device-management-iot-toolkit.md)

* [Página wiki de Azure IoT Hub Toolkit](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki)
