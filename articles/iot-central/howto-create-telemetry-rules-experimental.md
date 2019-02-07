---
title: Creación y administración de reglas de telemetría en una aplicación de Azure IoT Central | Microsoft Docs
description: Las reglas de telemetría de Azure IoT Central le permiten supervisar los dispositivos casi en tiempo real e invocar automáticamente acciones, como el envío de correo electrónico, cuando la regla se desencadena.
author: ankitgupta
ms.author: ankitgup
ms.date: 02/02/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1d6f43b23bddf2d1ff7a2a41a11b4a2c8623d372
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768631"
---
# <a name="create-a-telemetry-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Creación de una regla de telemetría y configuración de las notificaciones en la aplicación de Azure IoT Central

*Este artículo se aplica a los administradores, operadores y compiladores.*

Puede usar Azure IoT Central para supervisar de forma remota los dispositivos conectados. Las reglas de Azure IoT Central le permiten supervisar los dispositivos casi en tiempo real e invocar acciones automáticamente, como el envío de correo electrónico o el desencadenamiento de Microsoft Flow. En unos pocos clics, puede definir la condición para la cual supervisar los datos del dispositivo y configurar la acción correspondiente. En este artículo se explica cómo crear reglas para supervisar los datos de telemetría enviados por el dispositivo.

Los dispositivos pueden usar la medición de telemetría para enviar datos numéricos desde el dispositivo. Una regla de telemetría se desencadena cuando la telemetría del dispositivo seleccionado supera un umbral especificado.

## <a name="create-a-telemetry-rule"></a>Creación de una regla de telemetría

Para crear una regla de telemetría, la plantilla de dispositivos debe tener definida al menos una medida de telemetría. En este ejemplo se utiliza un dispositivo de máquina expendedora refrigerada que envía datos de telemetría de temperatura y humedad. La regla supervisa la temperatura notificada por el dispositivo y envía un correo electrónico cada vez que sube de 80 grados.

1. Con Device Explorer, vaya a la plantilla de dispositivos para la que va a agregar la regla.

1. En la plantilla seleccionada, haga clic en un dispositivo existente. 

    >[!TIP]
    >Si la plantilla no tiene ningún dispositivo, primero agregue uno.

1. Si aún no ha creado ninguna regla, verá la siguiente pantalla:

    ![No hay ninguna regla todavía](media/howto-create-telemetry-rules-experimental/Rules_Landing_Page.png)

1. En la pestaña **Reglas**, haga clic en **+ Nueva regla** para ver los tipos de reglas que puede crear.

1. Haga clic en **Telemetría** para crear una regla para supervisar los datos de telemetría del dispositivo.

    ![Tipos de regla](media/howto-create-telemetry-rules-experimental/Rule_Types.png)

1. Escriba un nombre que le ayude a identificar la regla en esta plantilla de dispositivos.

1. Para habilitar inmediatamente la regla para todos los dispositivos creados a partir esta plantilla, cambie el valor de **Enable rule for all devices of this template** (Habilitar regla para todos los dispositivos de esta plantilla).

   ![Detalle de la regla](media/howto-create-telemetry-rules-experimental/Rule_Detail.png)

    La regla se aplica automáticamente a todos los dispositivos de la plantilla de dispositivos.

### <a name="configure-the-rule-conditions"></a>Configuración de las condiciones de la regla

La condición define los criterios que la regla supervisa.

1. Haga clic en **+** junto a **Condiciones** para agregar una condición nueva.

1. Seleccione los datos de telemetría que desee supervisar en la lista desplegable **Medida**.

   ![Condición](media/howto-create-telemetry-rules-experimental/Aggregate_Condition_Filled_Out.png)

1. A continuación, elija **Agregación**, **Operador** y proporcione un valor de **Umbral**.
    - La agregación es opcional. Sin la agregación, la regla se desencadena para cada punto de datos de telemetría que cumple la condición. Por ejemplo, si la regla está configurada para desencadenarse cuando la temperatura está por encima de 80, la regla se desencadenará casi al instante cuando el dispositivo informe una temperatura >80.
    - Si se elige una función de agregado, como Promedio, Mín., Máx., Recuento, el usuario debe proporcionar una **ventana de tiempo agregado** en la cual debe evaluarse la condición. Por ejemplo, si establece el período como "5 minutos" y la regla busca la temperatura promedio por encima de 80, la regla se desencadena cuando la temperatura promedio está por encima de 80 durante al menos 5 minutos. La frecuencia de evaluación de la regla es la misma que la **ventana de tiempo agregado**, lo que significa que, en este ejemplo, la regla se evalúa una vez cada 5 minutos.

    >[!NOTE]
    >En **Condición** se puede agregar más de una medición de datos de telemetría. Cuando se especifican varias condiciones, deben cumplirse todas ellas para que la regla se desencadene. Cada condición se une mediante una cláusula "AND" de manera implícita. Al usar el agregado, se debe agregar cada medida.

### <a name="configure-actions"></a>Configuración de acciones

En esta sección se muestra cómo configurar las acciones que se realizarán cuando se active la regla. Se invocan las acciones cuando todas las condiciones especificadas en la regla se evalúan como verdaderas.

1. Seleccione **+** junto a **Acciones**. Aquí puede ver la lista de acciones disponibles.  

    ![Adición de acción](media/howto-create-telemetry-rules-experimental/Add_Action.png)

1. Elija la acción **Correo electrónico**, escriba una dirección de correo electrónico válida en el campo **Para** y proporcione una nota que aparecerá en el cuerpo del correo electrónico cuando la regla se desencadene.

    > [!NOTE]
    > Solo se envían mensajes de correo electrónico a los usuarios que se han agregado a la aplicación y han iniciado sesión al menos una vez. Obtenga más información sobre la [administración de usuarios](howto-administer-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) en Azure IoT Central.

   ![Configuración de acción](media/howto-create-telemetry-rules-experimental/Configure_Action.png)

1. Para guardar la regla, elija **Guardar**. La regla está activa en unos minutos e inicia la supervisión de telemetría que se envía a la aplicación. Cuando se cumple la condición especificada en la regla, la regla desencadena la acción de correo electrónico configurada.

Puede agregar otras acciones a la regla, como Microsoft Flow y webhooks. Puede agregar hasta 5 acciones por regla.

- [Acción de Microsoft Flow](howto-add-microsoft-flow-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) para iniciar un flujo de trabajo en Microsoft Flow cuando se desencadena una regla 
- [Acción de Webhook](howto-create-webhooks-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) para notificar a otros servicios cuando se desencadena una regla

## <a name="parameterize-the-rule"></a>Parametrización de la regla

Las reglas pueden derivar ciertos valores de las **propiedades del dispositivo**  como parámetros. El uso de parámetros es útil en aquellos escenarios en los que los umbrales de telemetría varían para diferentes dispositivos. Cuando cree la regla, elija una propiedad de dispositivo que especifique el umbral, como **umbral ideal máximo**, en lugar de proporcionar un valor absoluto, como 80 grados. Cuando la regla se ejecuta, hace coincidir los datos de telemetría del dispositivo con el valor establecido en la propiedad del dispositivo.

El uso de parámetros es una forma eficaz de reducir el número de reglas que va a administrar la plantilla de dispositivo.

Las acciones también pueden configurarse mediante la **Propiedad de dispositivo** como un parámetro. Si una dirección de correo electrónico se almacena como propiedad, se puede utilizar cuando se define la dirección **Para**.

## <a name="delete-a-rule"></a>Eliminar una regla

Si ya no necesita una regla, elimínela; para ello, abra la regla y seleccione **Eliminar**. Al eliminar la regla, se quita de la plantilla de dispositivos y todos los dispositivos asociados.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Habilitación o deshabilitación de una regla para una plantilla de dispositivo

Navegue hasta el dispositivo y elija la regla que desea habilitar o deshabilitar. Alterne el botón **Enable rule for all devices of this template** (Habilitar regla para todos los dispositivos de esta plantilla) en la regla para habilitar o deshabilitar la regla para todos los dispositivos asociados a la plantilla de dispositivos.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Habilitación o deshabilitación de una regla para un dispositivo

Navegue hasta el dispositivo y elija la regla que desea habilitar o deshabilitar. Active o desactive el botón **Enable rule for this device** (Habilitar regla para este dispositivo) para habilitar o deshabilitar la regla para ese dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear reglas en una aplicación de Azure IoT Central, aquí tiene algunos de los pasos siguientes:

- [Incorporación de una acción de Microsoft Flow a las reglas](howto-add-microsoft-flow-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Incorporación de una acción de Webhook a las reglas](howto-create-webhooks-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Administración de los dispositivos](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
