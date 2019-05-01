---
title: Creación y administración de reglas de evento en una aplicación de Azure IoT Central | Microsoft Docs
description: Las reglas de eventos de Azure IoT Central le permiten supervisar los dispositivos casi en tiempo real e invocar automáticamente acciones, como el envío de correo electrónico, cuando la regla se desencadena.
author: ankitscribbles
ms.author: ankitgup
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: c98136e2f45965834fa1c538a5929eee14b24466
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60886249"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Creación de una regla de evento y configuración de las notificaciones en la aplicación de Azure IoT Central

*Este artículo se aplica a los administradores, operadores y compiladores.*

Puede usar Azure IoT Central para supervisar de forma remota los dispositivos conectados. Las reglas de Azure IoT Central le permiten supervisar los dispositivos casi en tiempo real e invocar acciones automáticamente, como el envío de correo electrónico o el desencadenamiento de Microsoft Flow. En unos pocos clics, puede definir la condición para la cual supervisar los datos del dispositivo y configurar la acción correspondiente. En este artículo se explica cómo crear reglas para supervisar los eventos que envía el dispositivo.

Los dispositivos pueden usar medidas de los eventos para enviar eventos de dispositivo importantes o informativos. Una regla de evento se desencadena cuando se notifica el evento de dispositivo seleccionado por el dispositivo.

## <a name="create-an-event-rule"></a>Creación de una regla de eventos

Para crear una regla de evento, la plantilla de dispositivos debe tener definida al menos una medida de evento. En este ejemplo se utiliza un máquina expendedora con refrigeración que notifica un evento de error del motor del ventilador. La regla supervisa el evento notificado por el dispositivo y envía un correo electrónico cada vez que se notifica el evento.

1. Mediante el **las plantillas de dispositivo** página, vaya a la plantilla de dispositivo para el que va a agregar la regla para.

1. Si aún no ha creado ninguna regla, consulte la siguiente pantalla:

    ![No hay ninguna regla todavía](media/howto-create-event-rules/Rules_Landing_Page.png)

1. En el **reglas** ficha, seleccione **+ nueva regla** para ver los tipos de reglas que puede crear.

1. Elija la **eventos** icono para crear una regla de supervisión de eventos.

    ![Tipos de regla](media/howto-create-event-rules/Rule_Types.png)

1. Escriba un nombre que le ayude a identificar la regla en esta plantilla de dispositivos.

1. Para habilitar inmediatamente la regla para todos los dispositivos que se crea a partir de esta plantilla, alternar **Habilitar regla para todos los dispositivos de esta plantilla**.

    ![Detalle de la regla](media/howto-create-event-rules/Rule_Detail.png)

    La regla se aplica automáticamente a todos los dispositivos de la plantilla de dispositivos.

### <a name="configure-the-rule-conditions"></a>Configuración de las condiciones de la regla

La condición define los criterios que la regla supervisa.

1. Elija **+** junto a **Condiciones** para agregar una condición nueva.

1. Elija el evento que desee supervisar de la lista desplegable Medida. En este ejemplo, se ha seleccionado el evento **Fan Motor Error** (Error del motor del ventilador).

   ![Condición](media/howto-create-event-rules/Condition_Filled_Out.png)

1. Si lo desea, también puede establecer **Recuento** como **Agregación** y proporcionar el umbral correspondiente.

   - Sin agregación, la regla se desencadena para cada punto de datos del evento que cumple la condición. Por ejemplo, si configura la regla de condición para desencadenar cuando un **Error de ventilador Motor** , a continuación, la regla desencadena casi inmediatamente cuando el dispositivo notifica ese evento, se produce el evento.
   - Si usa Recuento como función de agregación, debe proporcionar un **Umbral** y una **ventana de tiempo agregado** en la cual se evalúe la condición. En este caso, se agrega el recuento de eventos y los desencadenadores de la regla solo si el recuento de eventos agregados coincide con el umbral.

     Por ejemplo, si desea que se envíe una alerta cuando se produzcan más de tres eventos de dispositivo en cinco minutos, seleccione el evento y establezca la función de agregado como "count", el operador como "greater than" y "threshold" como 3. Establezca "Aggregation time period" (Tiempo de agregación) como "5 minutos". La regla se desencadena cuando el dispositivo envía más de tres eventos en 5 minutos. La frecuencia de evaluación de la regla es la misma que la **ventana de tiempo agregado**, lo que significa que, en este ejemplo, la regla se evalúa una vez cada 5 minutos.

     ![Adición de condición de evento](media/howto-create-event-rules/Aggregate_Condition_Filled_Out.png)

     >[!NOTE]
     >En **Condición** se puede agregar más de una medida del evento. Cuando se especifican varias condiciones, deben cumplirse todas ellas para que la regla se desencadene. Cada condición obtiene unido por una cláusula 'AND' implícitamente. Al usar el agregado, se debe agregar cada medida.

### <a name="configure-actions"></a>Configuración de acciones

En esta sección se muestra cómo configurar las acciones que se realizarán cuando se active la regla. Se invocan las acciones cuando todas las condiciones especificadas en la regla se evalúan como verdaderas.

1. Seleccione **+** junto a **Acciones**. Aquí puede ver la lista de acciones disponibles.

    ![Adición de acción](media/howto-create-event-rules/Add_Action.png)

1. Elija la acción **Correo electrónico**, escriba una dirección de correo electrónico válida en el campo **Para** y proporcione una nota que aparecerá en el cuerpo del correo electrónico cuando la regla se desencadene.

    > [!NOTE]
    > Solo se envían mensajes de correo electrónico a los usuarios que se han agregado a la aplicación y han iniciado sesión al menos una vez. Obtenga más información sobre la [administración de usuarios](howto-administer.md) en Azure IoT Central.

   ![Configuración de acción](media/howto-create-event-rules/Configure_Action.png)

1. Para guardar la regla, elija **Guardar**. La regla está activa en unos minutos e inicia la supervisión de los eventos que se envían a la aplicación. Si coincide con la condición especificada en la regla, la regla desencadena la acción de correo electrónico configurada.

Puede agregar otras acciones a la regla, como Microsoft Flow y webhooks. Puede agregar hasta 5 acciones por regla.

- [Acción de Microsoft Flow](howto-add-microsoft-flow.md) para iniciar un flujo de trabajo en Microsoft Flow cuando se desencadena una regla 
- [Acción de Webhook](howto-create-webhooks.md) para notificar a otros servicios cuando se desencadena una regla

## <a name="parameterize-the-rule"></a>Parametrización de la regla

Las acciones también pueden configurarse mediante la **Propiedad de dispositivo** como un parámetro. Si una dirección de correo electrónico se almacena como una propiedad de dispositivo, se puede utilizar cuando se define la dirección **Para**.

## <a name="delete-a-rule"></a>Eliminar una regla

Si ya no necesita una regla, elimínela; para ello, abra la regla y seleccione **Eliminar**. Al eliminar la regla, se quita de la plantilla de dispositivos y todos los dispositivos asociados.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Habilitación o deshabilitación de una regla para una plantilla de dispositivo

Navegue hasta el dispositivo y elija la regla que desea habilitar o deshabilitar. Alterne el botón **Enable rule for all devices of this template** (Habilitar regla para todos los dispositivos de esta plantilla) para habilitar o deshabilitar la regla para todos los dispositivos asociados a la plantilla de dispositivos.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Habilitación o deshabilitación de una regla para un dispositivo

Navegue hasta el dispositivo y elija la regla que desea habilitar o deshabilitar. Active o desactive el botón **Enable rule for this device** (Habilitar regla para este dispositivo) para habilitar o deshabilitar la regla para ese dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear reglas en una aplicación de Azure IoT Central, aquí tiene algunos de los pasos siguientes:

- [Incorporación de una acción de Microsoft Flow a las reglas](howto-add-microsoft-flow.md)
- [Incorporación de una acción de Webhook a las reglas](howto-create-webhooks.md)
- [Agrupar varias acciones para ejecutar desde una o varias reglas](howto-use-action-groups.md)
- [Administración de los dispositivos](howto-manage-devices.md)
