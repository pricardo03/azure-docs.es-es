---
title: 'Registro de un dispositivo nuevo desde la línea de comandos: Azure IoT Edge | Microsoft Docs'
description: Uso de la extensión de IoT para la CLI de Azure para registrar un dispositivo IoT Edge nuevo y recuperar la cadena de conexión
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: de00c317483da9bcd93bb2e2505350d787385925
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66495391"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli"></a>Registro de un nuevo dispositivo Azure IoT Edge con la CLI de Azure

Para poder usar los dispositivos IoT con Azure IoT Edge, debe registrarlos con IoT Hub. Una vez que registre un dispositivo, recibirá una cadena de conexión que se puede usar para configurar el dispositivo para cargas de trabajo de IoT Edge.

La [CLI de Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) es una herramienta de la línea de comandos multiplataforma y de código abierto para la administración de recursos de Azure, como IoT Edge. Esta permite administrar recursos de Azure IoT Hub, instancias de servicio de aprovisionamiento de dispositivos y centros vinculados listos para usar. La extensión de IoT enriquece la CLI de Azure con características como administración de dispositivos y funcionalidad completa de IoT Edge.

En este artículo se muestra cómo registrar un nuevo dispositivo IoT Edge mediante la CLI de Azure.

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) en la suscripción de Azure.
* La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) en su entorno. La versión mínima de la CLI de Azure es la 2.0.24. Use `az –-version` para asegurarse. Esta versión admite comandos az extension e introduce la plataforma de comandos de Knack.
* La [extensión de IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Crear un dispositivo

Use el comando [az iot hub device-identity create](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create) para crear una nueva identidad de dispositivo en su centro de IoT. Por ejemplo:

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Este comando incluye tres parámetros:

* **device-id**: proporcione un nombre descriptivo que sea único de su centro de IoT.

* **hub-name**: proporcione el nombre de su centro de IoT.

* **edge-enabled**: este parámetro declara que el dispositivo se va a usar con IoT Edge.

   ![az iot hub device-identity create output](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Ver todos los dispositivos

Use el comando [az iot hub device-identity list](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-list) para ver todos los dispositivos en su centro de IoT. Por ejemplo:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Los dispositivos que están registrados como dispositivos IoT Edge tendrán la propiedad **capabilities.iotEdge** establecida en **true**.

## <a name="retrieve-the-connection-string"></a>Recuperación de la cadena de conexión

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT. Use el comando [az iot hub device-identity show-connection-string](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-show-connection-string) para devolver la cadena de conexión para un único dispositivo:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

El valor del parámetro `device-id` distingue mayúsculas y minúsculas. No copie las comillas alrededor de la cadena de conexión.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar módulos en un dispositivo con la CLI de Azure](how-to-deploy-modules-cli.md).
