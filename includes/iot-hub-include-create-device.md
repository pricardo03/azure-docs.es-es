---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: e93f78cf07cd4815e5b17ffd3953db121adb6535
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69558796"
---
<!-- put the ## header in the file that includes this file -->

En esta sección, creará una identidad de dispositivo en el registro de identidades del centro de IoT. No se puede conectar un dispositivo a un centro a menos que tenga una entrada en el registro de identidades. Para más información, consulte la sección "Registro de identidades" de la [guía para desarrolladores de IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

1. En el menú de navegación del centro de IoT, abra **Dispositivos IoT** y, después, seleccione **Nuevo** para agregar un dispositivo nuevo en el centro de IoT.

    ![Creación de la identidad del dispositivo en el portal](./media/iot-hub-include-create-device/create-identity-portal-vs2019.png)

1. En **Crear un dispositivo**, proporcione un nombre para el dispositivo nuevo, como **IdDeMiDispositivo** y seleccione **Guardar**. Esta acción crea una nueva identidad de dispositivo para su centro de IoT.

   ![Agregar un nuevo dispositivo](./media/iot-hub-include-create-device/create-a-device-vs2019.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. Después de crear el dispositivo, ábralo desde la lista del panel **Dispositivos de IoT**. Copie la **cadena de conexión principal** para usarla después.

    ![Cadena de conexión de dispositivo](./media/iot-hub-include-create-device/device-details-vs2019.png)

> [!NOTE]
> El registro de identidades de IoT Hub solo almacena identidades de dispositivos para permitir el acceso seguro al centro de IoT. Almacena las claves y los identificadores de dispositivo para usarlos como credenciales de seguridad, y un indicador de habilitado o deshabilitado que permite deshabilitar el acceso a un dispositivo individual. Si la aplicación necesita almacenar otros metadatos específicos del dispositivo, debe usar un almacén específico de la aplicación. Consulte la [guía de desarrolladores de IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md) para obtener más información.
