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
ms.openlocfilehash: 9f7fee71fb2b80be93d978569791dbb57f137949
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346841"
---
## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Ahora que ha creado un centro de IoT, busque la información importante que usa para conectar a él dispositivos y aplicaciones. 

En el menú de navegación de IoT Hub, abra **Directivas de acceso compartido**. Seleccione la directiva **iothubowner** y, a continuación, copie el valor de **Cadena de conexión: clave principal** de su IoT Hub. Para más información, consulte [Control del acceso a IoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).

   > [!NOTE] 
   > En este tutorial de configuración, no necesita la cadena de conexión iothubowner. Sin embargo, puede necesitarla en algunos de los tutoriales o en diferentes escenarios de IoT después de realizar esta configuración.

   ![Obtener la cadena de conexión del centro de IoT](./media/iot-hub-get-started-create-hub-and-device/create-iot-hub5.png)

## <a name="register-your-device-in-the-iot-hub"></a>Registro del dispositivo en IoT Hub

1. En el menú de navegación de IoT Hub, abra **Dispositivos de IoT** y, a continuación, haga clic en **Agregar** para registrar un dispositivo en su IoT Hub.

   ![Agregar un dispositivo en Dispositivos de IoT de su IoT Hub](./media/iot-hub-get-started-create-hub-and-device/create-identity-portal.png)

2. Escriba un **id. de dispositivo** para el dispositivo nuevo. Los identificadores de dispositivos distinguen mayúsculas de minúsculas.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

3. Haga clic en **Save**(Guardar).

4. Después de crear el dispositivo, ábralo desde la lista del panel **Dispositivos de IoT**.

5. Copie el valor de **Cadena de conexión: clave principal** para usarlo más adelante.

   ![Obtener la cadena de conexión del dispositivo](./media/iot-hub-get-started-create-hub-and-device/device-connection-string.png)
