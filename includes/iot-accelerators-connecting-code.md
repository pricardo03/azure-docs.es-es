---
title: archivo de inclusión
description: archivo de inclusión
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 09/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: c79b6f854dc78670a7eb8a1275c3e2fc46fcdd99
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61450227"
---
### <a name="code-walkthrough"></a>Tutorial de código

En esta sección se describen algunas de las partes principales del código de ejemplo y se explica cómo se relacionan con el acelerador de soluciones de supervisión remota.

El siguiente fragmento de código muestra cómo se definen las propiedades notificadas que describen las funcionalidades del dispositivo. Estas propiedades incluyen:

- La ubicación del dispositivo para habilitar que el acelerador de soluciones agregue el dispositivo a la asignación.
- La versión actual del firmware.
- La lista de métodos que admite el dispositivo.
- El esquema de los mensajes de telemetría enviados por el dispositivo.



El ejemplo incluye una función **serializeToJson** que serializa esta estructura de datos mediante la biblioteca Parson.

El ejemplo incluye varias funciones de devolución de llamada que imprimen información en la consola cuando el cliente interactúa con el acelerador de soluciones:

- **connection_status_callback**
- **send_confirm_callback**
- **reported_state_callback**
- **device_method_callback**

El siguiente fragmento de código muestra la función **device_method_callback**. Esta función determina la acción que se realiza cuando se recibe la llamada a un método desde el acelerador de soluciones. La función recibe una referencia a la estructura de datos de **Chiller** en el parámetro **userContextCallback**. El valor de **userContextCallback** se establece cuando la función de devolución de llamada se configura en la función **main**:



Cuando el acelerador de soluciones llama al método de actualización del firmware, el ejemplo deserializa la carga de JSON e inicia un subproceso en segundo plano para completar el proceso de actualización. El siguiente fragmento de código muestra el **do_firmware_update** que se ejecuta en el subproceso:



El siguiente fragmento de código muestra la forma en que el cliente envía un mensaje de telemetría al acelerador de soluciones. Las propiedades del mensaje incluyen el esquema del mensaje que ayuda al acelerador de soluciones a mostrar la telemetría en el panel:



La función **main** del ejemplo:

- Inicializa y cierra el subsistema del SDK.
- Inicializa la estructura de datos **Chiller**.
- Envía las propiedades notificadas al acelerador de soluciones.
- Configura la función de devolución de llamada del método del dispositivo.
- Envía valores de telemetría simulados al acelerador de soluciones.


