---
title: Administración de mensajería de dispositivos de nube de Azure IoT Hub con Cloud Explorer para Visual Studio | Microsoft Docs
description: Aprenda a usar Cloud Explorer para Visual Studio para supervisar los mensajes del dispositivo a la nube y enviar mensajes de la nube al dispositivo en Azure IoT Hub.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/07/2018
ms.author: xshi
ms.openlocfilehash: ab3c02d7207bca70a90df8aa08c73c1484cd635d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61440752"
---
# <a name="use-cloud-explorer-for-visual-studio-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Uso de Cloud Explorer para Visual Studio para enviar y recibir mensajes entre el dispositivo e IoT Hub

![Diagrama integral](./media/iot-hub-visual-studio-cloud-device-messaging/e-to-e-diagram.png)

[Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) es una extensión útil de Visual Studio que le permite ver los recursos de Azure, inspeccionar sus propiedades y realizar acciones de desarrollador clave desde dentro de Visual Studio. Este artículo se centra en cómo usar Cloud Explorer para enviar y recibir mensajes entre el dispositivo y su instancia de IoT Hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Lo qué aprenderá

Aprenderá a usar Cloud Explorer para Visual Studio para supervisar los mensajes del dispositivo a la nube y para enviar mensajes de la nube al dispositivo. Los mensajes del dispositivo a la nube podrían ser datos de sensor que el dispositivo recopila y luego envía a IoT Hub. Los mensajes de la nube al dispositivo podrían ser comandos que IoT Hub envía al dispositivo. Por ejemplo, hacer parpadear un LED que está conectado al dispositivo.

## <a name="what-you-will-do"></a>Lo que hará

- Usará Cloud Explorer para Visual Studio para supervisar los mensajes del dispositivo a la nube.
- Usará Cloud Explorer para Visual Studio para enviar mensajes de la nube al dispositivo.

## <a name="what-you-need"></a>Lo que necesita

- Una suscripción de Azure activa.
- Una instancia de Azure IoT Hub en su suscripción.
- Microsoft Visual Studio 2017 Update 8 o posterior
- El componente Cloud Explorer desde el Instalador de Visual Studio (se selecciona de forma predeterminada con la carga de trabajo de Azure)

## <a name="update-cloud-explorer-to-latest-version"></a>Actualización de Cloud Explorer a la versión más reciente

El componente Cloud Explorer desde el Instalador de Visual Studio solo admite la supervisión de mensajes del dispositivo a la nube y de la nube al dispositivo. Para enviar mensajes al dispositivo o a la nube, descargue e instale la versión más reciente de [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS).

## <a name="sign-in-to-access-your-iot-hub"></a>Inicio de sesión para acceder a IoT Hub

1. En la ventana de Visual Studio **Cloud Explorer**, haga clic en el icono de administración de cuentas. Puede abrir la ventana de Cloud Explorer desde el menú **Ver** > **Cloud Explorer**.

    ![Hacer clic en Administración de cuentas](media/iot-hub-visual-studio-cloud-device-messaging/click-account-management.png)


2. Haga clic en **Manage Accounts** (Administrar cuentas) en Cloud Explorer.

3. Haga clic en **Add an account...** (Agregar una cuenta...) en la nueva ventana para iniciar sesión en Azure por primera vez.

4. Después de iniciar sesión, se muestra la lista de suscripciones de Azure. Seleccione las suscripciones de Azure que quiere ver y haga clic en **Apply** (Aplicar).

5. Expanda **su suscripción** > **IoT Hubs** > **su instancia de IoT Hub**; la lista de dispositivos se mostrará en su nodo de IoT Hub.

    ![Lista de dispositivos](media/iot-hub-visual-studio-cloud-device-messaging/device-list.png)

## <a name="monitor-device-to-cloud-messages"></a>Supervisión de mensajes de dispositivo a nube

Para supervisar los mensajes que se envían desde el dispositivo a IoT Hub, siga estos pasos:

1. Haga clic con el botón derecho en su instancia de IoT Hub o en su dispositivo y seleccione **Start Monitoring D2C Message** (Iniciar la supervisión del mensaje de D2C).

    ![Iniciar la supervisión del mensaje de D2C](media/iot-hub-visual-studio-cloud-device-messaging/start-monitoring-d2c-message.png)

2. Los valores supervisados se mostrarán en el panel de salida de **IoT Hub**.

    ![Resultado de supervisión del mensaje de D2C](media/iot-hub-visual-studio-cloud-device-messaging/monitor-d2c-message-result.png)

3. Para detener la supervisión, haga clic con el botón derecho en cualquier instancia de IoT Hub o en cualquier dispositivo y seleccione **Stop Monitoring D2C Message** (Detener la supervisión del mensaje de D2C).

## <a name="send-cloud-to-device-messages"></a>Envío de mensajes de nube a dispositivo

Para enviar un mensaje desde IoT Hub al dispositivo, siga estos pasos:

1. Haga clic con el botón derecho en el dispositivo y seleccione **Send C2D Message** (Enviar mensaje de C2D).

    ![Enviar mensaje de C2D](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message.png)

2. Escriba el mensaje en el cuadro de entrada.

3. Los resultados se mostrarán en el panel de salida de **IoT Hub**.

    ![Resultado de enviar mensaje de C2D](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-result.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora sabe cómo supervisar los mensajes del dispositivo a la nube y enviar mensajes de la nube al dispositivo entre el dispositivo de IoT y la instancia de Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]