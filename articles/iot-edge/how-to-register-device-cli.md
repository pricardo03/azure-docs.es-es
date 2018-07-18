---
title: Registro de un nuevo dispositivo Azure IoT Edge (CLI) | Microsoft Docs
description: Uso de la extensión de IoT de la CLI de Azure 2.0 para registrar un nuevo dispositivo IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 05/30/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a6935dfe5b32364e402017cd3005ab17969ce1ce
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035322"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli-20"></a>Registro de un nuevo dispositivo Azure IoT Edge con la CLI de Azure 2.0

Para poder usar los dispositivos IoT con Azure IoT Edge, debe registrarlos con el centro de IoT. Una vez que registra un dispositivo, recibe una cadena de conexión que se puede usar para configurar el dispositivo para cargas de trabajo de Edge. 

La [CLI de Azure 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) es una herramienta de línea de comandos multiplataforma y de código abierto para la administración de recursos de Azure, como IoT Edge. Esta permite administrar recursos de Azure IoT Hub, instancias de servicio de aprovisionamiento de dispositivos y centros vinculados listos para usar. La nueva extensión de IoT enriquece la CLI de Azure 2.0 con características como la administración de dispositivos y funcionalidad completa de IoT Edge.

En este artículo se muestra cómo registrar un nuevo dispositivo IoT Edge mediante la CLI de Azure 2.0.

## <a name="prerequisites"></a>requisitos previos

* Un [centro de IoT](../iot-hub/iot-hub-create-using-cli.md) en la suscripción de Azure. 
* [CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) en su entorno. La versión mínima de la CLI de Azure 2.0 debe ser la 2.0.24. Use `az –-version` para realizar la validacion. Esta versión admite comandos az extension e introduce la plataforma de comandos de Knack. 
* [La extensión de IoT para la CLI de Azure 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Creación de un dispositivo

Use el siguiente comando para crear una nueva identidad de dispositivo en su centro de IoT: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Este comando incluye tres parámetros:
* **device-id**: proporcione un nombre descriptivo que sea único de su centro de IoT.
* **hub-name**: proporcione el nombre de su centro de IoT.
* **edge-enabled**: este parámetro declara que el dispositivo se va a usar con IoT Edge.

   ![Crear dispositivo de IoT Edge](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Visualización de todos los dispositivos

Use el siguiente comando para ver todos los dispositivos de su centro de IoT:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Los dispositivos que están registrados como dispositivos IoT Edge tendrán la propiedad **capabilities.iotEdge** establecida en **true**. 

## <a name="retrieve-the-connection-string"></a>Recuperación de la cadena de conexión

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT. Use el siguiente comando para devolver la cadena de conexión de un solo dispositivo:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

El parámetro "device id" distingue mayúsculas y minúsculas. 

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar módulos en un dispositivo con la CLI de Azure 2.0](how-to-deploy-modules-cli.md).