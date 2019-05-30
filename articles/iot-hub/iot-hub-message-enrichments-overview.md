---
title: Información general del grado de enriquecimiento del mensaje de Azure IoT Hub
description: Información general del grado de enriquecimiento del mensaje para los mensajes de IoT Hub de Azure
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: 2de945d2ef1d87b47ea81aac034916466d3c9bda
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258855"
---
# <a name="message-enrichments-for-device-to-cloud-iot-hub-messages-preview"></a>Grado de enriquecimiento del mensaje para mensajes de IoT Hub del dispositivo a nube (versión preliminar)

*Mensaje enriquecimientos* es la capacidad de IoT Hub a *marca* mensajes con información adicional antes de que los mensajes se envían al punto de conexión designado. Una razón para utilizar el grado de enriquecimiento del mensaje es incluir datos que se pueden usar para simplificar el procesamiento de bajada. Por ejemplo, enriquecimiento de mensajes de telemetría de dispositivo con una etiqueta de dispositivo gemelo puede reducir la carga en los clientes para realizar llamadas de API para obtener esta información de dispositivo gemelo.

![Flujo de mensajes enriquecimientos](./media/iot-hub-message-enrichments-overview/message-enrichments-flow.png)

El enriquecimiento de mensajes tiene tres elementos clave:

* Enriquecimiento nombre o la clave

* Un valor

* Uno o varios [extremos](iot-hub-devguide-endpoints.md) para que se debe aplicar el enriquecimiento.

La clave puede ser cualquier cadena.

El valor puede ser cualquiera de los ejemplos siguientes:

* Cualquier cadena estática. No se permiten valores dinámicos como condiciones, lógica, las operaciones y funciones. Por ejemplo, si desarrolla una aplicación SaaS que usa varios clientes, puede asignar un identificador a cada cliente y asegúrese de que el identificador disponible en la aplicación. Cuando se ejecuta la aplicación, IoT Hub se marca el dispositivo de mensajes de telemetría con el identificador del cliente, lo que permite procesar los mensajes de forma diferente para cada cliente.

* Información del dispositivo gemelo, como su ruta de acceso. Algunos ejemplos serían *$twin.tags.field* y *$twin.tags.latitude*.

* El nombre de IoT hub envía el mensaje. Este valor es *$iothubname*.

## <a name="applying-enrichments"></a>Aplicar enriquecimientos

Los mensajes pueden proceder de cualquier origen de datos admitido por [enrutamiento de mensajes de IoT Hub](iot-hub-devguide-messages-d2c.md), incluidos los ejemplos siguientes:

* telemetría del dispositivo, como temperatura, presión
* notificaciones de cambio de dispositivo gemelo: cambios en el dispositivo gemelo
* eventos de ciclo de vida de dispositivo, como cuando se crea o elimina el dispositivo

Puede agregar enriquecimientos a mensajes que se van a punto de conexión integrado de un centro de IoT, o que se están enrutando a puntos de conexión personalizados, como Azure Blob storage, una cola de Service Bus o un tema de Service Bus.

Se aplican enriquecimientos de cada punto de conexión. Si especifica cinco enriquecimientos de que se marcarán para un extremo específico, todos los mensajes que se va a ese punto de conexión se marcan con el mismos enriquecimientos de cinco.

Para ver cómo probar el grado de enriquecimiento del mensaje, consulte el [tutorial enriquecimientos de mensaje](tutorial-message-enrichments.md)

## <a name="limitations"></a>Limitaciones

* Puede agregar hasta 10 enriquecimientos por IoT Hub para esos centros en el nivel estándar o básico. Centros de IoT en el nivel gratis, puede agregar hasta 2 enriquecimientos.

* En algunos casos, si está aplicando un enriquecimiento con un valor establecido en una etiqueta o una propiedad en el dispositivo gemelo, el valor se marcarán como un valor de cadena. Por ejemplo, si se establece un valor de enriquecimiento en $twin.tags.field, los mensajes se marcarán con la cadena "$twin.tags.field" en lugar del valor de ese campo desde el gemelo. Esto sucede en los casos siguientes:

   * IoT Hub no está en el nivel básico. Centros de IoT de nivel básico no admiten a los dispositivos gemelos.

   * IoT Hub no está en el nivel estándar, pero el dispositivo que envía el mensaje no tiene ningún dispositivo gemelo.

   * IoT Hub no está en el nivel estándar, pero no existe la ruta de acceso de dispositivo gemelo usa para el valor del enriquecimiento. Por ejemplo, si se establece el valor de enriquecimiento en $twin.tags.location y el dispositivo gemelo no tiene una propiedad de ubicación en las etiquetas, el mensaje está marcado con la cadena "$twin.tags.location". 

* Las actualizaciones de un dispositivo gemelo pueden tardar hasta cinco minutos en quedar reflejados en el valor correspondiente de enriquecimiento.

* El tamaño total del mensaje, incluido el grado de enriquecimiento del, no puede superar los 256 KB. Si el tamaño del mensaje supera los 256 KB, el centro de IoT se quitará el mensaje. Puede usar [métricas de IoT Hub](iot-hub-metrics.md) para identificar y depurar errores cuando se quitan los mensajes. Por ejemplo, puede supervisar d2c.telemetry.egress.invalid.

## <a name="pricing"></a>Precios

Grado de enriquecimiento del mensaje está disponible sin ningún cargo adicional. Actualmente, se le cobra cuando envía un mensaje a IoT Hub. Solo se cobran una vez para ese mensaje, incluso si el mensaje llega a varios puntos de conexión.

## <a name="availability"></a>Disponibilidad

Esta funcionalidad está disponible en versión preliminar y está disponible en todas las regiones excepto este de Estados Unidos, oeste de Estados Unidos, Europa occidental, [Azure Government](/azure-government/documentation-government-welcome.md), [Azure China 21Vianet](/azure/china/china-welcome.md), y [Azure Germany](https://azure.microsoft.com/global-infrastructure/germany/).

## <a name="next-steps"></a>Pasos siguientes

Consulte estos artículos para obtener más información sobre el enrutamiento de mensajes a IoT Hub:

* [Usar el enrutamiento de mensajes de IoT Hub para enviar mensajes de dispositivo a nube a distintos puntos de conexión](iot-hub-devguide-messages-d2c.md)

* [Tutorial: Enrutamiento de IoT Hub](tutorial-routing.md)