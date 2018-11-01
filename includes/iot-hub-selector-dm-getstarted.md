---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 13eddced155eab6dedfbce77330e7a178ecfb3cb
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164801"
---
> [!div class="op_single_selector"]
> * [Dispositivo: Servicio Node.js: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Dispositivo: Servicio C#: C#](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Dispositivo: Servicio de Java: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Dispositivo: Servicio de Python: Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Las aplicaciones back-end pueden usar primitivos de Azure IoT Hub, por ejemplo, [dispositivos gemelos][lnk-devtwin] y [métodos directos][lnk-c2dmethod], para iniciar y supervisar de forma remota las acciones de administración de dispositivos en los dispositivos. Este tutorial muestra cómo una aplicación back-end y un dispositivo pueden trabajar juntos para iniciar y supervisar el reinicio remoto de un dispositivo mediante IoT Hub.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Use un método directo para iniciar acciones de administración de dispositivos (por ejemplo, reinicio, restablecimiento de fábrica y actualización de firmware) desde una aplicación back-end en la nube. El dispositivo es responsable de:

* Controlar la solicitud del método enviada desde IoT Hub.
* Iniciar la acción específica del dispositivo correspondiente en el dispositivo.
* Proporcionar actualizaciones de estado mediante las *propiedades notificadas* a IoT Hub.

Puede usar una aplicación de back-end en la nube para ejecutar consultas de dispositivos gemelos para informar sobre el progreso de las acciones de administración de los dispositivos.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
