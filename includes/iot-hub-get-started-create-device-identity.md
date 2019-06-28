---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 09/07/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b2bce9788006a564def9bd8c1375a85dc4184b67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814818"
---
## <a name="create-a-device-identity"></a>Creación de una identidad de dispositivo

En esta sección, se usa la CLI de Azure para crear una identidad del dispositivo para este tutorial. La CLI de Azure está preinstalada en [Azure Cloud Shell](~/articles/cloud-shell/overview.md) o puede [instalarla localmente](/cli/azure/install-azure-cli). Los identificadores de dispositivos distinguen mayúsculas de minúsculas.

1. Ejecute el siguiente comando en el entorno de línea de comandos donde utiliza la CLI de Azure para instalar la extensión de IoT:

    ```cmd/sh
    az extension add --name azure-cli-iot-ext
    ```

1. Si ejecuta la CLI de Azure localmente, utilice el siguiente comando para iniciar sesión en su cuenta de Azure (si usa Cloud Shell, ha iniciado sesión automáticamente y no tiene que ejecutar este comando):

    ```cmd/sh
    az login
    ```

1. Por último, cree una identidad del dispositivo denominada `myDeviceId` y recupere la cadena de conexión del dispositivo con estos comandos:

    ```cmd/sh
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Anote la cadena de conexión de dispositivo del resultado. La aplicación de dispositivo usa esta cadena de conexión de dispositivo para conectarse a su instancia de IoT Hub como un dispositivo.

<!-- images and links -->
