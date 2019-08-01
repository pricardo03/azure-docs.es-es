---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: eef073f5f4d1eb39fd5ccd8dafacd7074158fa37
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667844"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [.NET](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Los dispositivos gemelos son documentos JSON que almacenan información sobre el estado de los dispositivos (metadatos, configuraciones y condiciones). IoT Hub conserva un dispositivo gemelo por cada dispositivo que se conecta a él.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Use los dispositivos gemelos para:

* Almacenar metadatos del dispositivo desde el back-end de la solución.

* Notificar la información sobre el estado actual, como las funcionalidades y las condiciones disponibles (por ejemplo, el método de conectividad usado) de la aplicación del dispositivo.

* Sincronizar el estado de flujos de trabajo de larga duración (como las actualizaciones del firmware y de la configuración) entre la aplicación del dispositivo y la del back-end.

* Consultar los metadatos, la configuración o el estado del dispositivo.

Los dispositivos gemelos están diseñados para la sincronización y para consultar las condiciones y configuraciones del dispositivo. Para más información sobre cuándo usar dispositivos gemelos, consulte [Introducción a los dispositivos gemelos](../articles/iot-hub/iot-hub-devguide-device-twins.md).

Los dispositivos gemelos se almacenan en un IoT Hub y contienen:

* *etiquetas*, metadatos de dispositivo a los que solo puede acceder el back-end de la solución;

* *propiedades deseadas*, objetos JSON que puede modificar el back-end de la solución y puede observar la aplicación del dispositivo; y

* *propiedades indicadas*, objetos JSON que puede modificar la aplicación del dispositivo y que puede leer el back-end de la solución. Las etiquetas y propiedades no pueden contener matrices, pero se pueden anidar objetos.

![Imagen de dispositivo gemelo mostrando la funcionalidad](./media/iot-hub-selector-twin-get-started/twin.png)

Además, el back-end de la solución puede consultar los dispositivos gemelos en función de todos los datos descritos anteriormente.
Consulte [Introducción a los dispositivos gemelos](../articles/iot-hub/iot-hub-devguide-device-twins.md) para más información sobre los dispositivos gemelos y la referencia sobre el [lenguaje de consultas de IoT Hub](../articles/iot-hub/iot-hub-devguide-query-language.md) para realizar consultas.


En este tutorial se muestra cómo realizar las siguientes acciones:

* Crear una aplicación de back-end que agrega *etiquetas* a un dispositivo gemelo y una aplicación de dispositivo simulado que notifica su canal de conectividad como *propiedad notificada* en el dispositivo gemelo.

* Consultar dispositivos desde la aplicación de back-end mediante filtros en las etiquetas y propiedades que se han creado anteriormente.
