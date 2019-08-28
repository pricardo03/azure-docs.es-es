---
title: Creación y administración de reglas de evento en una aplicación de Azure IoT Central | Microsoft Docs
description: Las reglas de eventos de Azure IoT Central le permiten supervisar los dispositivos casi en tiempo real e invocar automáticamente acciones, como el envío de correo electrónico, cuando la regla se desencadena.
author: dominicbetts
ms.author: dobett
ms.date: 07/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1a7fc2b38e8354fb29255bb7f9d6b2c03ed53725
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879048"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application-preview-features"></a>Creación de una regla de eventos y configuración de las notificaciones en la aplicación de Azure IoT Central (características de versión preliminar)

*Este artículo se aplica a los administradores, operadores y compiladores.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Puede usar Azure IoT Central para supervisar de forma remota los dispositivos conectados. Las reglas de Azure IoT Central le permiten supervisar los dispositivos casi en tiempo real e invocar acciones automáticamente, como el envío de correo electrónico. En unos pocos clics, puede definir la condición para la cual supervisar los datos del dispositivo y configurar la acción correspondiente. En este artículo se explica cómo crear reglas para supervisar los eventos que envía el dispositivo.

Los dispositivos pueden usar medidas de los eventos para enviar eventos de dispositivo importantes o informativos. Una regla de evento se desencadena cuando se notifica el evento de dispositivo seleccionado por el dispositivo.

## <a name="create-an-event-rule"></a>Creación de una regla de eventos

Para crear una regla de evento, la plantilla de dispositivos debe tener definida al menos una medida de evento. En este ejemplo se utiliza un máquina expendedora con refrigeración que notifica un evento de error del motor del ventilador. La regla supervisa el evento notificado por el dispositivo y envía un correo electrónico cada vez que se notifica el evento.

1. Vaya a la página **Reglas**.

1. Si aún no ha creado ninguna regla, consulte la siguiente pantalla:

   ![No hay ninguna regla todavía](media/howto-create-event-rules-pnp/rules-landing-page1.png)

1. Seleccione la pestaña **+ Nueva regla** para ver los tipos de reglas que puede crear.

1. Elija **Evento** para crear una regla de supervisión de eventos.

   ![Tipos de regla](media/howto-create-event-rules-pnp/rule-types1.png)

1. Escriba un nombre que le ayude a identificar la regla y pulse ENTRAR.

1. Seleccione la definición del dispositivo del que quiere definir el ámbito de esta regla en la sección **Ámbitos**. Esta pantalla también permite filtrar los dispositivos a los que se aplica la regla mediante el uso de **+ Filtro**. La regla se aplica automáticamente a todos los dispositivos de la plantilla de dispositivos. Para deshabilitar la regla, seleccione el botón **Deshabilitar** en el encabezado.

### <a name="configure-the-rule-conditions"></a>Configuración de las condiciones de la regla

La condición define los criterios que la regla supervisa.

1. Seleccione si quiere la opción **Establecer agregación** activada o desactivada.

   - Sin agregación, la regla se desencadena para cada punto de datos del evento que cumple la condición. Por ejemplo, si configura la condición de regla para que se desencadene cuando se produzca un **evento de error del motor del ventilador**, la regla se desencadenará casi inmediatamente después de que el dispositivo notifique el evento.
   - Si usa **Recuento** como función de agregado, debe proporcionar un **valor** y una **ventana de tiempo** en la cual se evalúe la condición. En este caso, el recuento de eventos es agregado y la regla se desencadenará solo si el recuento de eventos agregados coincide con el valor.

1. Elija el evento que quiere supervisar de la lista desplegable **Medida**. En este ejemplo, se ha seleccionado el evento **Fan Motor Error** (Error del motor del ventilador).

1. Si quiere, también puede establecer **Recuento** como **Agregación** y proporcionar el valor según el que se desencadenará la regla.

     Por ejemplo, si quiere que se envíe una alerta cuando se produzcan más de tres eventos de dispositivo en cinco minutos, seleccione el evento y establezca la función de agregado como **Recuento**, el operador como **mayor que** y **Valor** como 3. Establezca la **ventana de tiempo** en **cinco minutos**. La regla se desencadena cuando el dispositivo envía más de tres eventos en 5 minutos. La frecuencia de evaluación de la regla es la misma que la **ventana de tiempo**, lo que significa que, en este ejemplo, la regla se evalúa una vez cada 5 minutos.

 ![Condición](media/howto-create-event-rules-pnp/aggregate-condition-filled-out1.png)

> [!NOTE]
> En **Condición** se puede agregar más de una medida del evento. Cuando se especifican varias condiciones, deben cumplirse todas ellas para que la regla se desencadene. Cada condición se une mediante una cláusula "AND" de manera implícita. Al usar el agregado, se debe agregar cada medida.

### <a name="configure-actions"></a>Configuración de acciones

En esta sección se muestra cómo configurar las acciones que se realizarán cuando se active la regla. Se invocan las acciones cuando todas las condiciones especificadas en la regla se evalúan como verdaderas.

1. Haga clic en **+ Acción** en las secciones de **Acción**. Aquí puede ver la lista de acciones disponibles.  

   ![Adición de acción](media/howto-create-event-rules-pnp/add-action1.png)

1. Elija la acción **Correo electrónico**, escriba un nombre para mostrar para la acción, una dirección de correo electrónico válida en el campo **Para** y proporcione una nota que aparecerá en el cuerpo del correo electrónico cuando la regla se desencadene.

   > [!NOTE]
   > Solo se envían mensajes de correo electrónico a los usuarios que se han agregado a la aplicación y han iniciado sesión al menos una vez. Obtenga más información sobre la [administración de usuarios](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) en Azure IoT Central.

   ![Configuración de acción](media/howto-create-event-rules-pnp/configure-action1.png)

1. Para guardar la acción, elija **Listo**.

1. Para guardar la regla, elija **Guardar**. La regla está activa en unos minutos e inicia la supervisión de los eventos que se envían a la aplicación. Cuando se cumple la condición especificada en la regla, la regla desencadena la acción de correo electrónico configurada.

## <a name="parameterize-the-rule"></a>Parametrización de la regla

Las reglas pueden derivar ciertos valores de las **propiedades del dispositivo**  como parámetros. El uso de parámetros es útil en aquellos escenarios en los que los umbrales de eventos varían para diferentes dispositivos. Cuando cree la regla, elija una propiedad de dispositivo que especifique el umbral, como **umbral ideal máximo**, en lugar de proporcionar un valor absoluto, como tres eventos. Cuando la regla se ejecuta, hace coincidir los datos de eventos del dispositivo con el valor establecido en la propiedad del dispositivo.

El uso de parámetros es una forma eficaz de reducir el número de reglas que va a administrar.

Las acciones también pueden configurarse mediante la **Propiedad de dispositivo** como un parámetro. Si una dirección de correo electrónico se almacena como una propiedad de dispositivo, se puede utilizar cuando se define la dirección **Para**.

## <a name="delete-a-rule"></a>Eliminar una regla

Si ya no necesita una regla, elimínela; para ello, abra la regla y seleccione **Eliminar**. Al eliminar la regla, se quita de la plantilla de dispositivos y todos los dispositivos asociados.

## <a name="enable-or-disable-a-rule"></a>Habilitación o deshabilitación de una regla

Elija la regla que quiere habilitar o deshabilitar. Alterne el botón **Habilitar** o **Deshabilitar** en la regla para habilitar o deshabilitar la regla para todos los dispositivos que tengan el ámbito de esta.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Habilitación o deshabilitación de una regla para un dispositivo

Elija la regla que quiere habilitar o deshabilitar. Agregue un filtro en la sección **Ámbitos** para incluir o excluir un determinado dispositivo en la plantilla de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear reglas en una aplicación de Azure IoT Central, el paso siguiente que sugerimos es que obtenga más información sobre la [administración de los dispositivos](howto-manage-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
