---
title: Escalado de Azure IoT Hub | Microsoft Docs
description: Cómo escalar el centro de IoT Hub para respaldar el rendimiento de mensajes previsto y características deseadas. Incluye un resumen del rendimiento admitido para cada nivel y opciones de particionamiento.
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: wesmc
ms.openlocfilehash: 8ff99b8c5cf839afb8c8e73c18c2c413998ba64a
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52261725"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Elección del nivel adecuado de IoT Hub para la solución

Cada solución de IoT es diferente, por tanto Azure IoT Hub ofrece varias opciones en función del precio y la escala. El objetivo de este artículo es ayudarle a evaluar sus necesidades de IoT Hub. Para más información sobre los niveles de IoT Hub, consulte [Precios de IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub). 

Para decidir qué nivel de IoT Hub es el adecuado para la solución, hágase dos preguntas:

**¿Qué características voy a utilizar?**
Azure IoT Hub ofrece dos niveles, Basic y Estándar, que se diferencian en el número de características que admiten. Si la solución de IoT se basa en la recopilación de datos de los dispositivos y en su análisis de forma centralizada, el nivel Basic es probablemente el más adecuado. Si desea utilizar configuraciones más avanzadas para controlar dispositivos IoT de forma remota o distribuir algunas de las cargas de trabajo en los mismos dispositivos, debería considerar el nivel Estándar. Para obtener un análisis detallado de las características que se incluyen en cada nivel, continúe a la sección sobre los [niveles Basic y Estándar](#basic-and-standard-tiers).

**¿Qué cantidad de datos tiene previsto mover cada día?**
Cada nivel del IoT Hub está disponible en tres tamaños, basados en la cantidad de datos que pueden tratar en un día determinado. Estos tamaños se identifican numéricamente como 1, 2 y 3. Por ejemplo, cada unidad de un centro de IoT de nivel 1 puede controlar 400 000 mensajes al día, mientras que una unidad de nivel 3 puede controlar 300 millones. Para más información acerca de las directrices de datos, vaya a la sección [Rendimiento de mensajes](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Niveles Basic y Estándar

El nivel estándar de IoT Hub permite todas las características y es necesario para cualquier solución de IoT que desee hacer uso de las funcionalidades de comunicación bidireccional. El nivel Basic permite un subconjunto de las características y está pensado para las soluciones de IoT que solo necesitan comunicación unidireccional de los dispositivos a la nube. Ambos niveles ofrecen las mismas características de seguridad y autenticación.

Solo se puede elegir un tipo de [edición](https://azure.microsoft.com/pricing/details/iot-hub/) dentro de un nivel por cada instancia de IoT Hub. Por ejemplo, puede crear una instancia de IoT Hub con varias unidades de S1, pero no con una combinación de unidades de versiones distintas como, por ejemplo, S1 y B3, o S1 y S2.

| Capacidad | Nivel Basic | Nivel Standard |
| ---------- | ---------- | ------------- |
| [Telemetría del dispositivo a la nube](iot-hub-devguide-messaging.md) | SÍ | SÍ |
| [Identidad por dispositivo](iot-hub-devguide-identity-registry.md) | SÍ | SÍ |
| [Enrutamiento de mensajes](iot-hub-devguide-messages-read-custom.md) e [integración con Event Grid](iot-hub-event-grid.md) | SÍ | SÍ |
| [Protocolos HTTP, AMQP y MQTT](iot-hub-devguide-protocols.md) | SÍ | SÍ |
| [Servicio Device Provisioning](../iot-dps/about-iot-dps.md) | SÍ | SÍ |
| [Supervisión y diagnóstico](iot-hub-monitor-resource-health.md) | SÍ | SÍ |
| [Mensajería de la nube a un dispositivo](iot-hub-devguide-c2d-guidance.md) |   | SÍ |
| [Dispositivos gemelos](iot-hub-devguide-device-twins.md), [Módulos gemelos](iot-hub-devguide-module-twins.md) y [Administración de dispositivos](iot-hub-device-management-overview.md) |   | SÍ |
| [Azure IoT Edge](../iot-edge/about-iot-edge.md) |   | SÍ |

IoT Hub también ofrece un nivel gratis que está diseñado para pruebas y evaluación. Tiene todas las capacidades del nivel estándar, pero las concesiones de mensajería son limitadas. No puede actualizar desde el nivel gratis al plan Básico o Estándar. 


## <a name="partitions"></a>Particiones

Azure IoT Hub contiene muchos componentes principales de [Azure Event Hubs](../event-hubs/event-hubs-features.md), incluidas las [particiones](../event-hubs/event-hubs-features.md#partitions). Los flujos de eventos de IoT Hub generalmente se rellenan con datos de telemetría entrantes que se notifican mediante varios dispositivos de IoT. La creación de particiones del flujo de eventos se usa para reducir las contenciones que se producen al leer y escribir simultáneamente en flujos de eventos. 

El límite de particiones se elige cuando se crea la instancia de IoT Hub y no se puede cambiar. El límite máximo de particiones para el nivel básico de IoT Hub es 8 y para el nivel estándar el máximo es 32. La mayoría de instancias de IoT Hub solo necesitan 4 particiones. Para más información para determinar el número de particiones, consulte esta pregunta frecuente sobre Event Hubs [¿Cuántas particiones necesito?](../event-hubs/event-hubs-faq.md#how-many-partitions-do-i-need)


## <a name="tier-upgrade"></a>Actualización del plan

Una vez creado el centro de IoT, puede actualizarlo desde el nivel Basic al nivel Estándar sin interrumpir las operaciones existentes. Para más información, consulte [How to upgrade your IoT hub](iot-hub-upgrade.md) (Actualización de IoT Hub).

La configuración de la partición permanecerá invariable cuando migre de un nivel básico a un nivel estándar.


## <a name="iot-hub-rest-apis"></a>API REST de IoT Hub

La diferencia de funcionalidades admitidas entre los niveles Basic y Estándar de IoT Hub significa que algunas llamadas a la API no funcionan con centros de nivel Basic. En la tabla siguiente se muestran las API que están disponibles: 

| API | Nivel Basic | Nivel Standard |
| --- | ---------- | ------------- |
| [Eliminar un dispositivo](https://docs.microsoft.com/rest/api/iothub/service/deletedevice) | SÍ | SÍ |
| [Obtener dispositivo](https://docs.microsoft.com/rest/api/iothub/service/getdevice) | SÍ | SÍ |
| Eliminar módulo | SÍ | SÍ |
| Obtener módulo | SÍ | SÍ |
| [Obtener estadísticas del registro](https://docs.microsoft.com/rest/api/iothub/service/getdeviceregistrystatistics) | SÍ | SÍ |
| [Obtener estadísticas de servicios](https://docs.microsoft.com/rest/api/iothub/service/getservicestatistics) | SÍ | SÍ |
| [Crear o actualizar el dispositivo](https://docs.microsoft.com/rest/api/iothub/service/createorupdatedevice) | SÍ | SÍ |
| Colocar módulo | SÍ | SÍ |
| [Consultar IoT Hub](https://docs.microsoft.com/rest/api/iothub/service/queryiothub) | SÍ | SÍ |
| Consultar módulos | SÍ | SÍ |
| [Crear el URI de SAS de carga de archivos](https://docs.microsoft.com/rest/api/iothub/device/createfileuploadsasuri) | SÍ | SÍ |
| [Recibir notificación de dispositivo enlazado](https://docs.microsoft.com/rest/api/iothub/device/receivedeviceboundnotification) | SÍ | SÍ |
| [Enviar evento de dispositivo](https://docs.microsoft.com/rest/api/iothub/device/senddeviceevent) | SÍ | SÍ |
| Enviar eventos de módulo | SÍ | SÍ |
| [Actualizar estado de la carga de archivo](https://docs.microsoft.com/rest/api/iothub/device/updatefileuploadstatus) | SÍ | SÍ |
| [Operación de dispositivos en bloque](/rest/api/iot-dps/runbulkenrollmentoperation/runbulkenrollmentoperation) | Sí, excepto las funcionalidades de IoT Edge | SÍ | 
| [Purgar cola de comandos](https://docs.microsoft.com/rest/api/iothub/service/purgecommandqueue) |   | SÍ |
| [Obtener dispositivo gemelo](https://docs.microsoft.com/rest/api/iothub/service/gettwin) |   | SÍ |
| Obtener módulo gemelo |   | SÍ |
| [Invocar método de dispositivo](https://docs.microsoft.com/rest/api/iothub/service/invokedevicemethod) |   | SÍ |
| [Actualizar dispositivo gemelo](https://docs.microsoft.com/rest/api/iothub/service/updatetwin) |   | SÍ | 
| Actualizar módulo gemelo |   | SÍ | 
| [Abandonar notificación de dispositivo enlazado](https://docs.microsoft.com/rest/api/iothub/device/abandondeviceboundnotification) |   | SÍ |
| [Completar notificación de dispositivo enlazado](https://docs.microsoft.com/rest/api/iothub/device/completedeviceboundnotification) |   | SÍ |
| [Cancelar trabajo](https://docs.microsoft.com/rest/api/iothub/service/canceljob) |   | SÍ |
| [Crear trabajo](https://docs.microsoft.com/rest/api/iothub/service/createjob) |   | SÍ |
| [Obtener trabajo](https://docs.microsoft.com/rest/api/iothub/service/getjob) |   | SÍ |
| [Consultar trabajos](https://docs.microsoft.com/rest/api/iothub/service/queryjobs) |   | SÍ |

## <a name="message-throughput"></a>Rendimiento de mensajes

La mejor forma de dimensionar una solución de IoT Hub es evaluar el tráfico en cada dispositivo. En concreto, tenga en cuenta la capacidad de procesamiento máxima requerida para las siguientes categorías de operaciones:

* Mensajes de dispositivo a nube
* Mensajes de nube a dispositivo
* Operaciones de registro de identidad

El tráfico se mide por unidad, no por centro. Una instancia de IoT Hub de nivel 1 o 2 puede tener hasta 200 unidades asociadas. Una instancia de IoT Hub de nivel 3 puede tener hasta 10 unidades. Una vez que creado el IoT Hub, puede cambiar el número de unidades o moverse entre los tamaños 1, 2 y 3 dentro de un nivel específico sin interrumpir las operaciones existentes. Para más información, consulte [How to upgrade your IoT hub](iot-hub-upgrade.md) (Actualización de IoT Hub).

Como ejemplo de las funcionalidades de tráfico de cada nivel, los mensajes del dispositivo a la nube siguen estas directrices de rendimiento sostenidas:

| Nivel | Capacidad de procesamiento sostenida | Velocidad de envío sostenida |
| --- | --- | --- |
| B1, S1 |Hasta 1111 KB/minuto por unidad<br/>(1,5 GB/día/unidad) |Promedio de 278 mensajes/minuto por unidad<br/>(400 000 mensajes/día por unidad) |
| B2, S2 |Hasta 16 MB/minuto por unidad<br/>(22,8 GB/día/unidad) |Promedio de 4167 mensajes/minuto por unidad<br/>(6 millones de mensajes/día por unidad) |
| B3, S3 |Hasta 814 MB/minuto por unidad<br/>(1144,4 GB/día/unidad) |Promedio de 208.333 mensajes/minuto por unidad<br/>(300 millones de mensajes/día por unidad) |

Además de esta información sobre la capacidad de procesamiento, vea [Cuotas y limitaciones de IoT Hub][IoT Hub quotas and throttles] y diseñe su solución en consecuencia.

### <a name="identity-registry-operation-throughput"></a>Capacidad de procesamiento para las operaciones de registro de identidad
Las operaciones de registro de identidad de IoT Hub no deberían ser operaciones en tiempo de ejecución porque tienen que ver principalmente con el aprovisionamiento de dispositivos.

Vea las cifras de rendimiento de ráfaga específicas en [Cuotas y limitaciones de IoT Hub][IoT Hub quotas and throttles].

## <a name="auto-scale"></a>Escalado automático
Si se está aproximando al límite de mensajes permitido por IoT Hub, puede usar estos [pasos para realizar un escalado automático](https://azure.microsoft.com/resources/samples/iot-hub-dotnet-autoscale/) que aumente una unidad de IoT Hub en el mismo nivel.

## <a name="sharding"></a>Particionamiento
Aunque un único Centro de IoT puede escalarse a millones de dispositivos, a veces la solución requiere características de rendimiento específicas que un único Centro de IoT no puede garantizar. En ese caso puede repartir los dispositivos en varios centros de IoT. Varios centros de IoT suavizan las ráfagas de tráfico y obtienen el rendimiento necesario o las velocidades de funcionamiento necesarias.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las funcionalidades de IoT Hub y detalles de rendimiento, consulte [Precios de IoT Hub][lnk-pricing] o [Cuotas y limitación de IoT Hub][IoT Hub quotas and throttles].
* Para cambiar el nivel de IoT Hub, siga los pasos descritos en el artículo sobre cómo [actualizar IoT Hub](iot-hub-upgrade.md).

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
