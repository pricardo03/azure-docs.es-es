---
title: Supervisión remota y notificaciones de IoT con Azure Logic Apps | Microsoft Docs
description: Use Azure Logic Apps para la supervisión de temperatura de IoT en IoT Hub y el envío automático de notificaciones de correo electrónico al buzón de correo cada vez que se detecten anomalías.
author: robinsh
keywords: supervisión de iot, notificaciones de iot, supervisión de temperatura de iot
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: robinsh
ms.openlocfilehash: 5a277ac18bcbcb7e7acc6faf52f7bc72759c82a7
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678010"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>Supervisión remota y notificaciones de IoT con Azure Logic Apps conectando IoT Hub y el buzón de correo

![Diagrama integral](media/iot-hub-monitoring-notifications-with-azure-logic-apps/iot-hub-e2e-logic-apps.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Azure Logic Apps proporciona una manera de automatizar los procesos como una serie de pasos. Una aplicación lógica puede conectar diversos servicios y protocolos. Comienza con un desencadenador como "Cuando se agregue una cuenta" y continúa con una combinación de acciones, una como "enviando una notificación de inserción". Esta característica convierte a Logic Apps en una solución de IoT perfecta para la supervisión de IoT, por ejemplo, para mantenerse alerta en busca de anomalías, entre otros escenarios de uso.

## <a name="what-you-learn"></a>Conocimientos que adquirirá

Aprenda a crear una aplicación lógica que conecte IoT Hub y el buzón de correo para la supervisión de temperatura y las notificaciones. Cuando la temperatura es superior a 30 °C, la aplicación cliente marca `temperatureAlert = "true"` en el mensaje que envía a IoT Hub. El mensaje desencadena la aplicación lógica para enviar una notificación de correo electrónico.

## <a name="what-you-do"></a>Qué debe hacer

* Crear un espacio de nombres de Service Bus y agregarle una cola.
* Agregar un punto de conexión y una regla de enrutamiento a IoT Hub.
* Crear, configurar y probar una aplicación lógica.

## <a name="what-you-need"></a>Lo que necesita

* Completar la [simulador en línea de Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) tutorial o uno de los tutoriales de dispositivo; por ejemplo, [Raspberry Pi con node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Estos cubren los siguientes requisitos:

  * Una suscripción de Azure activa.
  * Un centro de Azure IoT en su suscripción.
  * Una aplicación cliente que envía mensajes a su centro de Azure IoT.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Crear un espacio de nombres de Service Bus y agregarle una cola

### <a name="create-a-service-bus-namespace"></a>Crear un espacio de nombres de Service Bus

1. En el [portal Azure](https://portal.azure.com/), seleccione **crear un recurso** > **Enterprise Integration Pack** > **Service Bus**.

2. Proporcione la siguiente información:

   **Nombre**: El nombre de service bus.

   **Plan de tarifa**: Seleccione **básica** > **seleccione**. El nivel Básico es suficiente para este tutorial.

   **Grupo de recursos**: use el mismo grupo de recursos que usa el centro de IoT.

   **Ubicación**: Use la misma ubicación que utiliza el IoT hub.

3. Seleccione **Crear**.

   ![Crear un espacio de nombres de Service Bus en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Agregar una cola de Service Bus

1. Abra el espacio de nombres del bus de servicio y, a continuación, seleccione **+ cola**.

1. Escriba un nombre para la cola y, a continuación, seleccione **crear**.

1. Abra la cola de service bus y, a continuación, seleccione **directivas de acceso compartido** > **+ agregar**.

1. Escriba un nombre para la directiva, active **administrar**y, a continuación, seleccione **crear**.

   ![Agregar una cola de Service Bus en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-query-to-your-iot-hub"></a>Agregar un punto de conexión y una consulta de enrutamiento a IoT hub

Ahora agregue un punto de conexión y una consulta de enrutamiento a Iot hub.

### <a name="add-an-endpoint"></a>Agregación de un extremo

1. Abra el IoT hub, seleccione **extremos** > **+ agregar**.

1. Escriba la siguiente información:

   **Nombre**: Nombre del punto de conexión.

   **Tipo de extremo**: Seleccione la **cola de Service Bus**.

   **Espacio de nombres de Service Bus**: Seleccione el espacio de nombres que creó.

   **Cola de Service Bus**: Seleccione la cola que creó.

3. Seleccione **Aceptar**.

   ![Agregar un punto de conexión a IoT Hub en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Agregar una regla de enrutamiento

1. En la instancia de IoT hub, seleccione **rutas** > **+ agregar**.

2. Escriba la siguiente información:

   **Nombre**: El nombre de la regla de enrutamiento.

   **Origen de datos**: Seleccione **DeviceMessages**.

   **Punto de conexión**: Seleccione el punto de conexión que creó.

   **Cadena de consulta**: Escriba `temperatureAlert = "true"`.

3. Seleccione **Guardar**.

   ![Agregar una regla de enrutamiento en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Crear y configurar una aplicación lógica

A continuación, crear y configurar una aplicación lógica.

### <a name="create-a-logic-app"></a>Creación de una aplicación lógica

1. En el [portal Azure](https://portal.azure.com/), seleccione **crear un recurso** > **Enterprise Integration Pack** > **aplicación lógica**.

2. Escriba la siguiente información:

   **Nombre**: El nombre de la aplicación lógica.

   **Grupo de recursos**: use el mismo grupo de recursos que usa el centro de IoT.

   **Ubicación**: Use la misma ubicación que utiliza el IoT hub.

3. Seleccione **Crear**.

### <a name="configure-the-logic-app"></a>Configurar la aplicación lógica

1. Abra la aplicación lógica que se abre en el Diseñador de aplicaciones lógicas.

2. En el Diseñador de Logic Apps, seleccione **aplicación lógica en blanco**.

   ![Comenzar con una aplicación lógica en blanco en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

3. Seleccione **Service Bus**.

   ![Seleccionar Service Bus para empezar a crear la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

4. Seleccione **Service Bus: cuando uno o más mensajes llegan a una cola (Autocompletar)**.

5. Cree una conexión de Service Bus.

   1. Escriba un nombre de conexión.

   2. Seleccione el espacio de nombres de service bus > directiva de service bus > **crear**.

      ![Crear una conexión de Service Bus para la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   3. Seleccione **continuar** después de crea la conexión de service bus.

   4. Seleccione la cola que ha creado y escriba `175` para **recuento máximo de mensajes**.

      ![Especificar el recuento máximo de mensajes de la conexión de Service Bus en la aplicación lógica](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)

   5. Seleccione el "botón Guardar" para guardar los cambios.

6. Cree una conexión de servicio SMTP.

   1. Seleccione **Nuevo paso** > **Agregar una acción**.

   2. Tipo `SMTP`, seleccione el **SMTP** de servicio en el resultado de búsqueda y, a continuación, seleccione **SMTP - enviar correo electrónico**.

      ![Crear una conexión SMTP en la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   3. Escriba la información de SMTP del buzón de correo y, a continuación, seleccione **crear**.

      ![Escribir la información de la conexión SMTP en la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Obtenga la información SMTP de [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en) y [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).

   4. Escriba la dirección de correo electrónico en **De** y **Para** y `High temperature detected` en **Asunto** y **Cuerpo**.

   5. Seleccione **Guardar**.

La aplicación lógica está funcionando al guardarse.

## <a name="test-the-logic-app"></a>Probar la aplicación lógica

1. Inicie la aplicación cliente que se implementa en el dispositivo en [Connect ESP8266 to Azure IoT Hub (Conectar ESP8266 a Azure IoT Hub)](iot-hub-arduino-huzzah-esp8266-get-started.md).

2. Aumente la temperatura ambiental en torno al SensorTag para que sea superior a 30 °C. Por ejemplo, encienda una vela al lado del SensorTag.

3. Debería recibir una notificación de correo electrónico enviada por la aplicación lógica.

   > [!NOTE]
   > Es posible que el proveedor de servicios de correo electrónico necesite comprobar la identidad del remitente para asegurarse de que es usted quien envía el mensaje.

## <a name="next-steps"></a>Pasos siguientes

Ha creado correctamente una aplicación lógica que conecta IoT Hub y el buzón de correo para la supervisión de temperatura y las notificaciones.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
