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
ms.openlocfilehash: 935e2b9e861a889bef48c1d7ba119ab694cddfb3
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094227"
---
## <a name="create-a-device-identity"></a>Creación de una identidad de dispositivo

En esta sección, usará [Azure Portal](https://portal.azure.com) para crear una identidad de dispositivo en el registro de identidades de su instancia de IoT Hub. No se puede conectar un dispositivo a IoT Hub a menos que tenga una entrada en el registro de identidades. Para más información, consulte la sección sobre el registro de identidades de la [guía del desarrollador de IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md). Use el panel **Dispositivos de IoT** en el portal para generar un identificador y una clave de dispositivo únicos para que su dispositivo se identifique en IoT Hub. Los identificadores de dispositivos distinguen mayúsculas de minúsculas.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)

1. Seleccione **Todos los recursos** y busque su recurso de IoT Hub.

1. Cuando se abra el recurso de IoT Hub, haga clic en la herramienta **Dispositivos IoT** y, a continuación, haga clic en **Agregar** en la parte superior. 

    ![Creación de la identidad del dispositivo en el portal](./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png)

1. Proporcione un nombre para el dispositivo nuevo, como **myDeviceId**, y haga clic en **Guardar**. Esta acción crea una nueva identidad de dispositivo para su instancia de IoT Hub.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Agregar un nuevo dispositivo](./media/iot-hub-get-started-create-device-identity-portal/add-device.png)

1. En la lista de dispositivos, haga clic en el dispositivo recién creado y copie el valor de **Cadena de conexión: clave principal** para usarlo más adelante.

    ![Cadena de conexión de dispositivo](./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png)

> [!NOTE]
> El registro de identidades de IoT Hub solo almacena identidades de dispositivos para permitir el acceso seguro al centro de IoT. Almacena las claves y los identificadores de dispositivo para usarlos como credenciales de seguridad, y un indicador de habilitado o deshabilitado que permite deshabilitar el acceso a un dispositivo individual. Si la aplicación necesita almacenar otros metadatos específicos del dispositivo, debe usar un almacén específico de la aplicación. Consulte la [guía de desarrolladores de IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md) para obtener más información.
