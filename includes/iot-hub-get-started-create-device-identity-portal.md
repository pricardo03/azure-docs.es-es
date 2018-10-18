---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: ffd5da239f8e271a8c9b2aaf3f6d5fd9f885c79c
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400187"
---
## <a name="create-a-device-identity"></a>Creación de una identidad de dispositivo

En esta sección, usará [Azure Portal](https://portal.azure.com) para crear una identidad de dispositivo en el registro de identidades de su instancia de IoT Hub. No se puede conectar un dispositivo a IoT Hub a menos que tenga una entrada en el registro de identidades. Para más información, consulte la sección sobre el registro de identidades de la [guía del desarrollador de IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md). Use el panel **Dispositivos de IoT** en el portal para generar un identificador y una clave de dispositivo únicos para que su dispositivo se identifique en IoT Hub. Los identificadores de dispositivos distinguen mayúsculas de minúsculas.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)

2. Seleccione **Todos los recursos** y busque su recurso de IoT Hub.

3. Cuando se abra el recurso de IoT Hub, haga clic en la herramienta **Dispositivos IoT** y, a continuación, haga clic en **Agregar** en la parte superior. 

    ![Creación de la identidad del dispositivo en el portal](./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png)

4. Proporcione un nombre para el dispositivo nuevo, como **myDeviceId**, y haga clic en **Guardar**. Esta acción crea una nueva identidad de dispositivo para su instancia de IoT Hub.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Agregar un nuevo dispositivo](./media/iot-hub-get-started-create-device-identity-portal/create-a-device.png)

5. En la lista de dispositivos, haga clic en el dispositivo recién creado y copie el valor de **Cadena de conexión: clave principal** para usarlo más adelante.

    ![Cadena de conexión de dispositivo](./media/iot-hub-get-started-create-device-identity-portal/device-details.png)

> [!NOTE]
> El registro de identidades de IoT Hub solo almacena identidades de dispositivos para permitir el acceso seguro al centro de IoT. Almacena las claves y los identificadores de dispositivo para usarlos como credenciales de seguridad, y un indicador de habilitado o deshabilitado que permite deshabilitar el acceso a un dispositivo individual. Si la aplicación necesita almacenar otros metadatos específicos del dispositivo, debe usar un almacén específico de la aplicación. Consulte la [guía de desarrolladores de IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md) para obtener más información.
