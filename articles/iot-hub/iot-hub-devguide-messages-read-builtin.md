---
title: Información sobre el punto de conexión integrado de Azure IoT Hub | Microsoft Docs
description: En esta guía del desarrollador se describe cómo usar el punto de conexión integrado compatible con Event Hubs para leer los mensajes del dispositivo a la nube.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 827d7d9a3d584342703a84dd2a42e5cda9b3a656
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364077"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Leer mensajes del dispositivo a la nube desde el punto de conexión integrado

De forma predeterminada, los mensajes se enrutan al punto de conexión orientado al servicio integrado (**/messages/events**), que es compatible con [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Actualmente, este punto de conexión solo se expone mediante el protocolo [AMQP](https://www.amqp.org/) en el puerto 5671. IoT Hub muestra las propiedades siguientes para permitirle controlar el punto de conexión de mensajería integrado compatible con Event Hubs **messages/events**.

| Propiedad            | DESCRIPCIÓN |
| ------------------- | ----------- |
| **Número de particiones** | Establezca esta propiedad durante la creación para definir el número de [particiones](../event-hubs/event-hubs-features.md#partitions) para ingesta de eventos del dispositivo a la nube. |
| **Tiempo de retención**  | Esta propiedad especifica cuánto tiempo, en días, IoT Hub conserva los mensajes. El valor predeterminado es un día, pero se puede aumentar a siete días. |

IoT Hub permite la retención de datos en los centros de eventos integrados para un máximo de 7 días. Puede establecer el tiempo de retención durante la creación de IoT Hub. Tiempo de retención de datos en IoT Hub depende de su nivel de IoT hub y el tipo de unidad. En cuanto a tamaño, los centros de eventos integrado puede conservar los mensajes del tamaño máximo del mensaje hasta al menos 24 horas de cuota. Por ejemplo, que IOT Hub proporciona suficiente almacenamiento para conservar al menos 1 unidad de S1 mensajes de 400 KB de 4k tamaño cada uno. Si los dispositivos envían mensajes más pequeños, puede conservarse durante más tiempo (hasta 7 días) según la cantidad de almacenamiento se consume. Garantizamos que conservar los datos durante el tiempo de retención especificado como mínimo.

IoT Hub también le permite administrar los grupos de consumidores en el punto de conexión de recepción de dispositivo a nube integrado.

Si usas [enrutamiento de mensajes](iot-hub-devguide-messages-d2c.md) y [ruta de reserva](iot-hub-devguide-messages-d2c.md#fallback-route) está habilitado, todos los mensajes que no coinciden con una consulta en cualquier ruta que se van a punto de conexión integrado. Si deshabilita esta ruta de reserva, se quitan los mensajes que no coinciden con cualquier consulta.

Puede modificar el tiempo de retención mediante programación con las [API REST del proveedor de recursos de IoT Hub](/rest/api/iothub/iothubresource) o con [Azure Portal](https://portal.azure.com).

IoT Hub expone el punto de conexión integrado **messages/events** para los servicios de back-end con el fin de leer los mensajes de dispositivo a nube recibidos por el centro. Este punto de conexión es compatible con Event Hubs, lo que permite usar cualquiera de los mecanismos del servicio Event Hubs para leer mensajes.

## <a name="read-from-the-built-in-endpoint"></a>Leer desde el punto de conexión integrado

Algunos integraciones de productos y SDK de Event Hubs son conscientes de IoT Hub y le permiten usar la cadena de conexión de servicio de IoT hub para conectarse al punto de conexión integrado.

Cuando se usa el SDK de Event Hubs o integraciones de productos que no detecten IoT Hub, necesita un punto de conexión compatible con Event hubs y un nombre compatible con Event hubs. Puede recuperar estos valores desde el portal como sigue:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.

2. Haga clic en **Puntos de conexión integrados**.

3. El **eventos** sección contiene los siguientes valores: **Las particiones**, **nombre compatible con Event Hub**, **punto de conexión compatible con Event hubs**, **tiempo de retención**, y **degruposdeconsumidores**.

    ![Configuración de dispositivo a nube](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

En el portal, el campo de punto de conexión compatible con Event hubs contiene una cadena de conexión de Event Hubs completa similar al siguiente: **Endpoint=sb://abcd1234namespace.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=keykeykeykeykeykey=;EntityPath=iothub-ehub-abcd-1234-123456**. Si el SDK usa requiere otros valores, a continuación, serían:

| NOMBRE | Valor |
| ---- | ----- |
| Punto de conexión | sb://abcd1234namespace.servicebus.windows.net/ |
| Nombre de host. | abcd1234namespace.servicebus.windows.net |
| Espacio de nombres | abcd1234namespace |

A continuación, puede usar cualquier directiva de acceso compartido que tenga permisos **ServiceConnect** para conectarse a la instancia de Event Hub especificada.

Los SDK que puede usar para conectarse al punto de conexión compatible con Event Hub integrado que muestra IoT Hub incluyen:

| Idioma | SDK | Ejemplo | Notas |
| -------- | --- | ------ | ----- |
| .NET | https://github.com/Azure/azure-event-hubs-dotnet | [Guía de inicio rápido](quickstart-send-telemetry-dotnet.md) | Usa la información de compatible con Event Hubs |
 Java | https://github.com/Azure/azure-event-hubs-java | [Guía de inicio rápido](quickstart-send-telemetry-java.md) | Usa la información de compatible con Event Hubs |
| Node.js | https://github.com/Azure/azure-event-hubs-node | [Guía de inicio rápido](quickstart-send-telemetry-node.md) | Usa la cadena de conexión de IoT Hub |
| Python | https://github.com/Azure/azure-event-hubs-python | https://github.com/Azure/azure-event-hubs-python/blob/master/examples/iothub_recv.py | Usa la cadena de conexión de IoT Hub |

Las integraciones de productos que puede usar con el punto de conexión compatible con Event Hub integrado que muestra IoT Hub incluyen:

* [Azure Functions](https://docs.microsoft.com/azure/azure-functions/). Consulte [procesar datos desde IoT Hub con Azure Functions](https://azure.microsoft.com/resources/samples/functions-js-iot-hub-processing/).
* [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/). Consulte [Stream datos como entrada en Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-iot-hub).
* [Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/). Consulte [agregar un origen de eventos de IoT hub al entorno de Time Series Insights](../time-series-insights/time-series-insights-how-to-add-an-event-source-iothub.md).
* [Spout de Apache Storm](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). Puede ver el [origen de spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) en GitHub.
* [Integración de Apache Spark](../hdinsight/spark/apache-spark-eventhub-streaming.md).
* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/).

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los puntos de conexión de IoT Hub, vea [Puntos de conexión de IoT Hub](iot-hub-devguide-endpoints.md).

* En las [guías de inicio rápidos](quickstart-send-telemetry-node.md) se muestra cómo enviar mensajes del dispositivo a la nube desde dispositivos simulados y cómo leer los mensajes desde el punto de conexión integrado. 

Para más información, consulte el tutorial [Procesamiento de mensajes de dispositivo a nube de IoT Hub mediante rutas](tutorial-routing.md).

* Si quiere enrutar los mensajes del dispositivo a la nube a puntos de conexión personalizados, consulte [Uso de rutas de mensajes y de puntos de conexión personalizados para mensajes de dispositivo a nube](iot-hub-devguide-messages-read-custom.md).
