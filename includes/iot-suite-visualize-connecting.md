---
title: archivo de inclusión
description: archivo de inclusión
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 9b9e28f18208674609d0842b0e3a54e3fc661c9f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66147745"
---
## <a name="view-device-telemetry"></a>Ver la telemetría de dispositivo

Puede ver los datos de telemetría enviados desde el dispositivo el **Device Explorer** página en la solución.

1. Seleccione el dispositivo que aprovisionó en la lista de dispositivos de la **Device Explorer** página. Un panel muestra información sobre el dispositivo incluyendo un trazado de su telemetría:

    ![Ver detalles del dispositivo](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Elija **Presión** para cambiar la presentación de telemetría:

    ![Ver la telemetría de presión](media/iot-suite-visualize-connecting/devicespressure.png)

1. Para ver información de diagnóstico sobre el dispositivo, desplácese hacia abajo hasta **Diagnósticos**:

    ![Ver el diagnóstico de los dispositivos](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Actúe en su dispositivo

Para invocar métodos en los dispositivos, use el **Device Explorer** página en la solución de supervisión remota. Por ejemplo, en la solución de supervisión remota **refrigerador** dispositivos implementan un **reiniciar** método.

1. Elija **dispositivos** para navegar hasta el **Device Explorer** página en la solución.

1. Seleccione el dispositivo que aprovisionó en la lista de dispositivos de la **Device Explorer** página:

    ![Selección del dispositivo real](media/iot-suite-visualize-connecting/devicesselect.png)

1. Para mostrar una lista de los métodos se pueden llamar en el dispositivo, elija **trabajos**, a continuación, **métodos**. Para programar un trabajo para ejecutarse en varios dispositivos, puede seleccionarlos en la lista. El panel **Trabajos** muestra los tipos de métodos comunes a todos los dispositivos seleccionados.

1. Elija **reiniciar**, Establece el nombre del trabajo en **RebootPhysicalChiller** y, a continuación, elija **aplicar**:

    ![Programación de la actualización de firmware](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Aparece una secuencia de mensajes en la consola donde se ejecuta el código del dispositivo cuando el dispositivo controla el método.

> [!NOTE]
> Para realizar un seguimiento del estado del trabajo en la solución, elija **ver estado del trabajo**.

## <a name="next-steps"></a>Pasos siguientes

En el artículo [Personalización de la solución preconfigurada de supervisión remota](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) se describen algunas maneras de personalizar el acelerador de soluciones.