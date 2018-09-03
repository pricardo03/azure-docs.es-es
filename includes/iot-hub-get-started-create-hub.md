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
ms.openlocfilehash: 08afdcf91499fdb6f2da6e9ccc82313286f5c964
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43111955"
---
## <a name="create-an-iot-hub"></a>Crear un centro de IoT
Cree un Centro de IoT al que se pueda conectar la aplicación de dispositivo simulado. En los siguientes pasos se muestra cómo completar esta tarea mediante el Portal de Azure.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. Seleccione **Crear un recurso** > **Internet de las cosas** > **IoT Hub**.
   
    ![Barra de accesos del Portal de Azure](./media/iot-hub-get-started-create-hub/create-iot-hub1.png)

3. En el panel **Centro de IoT**, escriba la información necesaria para su centro de IoT:

   * **Suscripción**: elija la suscripción que quiere usar para crear este IoT Hub.

   * **Grupo de recursos**: cree un grupo de recursos para hospedar el centro de IoT o use uno existente. Para más información, consulte [Administración de los recursos de Azure a través del Portal](../articles/azure-resource-manager/resource-group-portal.md).

   * **Región**: seleccione la ubicación más cercana a usted.

   * **Nombre**: cree un nombre para su centro de IoT. Si el nombre que escribe está disponible, aparece una marca de verificación verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Ventana Datos básicos de IoT Hub](./media/iot-hub-get-started-create-hub/create-iot-hub2.png)

4. Seleccione **Next: Size and scale** (Siguiente: tamaño y escala) para seguir creando su IoT Hub. 

5. Seleccione su **Nivel de precios y de escala**. En este artículo, seleccione el nivel **F1 - Free** (F1: gratis) si aún está disponible en la suscripción. Para más información, consulte la página de [precios de IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![Ventana Tamaño y escala de IoT Hub](./media/iot-hub-get-started-create-hub/create-iot-hub3.png)

6. Seleccione **Revisar + crear**.

7. Revise la información de IoT Hub y, a continuación, haga clic en **Crear**. El centro de IoT puede tardar varios minutos en crearse. Puede ver el progreso en el panel **Notificaciones**.

8. Cuando la nueva instancia de IoT Hub esté lista, haga clic en su icono en Azure Portal para abrir su ventana de propiedades. Ahora que ha creado un centro de IoT, busque la información importante que usa para conectar a él dispositivos y aplicaciones. Haga clic en **Directivas de acceso compartido**.
   
9. En **Directivas de acceso compartido**, seleccione la directiva **iothubowner**. Copie el valor del campo **Connection string---primary key** (Cadena de conexión: clave principal) del centro de IoT para usarlo más adelante. Para más información, consulte [Control de acceso](../articles/iot-hub/iot-hub-devguide-security.md) en la "Guía del desarrollador de Azure IoT Hub".
   
    ![Directivas de acceso compartido](./media/iot-hub-get-started-create-hub/create-iot-hub5.png)