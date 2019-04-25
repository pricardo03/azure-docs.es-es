---
title: Administración de dispositivos Azure IoT con Cloud Explorer para Visual Studio | Microsoft Docs
description: Use la herramienta Cloud Explorer para Visual Studio para la administración de dispositivos de Azure IoT Hub, que incluye métodos directos y opciones de administración de las propiedades deseadas de los dispositivos gemelos.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: xshi
ms.openlocfilehash: 87a0847f5d42e014f3b2691c96446892176b481b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60399579"
---
# <a name="use-cloud-explorer-for-visual-studio-for-azure-iot-hub-device-management"></a>Uso de Cloud Explorer para Visual Studio para la administración de dispositivos de Azure IoT Hub

![Diagrama integral](media/iot-hub-device-management-visual-studio/iot-e2e-simple.png)

[Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) es una extensión útil de Visual Studio que le permite ver los recursos de Azure, inspeccionar sus propiedades y realizar acciones de desarrollador clave desde dentro de Visual Studio. Incluye opciones de administración que puede usar para realizar varias tareas.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Opción de administración          | Tarea                    |
|----------------------------|--------------------------------|
| Métodos directos             | Hacer que un dispositivo actúe, por ejemplo, para iniciar o detener el envío de mensajes o reiniciar el dispositivo.                                        |
| Leer dispositivo gemelo           | Obtener el estado notificado de un dispositivo. Por ejemplo, el dispositivo informa de que el LED está parpadeando.                                    |
| Actualizar dispositivo gemelo         | Poner un dispositivo en determinados estados, como establecer un indicador LED en verde o establecer el intervalo de envío de telemetría en 30 minutos.         |
| Mensajes de nube a dispositivo   | Enviar notificaciones a un dispositivo. Por ejemplo, "Es muy probable que llueva hoy. No olvide traerse un paraguas".              |

Para obtener una explicación más detallada acerca de las diferencias y orientación sobre el uso de estas opciones, consulte la [Guía de comunicación de dispositivo a nube](iot-hub-devguide-d2c-guidance.md) y la [Guía de comunicación de nube a dispositivo](iot-hub-devguide-c2d-guidance.md).

Los dispositivos gemelos son documentos JSON que almacenan información sobre el estado de los dispositivos (metadatos, configuraciones y condiciones). IoT Hub conserva un dispositivo gemelo por cada dispositivo que se conecta a él. Para más información acerca de los dispositivos gemelos, consulte [Introducción a los dispositivos gemelos](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Conocimientos que adquirirá

Aprenda a usar Cloud Explorer para Visual Studio con distintas opciones de administración en el equipo de desarrollo.

## <a name="what-you-do"></a>Qué debe hacer

Ejecute Cloud Explorer para Visual Studio con distintas opciones de administración.

## <a name="what-you-need"></a>Lo que necesita

- Una suscripción de Azure activa.
- Un centro de IoT de Azure en su suscripción.
- Microsoft Visual Studio 2017 Update 8 o posterior
- El componente Cloud Explorer desde el Instalador de Visual Studio (se selecciona de forma predeterminada con la carga de trabajo de Azure)

## <a name="update-cloud-explorer-to-latest-version"></a>Actualización de Cloud Explorer a la versión más reciente

El componente Cloud Explorer desde el Instalador de Visual Studio solo admite la supervisión de mensajes del dispositivo a la nube y de la nube al dispositivo. Deberá descargar e instalar la versión más reciente de [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) para tener acceso a las opciones de administración.

## <a name="sign-in-to-access-your-iot-hub"></a>Inicio de sesión para acceder a IoT Hub

1. En la ventana de Visual Studio **Cloud Explorer**, haga clic en el icono de administración de cuentas. Puede abrir la ventana de Cloud Explorer desde el menú **Ver** > **Cloud Explorer**.

    ![Hacer clic en Administración de cuentas](media/iot-hub-visual-studio-cloud-device-messaging/click-account-management.png)

1. Haga clic en **Manage Accounts** (Administrar cuentas) en Cloud Explorer.
1. Haga clic en **Add an account...** (Agregar una cuenta...) en la nueva ventana para iniciar sesión en Azure por primera vez.
1. Después de iniciar sesión, se muestra la lista de suscripciones de Azure. Seleccione las suscripciones de Azure que quiere ver y haga clic en **Apply** (Aplicar).
1. Expanda **su suscripción** > **IoT Hubs** > **su instancia de IoT Hub**; la lista de dispositivos se mostrará en su nodo de IoT Hub. Haga clic en un dispositivo para tener acceso a las opciones de administración.

    ![Opciones de administración](media/iot-hub-device-management-visual-studio/management-options.png)

## <a name="direct-methods"></a>Métodos directos

1. Haga clic con el botón derecho en el dispositivo y seleccione **Invoke Device Direct Method** (Invocar método directo de dispositivo).
1. Escriba el nombre del método y la carga en el cuadro de entrada.
1. Los resultados se mostrarán en el panel de salida de **IoT Hub**.

## <a name="read-device-twin"></a>Leer dispositivo gemelo

1. Haga clic con el botón derecho en el dispositivo y seleccione **Editar dispositivo gemelo**.
1. Se abrirá un archivo **azure-iot-device-twin.json** con el contenido del dispositivo gemelo.

## <a name="update-device-twin"></a>Actualizar dispositivo gemelo

1. Realice algunas modificaciones en el campo **tags** o **properties.desired** del archivo **azure-iot-dispositivo-twin.json**.
1. Presione **CTRL+S** para actualizar el dispositivo gemelo.
1. Los resultados se mostrarán en el panel de salida de **IoT Hub**.

## <a name="send-cloud-to-device-messages"></a>Envío de mensajes de nube a dispositivo

Para enviar un mensaje desde IoT Hub al dispositivo, siga estos pasos:

1. Haga clic con el botón derecho en el dispositivo y seleccione **Send C2D Message** (Enviar mensaje de C2D).
1. Escriba el mensaje en el cuadro de entrada.
1. Los resultados se mostrarán en el panel de salida de **IoT Hub**.

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido a utilizar Cloud Explorer para Visual Studio con distintas opciones de administración.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]