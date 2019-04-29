---
title: Supervisión remota y notificaciones de IoT con Azure Logic Apps | Microsoft Docs
description: Use Azure Logic Apps para la supervisión de temperatura de IoT en IoT Hub y el envío automático de notificaciones de correo electrónico al buzón de correo cada vez que se detecten anomalías.
author: robinsh
keywords: supervisión de iot, notificaciones de iot, supervisión de temperatura de iot
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/19/2019
ms.author: robinsh
ms.openlocfilehash: 26637468f44e12f7ad66f907e0f6be3d907e578f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126279"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>Supervisión remota y notificaciones de IoT con Azure Logic Apps conectando IoT Hub y el buzón de correo

![Diagrama integral](media/iot-hub-monitoring-notifications-with-azure-logic-apps/iot-hub-e2e-logic-apps.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) puede ayudar a organizar flujos de trabajo locales y servicios de nube, las empresas de uno o más y a través de diversos protocolos. Una aplicación lógica comienza con un desencadenador, que, a continuación, seguido de una o varias acciones que se pueden secuenciar mediante los controles integrados, como las condiciones y los iteradores. Esta flexibilidad hace que las aplicaciones lógicas una solución de IoT ideal para escenarios de supervisión de IoT. Por ejemplo, la llegada de los datos de telemetría desde un dispositivo en un punto de conexión de IoT Hub puede iniciar aplicaciones lógicas de almacenamiento de los datos en un blob de Azure Storage, enviar alertas por correo electrónico para avisar de anomalías de datos, programar una visita del técnico, si un dispositivo notifica un error , y así sucesivamente.

## <a name="what-you-learn"></a>Conocimientos que adquirirá

Aprenda a crear una aplicación lógica que conecte IoT Hub y el buzón de correo para la supervisión de temperatura y las notificaciones.

El código de cliente que se ejecutan en el dispositivo establece una propiedad de la aplicación, `temperatureAlert`, en cada telemetría de mensaje se envía a IoT hub. Cuando el código de cliente detecta una temperatura superior a 30 C, establece esta propiedad en `true`; en caso contrario, Establece la propiedad en `false`.

En este tema, configurar el enrutamiento en IoT hub para enviar mensajes en el que `temperatureAlert = true` a un Bus de servicio de punto de conexión y configurar una aplicación lógica que se desencadena en los mensajes que llegan en el extremo de Service Bus y envía una notificación por correo electrónico.

## <a name="what-you-do"></a>Qué debe hacer

* Crear un espacio de nombres de Service Bus y agregarle una cola de Service Bus.
* Agregue un punto de conexión personalizado y una regla de enrutamiento a IoT hub para enrutar los mensajes que contienen una alerta de temperatura a la cola de Service Bus.
* Crear, configurar y probar una aplicación lógica para consumir mensajes de la cola de Service Bus y enviar correos electrónicos de notificación a un destinatario deseado.

## <a name="what-you-need"></a>Lo que necesita

* Completar la [simulador en línea de Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) tutorial o uno de los tutoriales de dispositivo; por ejemplo, [Raspberry Pi con node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Estos cubren los siguientes requisitos:

  * Una suscripción de Azure activa.
  * Un centro de Azure IoT en su suscripción.
  * Una aplicación cliente que se ejecuta en el dispositivo que envía mensajes de telemetría a IoT hub de Azure.

## <a name="create-service-bus-namespace-and-queue"></a>Crear cola y el espacio de nombres de Service Bus

Cree un espacio de nombres de Service Bus y una cola. Más adelante en este tema, creará una regla de enrutamiento en IoT hub para dirigir los mensajes que contienen una alerta de temperatura a la cola de Service Bus, donde se recogerán mediante una aplicación lógica y activarlo para enviar un correo electrónico de notificación.

### <a name="create-a-service-bus-namespace"></a>Creación de un espacio de nombres de Service Bus

1. En el [portal Azure](https://portal.azure.com/), seleccione **+ crear un recurso** > **integración** > **Service Bus**.

1. En el **crear espacio de nombres** panel, proporcione la siguiente información:

   **Nombre**: El nombre del espacio de nombres del bus de servicio. El espacio de nombres debe ser único en Azure.

   **Plan de tarifa**: Seleccione **básica** en la lista desplegable. El nivel Básico es suficiente para este tutorial.

   **Grupo de recursos**: use el mismo grupo de recursos que usa el centro de IoT.

   **Ubicación**: Use la misma ubicación que utiliza el IoT hub.

   ![Crear un espacio de nombres de Service Bus en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1-create-service-bus-namespace-azure-portal.png)

1. Seleccione **Crear**. Espere a que la implementación se complete antes de continuar con el paso siguiente.

### <a name="add-a-service-bus-queue-to-the-namespace"></a>Agregar una cola de Service Bus para el espacio de nombres

1. Abra el espacio de nombres de Service Bus. La manera más fácil de obtener al espacio de nombres de Service Bus consiste en seleccionar **grupos de recursos** desde el panel recursos, seleccione el grupo de recursos y luego seleccione el espacio de nombres del Bus de servicio en la lista de recursos.

1. En el **Service Bus Namespace** panel, seleccione **+ cola**.

1. Escriba un nombre para la cola y, a continuación, seleccione **crear**. Cuando la cola se ha creado correctamente, el **Crear cola** panel se cierra.

   ![Agregar una cola de Service Bus en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-queue.png)

1. En el **Service Bus Namespace** panel, en **entidades**, seleccione **colas**. Abrir la cola de Service Bus en la lista y, a continuación, seleccione **directivas de acceso compartido** > **+ agregar**.

1. Escriba un nombre para la directiva, active **administrar**y, a continuación, seleccione **crear**.

   ![Agregar una directiva de cola de service bus en Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2-add-service-bus-queue-azure-portal.png)

## <a name="add-a-custom-endpoint-and-routing-rule-to-your-iot-hub"></a>Agregar un punto de conexión personalizado y una regla de enrutamiento a IoT hub

Agregue un punto de conexión personalizado para la cola de Service Bus a IoT hub y cree una regla de enrutamiento de mensajes para dirigir los mensajes que contienen una alerta de temperatura a ese punto de conexión, donde seleccionará la aplicación lógica. La regla de enrutamiento usa una consulta de enrutamiento, `temperatureAlert = "true"`, para reenviar los mensajes según el valor de la `temperatureAlert` establecido por el código de cliente que se ejecutan en el dispositivo propiedad de la aplicación. Para obtener más información, consulte [consulta enrutamiento según las propiedades de mensaje del mensaje](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-properties).

### <a name="add-a-custom-endpoint"></a>Agregar un punto de conexión personalizado

1. Abra el IoT Hub. La manera más fácil de obtener al centro de IoT consiste en seleccionar **grupos de recursos** desde el panel recursos, seleccione el grupo de recursos y luego seleccione el centro de IoT en la lista de recursos.

1. En **mensajería**, seleccione **enrutamiento de mensajes**. En el **enrutamiento de mensajes** panel, seleccione el **puntos de conexión personalizados** pestaña y, a continuación, seleccione **+ agregar**. En la lista desplegable, seleccione **cola de Service bus**.

   ![Agregar un punto de conexión a IoT Hub en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-iot-hub-custom-endpoint.png)

1. En el **agregar un extremo de service bus** panel, escriba la siguiente información:

   **Nombre del extremo**: Nombre del punto de conexión.

   **Espacio de nombres de Service Bus**: Seleccione el espacio de nombres que creó.

   **Cola de Service bus**: Seleccione la cola que creó.

   ![Agregar un punto de conexión a IoT Hub en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3-add-iot-hub-endpoint-azure-portal.png)

1. Seleccione **Crear**. Después de que el punto de conexión se crea correctamente, continúe con el paso siguiente.

### <a name="add-a-routing-rule"></a>Agregar una regla de enrutamiento

1. En el **enrutamiento de mensajes** panel, seleccione el **rutas** pestaña y, a continuación, seleccione **+ agregar**.

1. En el **agregar una ruta** panel, escriba la siguiente información:

   **Nombre**: El nombre de la regla de enrutamiento.

   **Punto de conexión**: Seleccione el punto de conexión que creó.

   **Origen de datos**: Seleccione **mensajes de telemetría de dispositivo**.

   **Consulta de enrutamiento**: Escriba `temperatureAlert = "true"`.

   ![Agregar una regla de enrutamiento en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4-add-routing-rule-azure-portal.png)

1. Seleccione **Guardar**. Puede cerrar la **enrutamiento de mensajes** panel.

## <a name="create-and-configure-a-logic-app"></a>Crear y configurar una aplicación lógica

En la sección anterior, configure su IoT hub para enrutar los mensajes que contiene una alerta de temperatura a la cola de Service Bus. Ahora, configure una aplicación lógica para supervisar la cola de Service Bus y enviar una notificación por correo electrónico cada vez que se agrega un mensaje a la cola.

### <a name="create-a-logic-app"></a>Creación de una aplicación lógica

1. Seleccione **crear un recurso** > **integración** > **aplicación lógica**.

1. Escriba la siguiente información:

   **Nombre**: El nombre de la aplicación lógica.

   **Grupo de recursos**: use el mismo grupo de recursos que usa el centro de IoT.

   **Ubicación**: Use la misma ubicación que utiliza el IoT hub.

   ![Crear una aplicación lógica en Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-a-logic-app.png)

1. Seleccione **Crear**.

### <a name="configure-the-logic-app-trigger"></a>Configurar el desencadenador de aplicación lógica

1. Abra la aplicación lógica. La manera más fácil para llegar a la aplicación lógica consiste en seleccionar **grupos de recursos** desde el panel recursos, seleccione el grupo de recursos y luego seleccione la aplicación lógica en la lista de recursos. Cuando se selecciona la aplicación lógica, se abre el Diseñador de Logic Apps.

1. En el Diseñador de aplicaciones lógicas, desplácese hacia abajo hasta **plantillas** y seleccione **aplicación lógica en blanco**.

   ![Comenzar con una aplicación lógica en blanco en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5-start-with-blank-logic-app-azure-portal.png)

1. Seleccione el **todas** pestaña y, a continuación, seleccione **Service Bus**.

   ![Seleccionar Service Bus para empezar a crear la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6-select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. En **desencadenadores**, seleccione **cuando llegan uno o más mensajes en una cola (Autocompletar)**.

   ![Seleccione el desencadenador para la aplicación lógica en Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-service-bus-trigger.png)

1. Cree una conexión de Service Bus.
   1. Escriba un nombre de conexión y seleccione el espacio de nombres del Bus de servicio en la lista. Se abre la pantalla siguiente.

      ![Crear una conexión de Service Bus para la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-connection-1.png)

   1. Seleccione la directiva de service bus (RootManageSharedAccessKey). A continuación, seleccione **crear**.

      ![Crear una conexión de Service Bus para la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7-create-service-bus-connection-in-logic-app-azure-portal.png)

   1. En la pantalla final, para **nombre de la cola**, seleccione la cola que creó a partir de la lista desplegable. Escriba `175` para **recuento máximo de mensajes**.

      ![Especificar el recuento máximo de mensajes de la conexión de Service Bus en la aplicación lógica](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8-specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)

   1. Seleccione **guardar** en el menú en la parte superior del Diseñador de aplicaciones lógicas para guardar los cambios.

### <a name="configure-the-logic-app-action"></a>Configurar la acción de aplicación lógica

1. Cree una conexión de servicio SMTP.

   1. Seleccione **Nuevo paso**. En **elegir una acción**, seleccione el **todas** ficha.

   1. Tipo `smtp` en el cuadro de búsqueda, seleccione el **SMTP** de servicio en el resultado de búsqueda y, a continuación, seleccione **enviar correo electrónico**.

      ![Crear una conexión SMTP en la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9-create-smtp-connection-logic-app-azure-portal.png)

   1. Escriba la información de SMTP para el buzón y, a continuación, seleccione **crear**.

      ![Escribir la información de la conexión SMTP en la aplicación lógica en Azure Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10-enter-smtp-connection-info-logic-app-azure-portal.png)

      Obtenga la información SMTP de [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en) y [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).

      > [!NOTE]
      > Es posible que deba deshabilitar SSL para establecer la conexión. Si este es el caso y desea volver a habilitar SSL una vez establecida la conexión, consulte el paso opcional al final de esta sección.

   1. Desde el **Agregar nuevo parámetro** desplegable en el **enviar correo electrónico** paso, seleccione **desde**, **a**, **asunto**y **cuerpo**. Haga clic o pulse en cualquier parte en la pantalla para cerrar el cuadro de selección.

      ![Elija los campos de correo electrónico de conexión de SMTP](media/iot-hub-monitoring-notifications-with-azure-logic-apps/smtp-connection-choose-fields.png)

   1. Escriba la dirección de correo electrónico en **De** y **Para** y `High temperature detected` en **Asunto** y **Cuerpo**. Si el **agregar contenido dinámico de las aplicaciones y conectores que se usan en este flujo** abre el cuadro de diálogo, seleccione **ocultar** para cerrarlo. No use contenido dinámico en este tutorial.

      ![Campos de correo electrónico de conexión de SMTP para rellenar](media/iot-hub-monitoring-notifications-with-azure-logic-apps/fill-in-smtp-connection-fields.png)

   1. Seleccione **guardar** para guardar la conexión de SMTP.

1. (Opcional) Si tuviera que deshabilitar SSL para establecer una conexión con el proveedor de correo electrónico y desea volver a habilitarla, siga estos pasos:

   1. En el **aplicación lógica** panel, en **herramientas de desarrollo**, seleccione **las conexiones de API**.

   1. En la lista de conexiones de API, seleccione la conexión de SMTP.

   1. En el **smtp conexión de API** panel, en **General**, seleccione **Editar conexión de API**.

   1. En el **Editar conexión de API** panel, seleccione **habilitar SSL?**, vuelva a escribir la contraseña de su cuenta de correo electrónico y seleccione **guardar**.

      ![Editar conexión de API de SMTP en la aplicación lógica en Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/re-enable-smtp-connection-ssl.png)

La aplicación lógica ahora está lista para procesar las alertas de temperatura de la cola de Service Bus y envían notificaciones a su cuenta de correo electrónico.

## <a name="test-the-logic-app"></a>Probar la aplicación lógica

1. Inicie la aplicación cliente en el dispositivo.

1. Si usa un dispositivo físico, poner cuidadosamente una fuente de calor cerca del sensor de calor hasta que la temperatura supere los 30 grados C. Si usas el simulador en línea, el código de cliente generará aleatoriamente los mensajes de telemetría que superar los 30 C.

1. Debería empezar a recibir notificaciones por correo electrónico enviadas por la aplicación lógica.

   > [!NOTE]
   > Es posible que el proveedor de servicios de correo electrónico necesite comprobar la identidad del remitente para asegurarse de que es usted quien envía el mensaje.

## <a name="next-steps"></a>Pasos siguientes

Ha creado correctamente una aplicación lógica que conecta IoT Hub y el buzón de correo para la supervisión de temperatura y las notificaciones.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
