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
ms.openlocfilehash: 63acf0297a694ff442d56e67d52fd9b4e49f812d
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008593"
---
El primer paso es usar Azure Portal para crear una instancia de IoT Hub en su suscripción. IoT Hub le permite ingerir grandes volúmenes de datos de telemetría en la nube desde muchos dispositivos. Posteriormente, el concentrador permite que uno o varios servicios de back-end se ejecuten en la nube para leer y procesar esos datos de telemetría.

1. Inicie sesión en el [Azure Portal](http://portal.azure.com).

1. Seleccione **Crear un recurso** > **Internet de las cosas** > **IoT Hub**.

    ![Seleccionar para instalar IoT Hub][1]

1. En el panel **Centro de IoT**, escriba la información necesaria para su centro de IoT:

   * **Suscripción**: elija la suscripción que quiere usar para crear este IoT Hub.
   * **Grupo de recursos**: cree un grupo de recursos para hospedar el centro de IoT o use uno existente. Al colocar juntos todos los recursos relacionados en un grupo, como **TestResources**, puede administrarlos juntos también. Por ejemplo, si elimina el grupo de recursos también se eliminarán todos los recursos que contiene. Para obtener más información, consulte [Uso de grupos de recursos para administrar recursos de Azure][lnk-resource-groups].
   * **Región**: seleccione la ubicación más cercana a sus dispositivos.
   * **Nombre**: cree un nombre único para su centro de IoT. Si el nombre que escribe está disponible, aparece una marca de verificación verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Ventana Datos básicos de IoT Hub][2]

2. Seleccione **Next: Size and scale** (Siguiente: tamaño y escala) para seguir creando su IoT Hub. 

3. Seleccione su **Nivel de precios y de escala**. En este artículo, seleccione el nivel **F1 - Free** (F1: gratis) si aún está disponible en la suscripción. Para más información, consulte [Nivel de precios y de escala][lnk-pricing].

   ![Ventana Tamaño y escala de IoT Hub][3]

4. Seleccione **Revisar + crear**.

1. Revise la información de IoT Hub y, a continuación, haga clic en **Crear**. El centro de IoT puede tardar varios minutos en crearse. Puede ver el progreso en el panel **Notificaciones**.


<!-- Images -->
[1]: ./media/iot-hub-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-create-hub/create-iot-hub3.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md