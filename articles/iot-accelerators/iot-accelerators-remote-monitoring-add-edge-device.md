---
title: 'Adición de un dispositivo Edge a una solución de supervisión remota: Azure | Microsoft Docs'
description: En este artículo se describe cómo agregar un dispositivo IoT Edge a un acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/09/2018
ms.topic: conceptual
ms.openlocfilehash: d34ac159a216c5c77214b4c8b799a233c3671235
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/24/2019
ms.locfileid: "56749552"
---
# <a name="add-an-iot-edge-device-to-your-remote-monitoring-solution-accelerator"></a>Adición de un dispositivo IoT Edge al acelerador de soluciones de supervisión remota

Para agregar un dispositivo [IoT Edge](../iot-edge/about-iot-edge.md) al acelerador de soluciones, complete los dos pasos siguientes:

1. Agregar el dispositivo de Edge en el **Device Explorer** página en la supervisión remota solución Acelerador de la interfaz de usuario web.
1. Instale el entorno de ejecución de Azure IoT Edge en el dispositivo Edge.

## <a name="add-the-iot-edge-device"></a>Adición del dispositivo IoT Edge

Para agregar un dispositivo IoT Edge al acelerador de soluciones de supervisión remota, vaya a la página **Device Explorer** de la interfaz de usuario web y haga clic en **+ Nuevo dispositivo**.

En el panel **Nuevo dispositivo**, elija **Dispositivo IoT Edge**. Puede dejar los valores predeterminados para las demás opciones de configuración. A continuación, haga clic en **Aplicar**:

![Adición de un dispositivo IoT Edge](media/iot-accelerators-remote-monitoring-add-edge-device/addedgedevice.png)

### <a name="alternative-ways-to-add-an-iot-edge-device"></a>Maneras alternativas de agregar un dispositivo IoT Edge

También es posible registrar un dispositivo IoT Edge directamente con la instancia de IoT Hub en el acelerador de soluciones. Es preciso conocer el nombre del IoT Hub en el acelerador de soluciones antes de seguir cualquiera de estas guías de procedimientos:

- [Registro de un nuevo dispositivo Azure IoT Edge desde Azure Portal](../iot-edge/how-to-register-device-portal.md)
- [Registro de un nuevo dispositivo Azure IoT Edge con la CLI de Azure](../iot-edge/how-to-register-device-cli.md)
- [Registrar un nuevo dispositivo de Azure IoT Edge desde Visual Studio Code](../iot-edge/how-to-register-device-vscode.md)

Al registrar un dispositivo directamente con IoT hub en el Acelerador de solución de supervisión remota, se muestra en el **Device Explorer** página en la interfaz de usuario web.

## <a name="install-the-iot-edge-runtime"></a>Instalación del entorno de ejecución de IoT Edge

Antes de poder implementar módulos en su dispositivo Edge, debe instalar el entorno de ejecución de IoT Edge en el dispositivo real. En las guías de procedimientos siguientes se muestra cómo instalar el entorno de ejecución en las plataformas de dispositivo más comunes:

- [Instalar el entorno de ejecución de Azure IoT Edge en Linux (x64)](../iot-edge/how-to-install-iot-edge-linux.md)
- [Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)](../iot-edge/how-to-install-iot-edge-linux-arm.md)
- [Instalación del entorno de ejecución de Azure IoT Edge en Windows para usar con contenedores de Windows](../iot-edge/how-to-install-iot-edge-windows-with-windows.md)
- [Instalación del entorno de ejecución de Azure IoT Edge en Windows para su uso con contenedores Linux](../iot-edge/how-to-install-iot-edge-windows-with-linux.md)
- [Instalación de la instancia de IoT Edge en tiempo de ejecución en un dispositivo Windows IoT Core](../iot-edge/how-to-install-iot-core.md)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha preparado el dispositivo IoT Edge, el siguiente paso es implementar módulos en él. Vea [Importación de un paquete de IoT Edge en el acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-import-edge-package.md)
