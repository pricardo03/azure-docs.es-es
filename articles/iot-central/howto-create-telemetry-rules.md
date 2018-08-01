---
title: Creación y administración de reglas de telemetría en una aplicación de Azure IoT Central | Microsoft Docs
description: Las reglas de telemetría de Azure IoT Central le permiten supervisar los dispositivos casi en tiempo real e invocar automáticamente acciones, como el envío de correo electrónico, cuando la regla se desencadena.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 42516e4dd6a85e0d07d4a8e70e958b2ec6e84aad
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225207"
---
# <a name="create-a-telemetry-rule-and-set-up-an-action-in-your-azure-iot-central-application"></a>Creación de una regla de telemetría y configuración de una acción en la aplicación de Azure IoT Central

Puede usar Microsoft Azure IoT Central para supervisar de forma remota los dispositivos conectados. Las reglas de Azure IoT Central le permiten supervisar los dispositivos casi en tiempo real e invocar automáticamente acciones, como el envío de un correo electrónico o el desencadenamiento del flujo de trabajo en Microsoft Flow cuando se cumplen las condiciones de la regla. En unos pocos clics, puede definir las condiciones para supervisar los datos del dispositivo y configurar la acción que se va a invocar. En este artículo se explica con detalle la regla de telemetría.

Azure IoT Central usa la [medida de telemetría](howto-set-up-template.md) para capturar los datos del dispositivo. Cada tipo de medida tiene atributos claves que definen la medida. Puede crear reglas para supervisar cada tipo de medida de dispositivos y generar alertas cuando la regla se desencadena. Una regla de telemetría se desencadena cuando la telemetría del dispositivo seleccionado supera un umbral especificado.

## <a name="create-a-telemetry-rule"></a>Creación de una regla de telemetría

En esta sección se muestra cómo crear una regla de telemetría. En este ejemplo se utiliza un dispositivo de aire acondicionado conectado que envía telemetría de temperatura y humedad. La regla supervisa la temperatura notificada por el dispositivo y envía un correo electrónico cada vez que sube de 80 grados.

1. Navegue a la página de detalles del dispositivo para el dispositivo al que va a agregar la regla.

1. Si aún no ha creado ninguna regla, consulte la siguiente pantalla:

    ![No hay ninguna regla todavía](media/howto-create-telemetry-rules/image1.png)

1. En la pestaña **Reglas**, elija **+Nueva regla** para ver los tipos de reglas que puede crear.

    ![Tipos de regla](media/howto-create-telemetry-rules/image2.png)

1. Elija el icono **Telemetría** para abrir el formulario para crear la regla.

    ![Regla de telemetría](media/howto-create-telemetry-rules/image3.png)

1. Elija un nombre que le ayude a identificar la regla en esta plantilla de dispositivo.

1. Para habilitar inmediatamente la regla para todos los dispositivos creados a partir esta plantilla, cambie el valor de **Habilitar regla**.

### <a name="configure-the-rule-condition"></a>Configuración de la condición de regla

En esta sección se muestra cómo agregar una condición para supervisar la telemetría de temperatura.

1. Seleccione **+** junto a **Condición**.

1. En la lista desplegable, elija el tipo de telemetría **Temperatura**. A continuación, seleccione el operador y proporcione un valor umbral. Puede agregar varias condiciones de telemetría. Cuando se especifican varias condiciones, deben cumplirse todas ellas para que la regla se desencadene.

    ![Adición de condición de telemetría](media/howto-create-telemetry-rules/image4.png)

    > [!NOTE]
    > Seleccione al menos una medida de telemetría cuando defina una condición de regla de telemetría.

1. Haga clic en **Guardar** para guardar la regla. La regla está activa en unos minutos e inicia la supervisión de telemetría que se envía a la aplicación.

### <a name="add-an-action"></a>Agregar una acción

En este ejemplo se muestra cómo agregar una acción a una regla. Se muestra cómo agregar la acción de correo electrónico, pero también puede agregar otras acciones:
-  [Acción de Microsoft Flow](howto-add-microsoft-flow.md) para iniciar un flujo de trabajo en Microsoft Flow cuando se desencadena una regla
- [Acción de Webhook](howto-create-webhooks.md) para notificar a otros servicios cuando se desencadena una regla

> [!NOTE]
> Solo se puede asociar una acción a una sola regla en este momento.

1. Seleccione **+** junto a **Acciones**. Aquí puede ver la lista de acciones disponibles.

    ![Adición de acción](media/howto-create-telemetry-rules/image5.png)

1. Elija la acción **Correo electrónico**, escriba una dirección de correo electrónico válida en el campo **Para** y proporcione una nota que aparecerá en el cuerpo del correo electrónico cuando la regla se desencadene.

    > [!NOTE]
    > Solo se envían mensajes de correo electrónico a los usuarios que se han agregado a la aplicación y han iniciado sesión al menos una vez. Obtenga más información sobre la [administración de usuarios](howto-administer.md) en Azure IoT Central.

   ![Configuración de acción](media/howto-create-telemetry-rules/image6.png)

1. Haga clic en **Save**(Guardar). La regla está activa en unos minutos e inicia la supervisión de telemetría que se envía a la aplicación. Si coincide con la condición especificada en la regla, la regla desencadena la acción de correo electrónico configurada.

## <a name="parameterize-the-rule"></a>Parametrización de la regla

Las reglas pueden derivar ciertos valores de las **propiedades del dispositivo**  como parámetros. El uso de parámetros es útil en aquellos escenarios en los que los umbrales de telemetría varían para diferentes dispositivos. Cuando cree la regla, elija una propiedad de dispositivo que especifique el umbral, como **umbral ideal máximo**, en lugar de proporcionar un valor absoluto, como 80 grados. Cuando la regla se ejecuta, hace coincidir la telemetría del dispositivo con el valor proporcionado en la propiedad del dispositivo.

El uso de parámetros es una forma eficaz de reducir el número de reglas que va a administrar la plantilla de dispositivo.

Las acciones también pueden configurarse mediante la **Propiedad de dispositivo** como un parámetro. Si una dirección de correo electrónico se almacena como una propiedad de dispositivo, se puede utilizar cuando se define la dirección **Para**.

## <a name="delete-a-rule"></a>Eliminar una regla

Si ya no necesita una regla, elimínela; para ello, abra la regla y seleccione **Eliminar**. Al eliminar la regla, se quita de la plantilla de dispositivos y todos los dispositivos asociados.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Habilitación o deshabilitación de una regla para una plantilla de dispositivo

Navegue hasta el dispositivo y elija la regla que desea habilitar o deshabilitar. El hecho de activar o desactivar el botón **Enable rule for all devices of this template** (Habilitar regla para todos los dispositivos de esta plantilla) en la regla habilita o deshabilita la regla para todos los dispositivos asociados a la plantilla de dispositivo.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Habilitación o deshabilitación de una regla para un dispositivo

Navegue hasta el dispositivo y elija la regla que desea habilitar o deshabilitar. Active o desactive el botón **Enable rule for this device** (Habilitar regla para este dispositivo) para habilitar o deshabilitar la regla para ese dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a editar reglas en una aplicación de Azure IoT Central, le sugerimos los pasos siguientes:

> [!div class="nextstepaction"]
> [Incorporación de una acción de Microsoft Flow a una regla](howto-add-microsoft-flow.md)
> [Administración de los dispositivos](howto-manage-devices.md)
