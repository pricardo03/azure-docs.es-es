---
title: Comparación de Azure IoT Hub con Azure Event Hubs | Microsoft Docs
description: Comparación de los servicios de IoT Hub y los servicios de Event Hubs de Azure en la que se resaltan diferencias funcionales y casos de uso. La comparación incluye protocolos admitidos, administración de dispositivos, supervisión y cargas de archivos.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 9ad95071de07777e38533ecec9e8558841d8b1ca
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633968"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>Conexión de dispositivos IoT a Azure: IoT Hub y Event Hubs

Azure proporciona servicios desarrollados específicamente para diversos tipos de conectividad y comunicación para ayudar a conectar los datos a la tecnología de la nube. Tanto Azure IoT Hub como Azure Event Hubs son servicios en la nube que pueden ingerir grandes cantidades de datos y procesar o almacenar esos datos para obtener perspectivas empresariales. Los dos servicios se asemejan en que ambos admiten la ingesta de datos con baja latencia y alta fiabilidad, pero están diseñados para propósitos diferentes. IoT Hub se desarrolló específicamente para satisfacer los requisitos únicos de la conexión de dispositivos IoT, a escala, a la nube de Azure, mientras que Event Hubs se diseñó para el flujo de macrodatos. Por eso, Microsoft recomienda el uso de Azure IoT Hub para conectar dispositivos IoT a Azure

Azure IoT Hub es la puerta de enlace en la nube que conecta dispositivos IoT para recopilar los datos que dirigirán las perspectivas y automatización empresariales. Además, IoT Hub incluye características que enriquecen la relación entre los dispositivos y los sistemas back-end. Las capacidades de comunicación bidireccional implican que, mientras se reciben datos de los dispositivos, también es posible enviar comandos y directivas a los dispositivos, por ejemplo, para actualizar las propiedades o invocar acciones de administración de dispositivos.  Esta conectividad de nube a dispositivo también ofrece la importante capacidad de entregar inteligencia de nube a los dispositivos perimetrales con Azure IoT Edge. La identidad única de nivel de dispositivo proporcionada por IoT Hub ayuda a proteger mejor la solución IoT frente a ataques potenciales. 

[Azure Event Hubs] [ Azure Event Hubs] es el servicio de flujo de macrodatos de Azure. Está diseñado para los escenarios de flujo de datos de alto rendimiento en los que los clientes pueden enviar miles de millones de solicitudes al día. Event Hubs usa un modelo de consumidor con particiones para escalar horizontalmente el flujo de datos y se integra en los servicios de macrodatos y análisis de Azure, entre ellos, Databricks, Stream Analytics, ADLS y HDInsight. Gracias a características como Event Hubs Capture y el inflado automático, el diseño de este servicio admite soluciones y aplicaciones de macrodatos. Además, IoT Hub utiliza Event Hubs para la ruta de acceso del flujo de telemetría, por lo que la solución IoT también se beneficia de la gran eficacia de Event Hubs.

En resumen, aunque ambas soluciones están diseñadas para la ingesta de datos a escala masiva, solo IoT Hub proporciona las abundantes capacidades específicas de IoT diseñadas para maximizar el valor empresarial de conectar los dispositivos IoT a la nube de Azure.  Si acaba de iniciar su experiencia en IoT, al empezar por IoT Hub para admitir los escenarios de ingesta de datos garantizará el acceso inmediato a las capacidades de IoT con todas las características cuando las necesidades empresariales y técnicas las requieran.

En la tabla siguiente se proporcionan detalles de la comparación entre dos niveles de IoT Hub y Event Hubs al valorarlas en función de su capacidad de IoT. Para obtener más información acerca de los niveles básico y estándar de IoT Hub, consulte [Escalado de la solución de IoT Hub][lnk-scaling].

| Funcionalidad de IoT | Nivel estándar de IoT Hub | Nivel básico de IoT Hub | Event Hubs |
| --- | --- | --- | --- |
| Mensajería de un dispositivo a la nube | ![Comprobar][1] | ![Comprobar][1] | ![Comprobar][1] |
| Protocolos: HTTPS, AMQP, AMQP sobre WebSockets | ![Comprobar][1] | ![Comprobar][1] | ![Comprobar][1] |
| Protocolos: MQTT, MQTT sobre WebSockets | ![Comprobar][1] | ![Comprobar][1] |  |
| Identidad por dispositivo | ![Comprobar][1] | ![Comprobar][1] |  |
| Carga de archivos desde dispositivos | ![Comprobar][1] | ![Comprobar][1] |  |
| Servicio Device Provisioning | ![Comprobar][1] | ![Comprobar][1] |  |
| Mensajería de la nube a un dispositivo | ![Comprobar][1] |  |  |
| Administración de dispositivos y dispositivos gemelos | ![Comprobar][1] |  |  |
| IoT Edge | ![Comprobar][1] |  |  |

Aunque el único uso sea la ingesta de datos de dispositivo a nube, es muy recomendable utilizar IoT Hub porque ofrece un servicio que está diseñado para la conectividad de dispositivos IoT. 

### <a name="next-steps"></a>Pasos siguientes

Para explorar aún más las funcionalidades de IoT Hub, consulte la [Guía del desarrollador de Azure IoT Hub][lnk-devguide].


[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md

<!--Image references-->
[1]: ./media/iot-hub-compare-event-hubs/ic195031.png
